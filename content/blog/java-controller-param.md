---
title : "Spring Controller方法自定义参数"
date : "2018-04-26T23:13:45+08:00"
description : "给controller方法添加自定义参数"
tags : [
  "java",
  "spring",
]
---
&emsp;&emsp;WEB应用一般在用户登录后，我们都会保存用户信息到session里面或者到redis里面，如果在请求方法中想使用就要通过session或者redis的获取，每个一个请求需要都需要重复编写这样的代码，造成代码冗余.

&emsp;&emsp;SpringMVC给我提供了一个接口能给Controller层的接口方法注入自定义参数，我们就可以在这里做文章，给接口参数注入用户的实例。

> 文中的`@Data`是使用了[lombok](https://www.projectlombok.org/)插件

1. 编写User类

    ```java
    @Data
    public class User {

        private int id;

        private String name;
    }
    ```

2. 创建对象`UserContext`负责存取`User`的实例对象，容器使用的是_ThreadLocal_ ， _ThreadLocal_ 的关键每个线程都会拥有一个 _ThreadLocal_ 的对象独享，线程之间不会共享一个 _ThreadLocal_ ，换句话说就是一个 _ThreadLocal_ 实例对象当且仅当属于某条线程。

    ```java
    public class UserContext {

        private static ThreadLocal<User> userHolder = new ThreadLocal<>();

        // 存储user对象
        public static void setUser(User user) {
            userHolder.set(user);
        }

        // 获取user对象
        public static User getUser() {
            return userHolder.get();
        }
    }
    ```

3. 编写常量类

    ```java
    public class Consts {

      // 获取cookie中或者参数中token的标识
      public static final String TOKEN_NAME = "token_name";
    }
    ```

4. 实现拦截器`HandlerInterceptorAdapter`，实现方法`preHandle`，以便于在请求处理前将`User`的实例对象存储在`UserContext`中。

    > `preHandle`：方法将在请求处理之前进行调用

    ```java
    @Component
    public class UserParamInterceptor extends HandlerInterceptorAdapter{

        @Autowired
        private User miaoshaUserService;

        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            if (handler instanceof HandlerMethod) {
              // 获取user对象
              User user = getUser(request, response);
              // 用UserContext储存user对象
              UserContext.setUser(user);
            }
            // 返回值为true请求继续执行，返回值为false的时候请求结束
            return true;
        }

        /**
         * 通过参数或者cookie中的token，获取user
         */
        private User getUser(HttpServletRequest request, HttpServletResponse response) {

            // 通过参数获取token
            String paramToken = request.getParameter(Consts.TOKEN_NAME);
            // 通过cookie获取token
            String cookieToken = getCookieValue(request, Consts.TOKEN_NAME);
            // 如果cookie和param中都不存在token，说明用户还没登录，所以返回空
            if (StringUtils.isEmpty(cookieToken) && StringUtils.isEmpty(paramToken)) {
                return null;
            }
            String token = StringUtils.isEmpty(paramToken) ? cookieToken : paramToken;
            // 用户通过token获取用户对象，具体逻辑，可以自己实现UserService编写逻辑，一般是通过session或者redis获取，这取决于你在登录的时候user对象存在哪里
            return userService.getUserByToken(token);
        }

        /**
         * 通过客户端传来的cookie获取token
         */
        private String getCookieValue(HttpServletRequest request, String cookieNameToken) {

            Cookie[] cookies = request.getCookies();
            if (StringUtils.isEmpty(cookies)) { // 判断cookies是否为空
                return null;
            }
            for (Cookie cookie :
                    cookies) {
                if (cookie.getName().equals(cookieNameToken)) {
                    return cookie.getValue();
                }

            }
            return null;
        }
    }
    ```

5. 实现Spring自定义参数解析器`HandlerMethodArgumentResolver`接口，并且实现接口的两个方法`supportsParameter`、`resolveArgument`。

    > `supportsParameter`：用来判断参数需不需要被解析，如果是返回true则需要，false则相反。
    >  `resolveArgument`：当 _supportsParameter_ 方法返回true时，就会调用此方法，返回的对象就是Controller方法上的需要注入参数

    ```java
    @Component
    public class UserArgumentResolver implements HandlerMethodArgumentResolver {

        @Override
        public boolean supportsParameter(MethodParameter methodParameter) {
            // methodParameter.getParameterType()获取方法上的所有参数类型，当参数类型等于User对象的时候返回true，以调用resolveArgument
            Class<?> clazz = methodParameter.getParameterType();
            return clazz == User.class;
        }

        @Override
        public Object resolveArgument(MethodParameter methodParameter, ModelAndViewContainer modelAndViewContainer, NativeWebRequest nativeWebRequest, WebDataBinderFactory webDataBinderFactory) throws Exception {

            return UserContext.getUser();
        }
    }
    ```

6. 配置`WebMvcConfigurerAdapter`

    ```java
    @Configuration
    public class WebConfig extends WebMvcConfigurerAdapter {

        // 注入刚刚编写的userArgumentResolver
        @Autowired
        private UserArgumentResolver userArgumentResolver;

        // 注入刚刚编写的userParamInterceptor
        @Autowired
        private UserParamInterceptor userParamInterceptor;

        /**
         * 给spring添加参数解析器userArgumentResolver
         */
        @Override
        public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
            argumentResolvers.add(userArgumentResolver);
        }

        /**
         * 给spring添加拦截器userParamInterceptor
         */
        @Override
        public void addInterceptors(InterceptorRegistry registry) {
            registry.addInterceptor(userParamInterceptor);
        }
    }
    ```

7. 以上就完成了springMVC参数的注入，下面举个使用的栗子，当登录完成后，客户端调用`host:port/demo/user/get`
    不需要传任何参数，就会返回用户对象user

        ```java
        @Controller
           @RequestMapping("/demo")
           public class DemoController {

               @RequestMapping("/user/get")
               @ResponseBody
               public User getUser(User user) {
                   return user;
               }
           }
        ```
