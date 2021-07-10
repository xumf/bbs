---
title : "github静态网页"
date : "2018-04-28T13:54:50+08:00"
description : "github"
tags : [
  "blog",
  "github"
]
---

> 文章下的 **_xumf_** 都要换成你的 **_github用户名_**

### 第一步：创建一个repository

- `xumf.github.io`改为`你的github用户名.github.io`
- Description(可以不填)添加备注
- 点击Create repository

[![git-repository.png](https://s14.postimg.cc/6s4lynw4x/git-repository.png)](https://github.com)
[![fin.png](https://s14.postimg.cc/ih8ln0kn5/fin.png)](https://xumf.github.io/)

### 第二步：上传index.html到刚刚创建的repository上

- clone repository到本地，`$ git clone git@github.com:xumf/xumf.github.io.git`
    [![gitclone.png](https://s14.postimg.cc/ad0hi8vn5/gitclone.png)](https://xumf.github.io/)

- 进入到xumf.github.io.git目录，`$ cd xumf.github.io.git`
    [![cddir.png](https://s14.postimg.cc/qcj51fwq9/cddir.png)](https://xumf.github.io/)
- 添加一个文件并编辑，`$ vim index.html`，编辑完成后`:wq`，保存退出。
- 添加，上传index.html文件到github
    ```bash
    $ git add index.html
    $ git commit index.html -m 'init'
    $ git push origin master
    ```
- 在浏览器打开<https://xumf.github.io/>，就能看到刚刚`index.html`的内容。
    [![xgi.png](https://s14.postimg.cc/lk9bpx9r5/xgi.png)](https://xumf.github.io/)
