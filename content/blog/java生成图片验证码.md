---
title: "Java生成图片验证码"
date: 2018-04-22T15:49:23+08:00
tags: ["java"]
draft: false
---

```Java
public BufferedImage createVerifyCode() {

    int width = 80;
    int height = 32;
    // 创建图片
    BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
    Graphics g = image.getGraphics();
    // 设置背景颜色
    g.setColor(new Color(0xDCDCDC));
    g.fillRect(0, 0, width, height);
    // 边框
    g.setColor(Color.BLACK);
    g.drawRect(0, 0, width - 1, height - 1);
    // 生成随机数
    Random rdm = new Random();
    // 生成50个点
    for (int i = 0; i < 50; i++) {
        int x = rdm.nextInt(width);
        int y = rdm.nextInt(height);
        g.drawOval(x, y, 0, 0);
    }
    // 生成验证码
    String verifyCode = generateVerifyCode(rdm);
    g.setColor(new Color(0, 100, 0));
    g.setFont(new Font("Candara", Font.BOLD, 24));
    g.drawString(verifyCode, 8, 24);
    g.dispose();
    // 把验证码存到redis中
    int rnd = calc(verifyCode);
    return image;
}

/**
 * 计算得数
*/
private int calc(String exp) {
    try {
        ScriptEngineManager manager = new ScriptEngineManager();
        ScriptEngine engine = manager.getEngineByName("JavaScript");
        return (int) engine.eval(exp);
    } catch (Exception e) {
        e.printStackTrace();
        return 0;
    }
}

private static char[] ops = new char[]{'+', '-', '*'};

/**
 * 随机进行+ - *
*/
private String generateVerifyCode(Random rdm) {
    int num1 = rdm.nextInt(10);
    int num2 = rdm.nextInt(10);
    int num3 = rdm.nextInt(10);
    char op1 = ops[rdm.nextInt(3)];
    char op2 = ops[rdm.nextInt(3)];
    String exp = "" + num1 + op1 + num2 + op2 + num3;
    return exp;
}
```
