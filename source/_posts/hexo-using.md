---
title: 使用 hexo 在github上搭建个人博客
tags: [hexo]
categories: [技术分享]
date: 2019-04-06 00:46:15
---

## hexo 搭建个人博客

​	这两天，突然想搭建一个博客记录一些东西会比较好，但是，自己写的话，时间成本太高，并且，没有啥动力，就想起了之前看过有关 hexo 的文章，就想试试，谁知道，一搭建就是差不多一整天，加上找主题，修改主题，花了差不多一天时间，博客终于上线了；下面，我分享一些如何使用 hexo 搭建属于自己的博客，并且将它部署到 github 上去，部署到 github 上就可以随时随地看博客啦，还能发给朋友看，最爽的是不用花钱就能部署一个博客，多好啊。搭建博客的过程中，我遇到了很多坑，以至于花了我将近一天的时间，时间宝贵啊，下面我分享下搭建的心得吧！

​	hexo 是一款简洁高效的博客框架，使用它你能够快速搭建起一个博客网站，可以从[hexo官网](https://hexo.io/zh-cn)找到使用的教程。



### 环境配置

在分享之前，首先要确定您的系统环境，以及一些必要的工具是否安装好了，下面两个工具是必须的：

1. Git 是用来上传文件到 github 上去的，没有的话可以从 [git 的官网](https://git-scm.com/)下载。
2. node，这个是主要的，因为 hexo 框架是基于 node 搭建的，可以从 [node官网](https://nodejs.org/en) 下载。



安装完成之后，可以用以下命令检测以下：

```
$ git --version
git version 2.20.1.windows.1

$ node --version
v8.11.3
```

如果能够显示出对应的版本号，就代表安装成功了。



### 安装 hexo

既然环境已经配置好了，没有啥问题了，那就进入正题吧，安装 hexo，用以下的命令安装：

```
$ npm install hexo-cli -g
```

npm 是 node 自带的node 包管理工具，只要安装了 node 就会附带，`-g` 参数是全局安装，也就是说安装完成之后，无论在哪个目录下，都能够使用 `hexo` 命令了。

```
$ hexo version
hexo-cli: 1.1.0
os: Windows_NT 10.0.16299 win32 x64
http_parser: 2.8.0
node: 8.11.3
v8: 6.2.414.54
uv: 1.19.1
zlib: 1.2.11
ares: 1.10.1-DEV
modules: 57
nghttp2: 1.32.0
napi: 3
openssl: 1.0.2o
icu: 60.1
unicode: 10.0
cldr: 32.0
tz: 2017c
```

使用 `hexo version` 看看 hexo 是否安装成功。

安装成功之后，接下来就是创建一个博客了，选定一个目录，在这个目录下执行：

```
$ hexo init blog # blog 是文件夹名字，不一定要叫这个名字
```

如果命令执行成功的话，会在当前目录下创建一个名字叫 `blog` 的目录，

```
$ cd blog       # 进入 blog 的目录
$ dir 
_config.yml     # 配置文件
node_modules    
package.json  
scaffolds       # 博客的模板
source          # 写博客的地方
themes          # 存放博客的主题的地方
yarn.lock     
```

接下来使用：

```
$ hexo server
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

就能够启动一个服务器了，现在打开浏览器，输入`http://localhost:4000` ，就能够看到你的博客网站了。

![hexo 默认的主题](<https://i.loli.net/2019/04/06/5ca8531e63c76.png>)

这是默认的主题，我们可以从[hexo 官网](https://hexo.io/themes)找到好看的主题，然后替换。



##### _config.yml

```
...
title: Hexo                # 这是网站的标题
subtitle:				   # 这是网站的副标题 
description:			   # 网站的描述
keywords:                  # 网站 SEO 的关键字
author: John Doe           # 网站的作者
language:                  # 语言
timezone:                  # 时区
...
```

配置文件还有太多能配置的了，就不一一赘述了，切换主题主要是修改根目录下的 **_config.yml** 这个配置文件，有的主题，直接修改主题下的配置文件也能够生效，修改根目录的主题就是全局的，下面来换一个主题。

​	先找到一个喜欢的主题，然后使用将他下载下来放到 `.../blog/themes` 目录下，如果是压缩包，需要解压成文件夹（其实每个主题都有对应的安装说明的）。下面以 `next` 主题为例： 

```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

用 git 从github 上将主题克隆到本地的 `themes/next` ，命令执行完成之后会在 `themes` 目录下看到 `next` 目录，这个目录下就是 `next` 主题的文件了。

> 注： 其实每个主题都会有安装的说明，到这其实就可以按照主题的安装说明来配置了，不过有些相同的步骤，还是介绍一下吧！

​	主题下载好了之后，我们用文本编辑器打开 `blog` 目录下的 `_config.yml` 文件；

修改：

```
theme: landscape # 默认主题是  landscape
```

为：

```
theme： next     # 记得冒号之后有一个空格，没有空格可能会报错。
```

修改完成之后，可以启动服务器，看看效果：

```
$ hexo server
```

服务器启动后，就能够看到你喜欢的主题啦，但是，此时还只不过是开始而已。

### 配置 分类 标签 关于 页

一般主题都会提供 `标签` 、`分类`、`关于` 这三个页面的，下面我们来看看如何配置吧：

首先配置分类：

```
$ hexo new page categories
```

命令执行完毕，会在 `source` 目录下创建 `categories` 目录，这个目录下有 `index.md` 文件。

这个文件就是分类的页面了，不过还要配置一下才能使用。

将文件中的内容改为：

```
---
title: categories                 # 显示的标题
date: 2019-04-06 01:38:38         # 文件创建的日期
type: "categories"                # 类型
layout: "categories"              # 最重要的就是这个 layout 了
---
```



然后再在主题的目录下（`blog/themes/next`），修改配置文件（`_conifg.yml`） 其中的：

```yaml
menu:
  home: / || home
  #about: /about/ || user
  #tags: /tags/ || tags
  #categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
```

前面带 `#` 号的代表被注释了，不会生效；只需要将 `#` 号删除，然后重启服务器，就能够看到效果啦，像 `tags` 、`about` 这些，都是一样的步骤，重复上面的步骤即可。



配置好了这些以后，就可以愉快的写文章了，写文章可以使用（默认是用`scaffolds` 下的 `post.md` 来生成模板）：

```
$ hexo new <文章标题>
```

运行上面的命令之后，会在 `source/_posts`目录下生成相应的文件，编辑这个文件即可，有一些参数，可以从 hexo 的官网去看，像一些基础的：

```
---
title: 测试hexo
date: 2019-04-05 16:22:39
tags: [hexo,测试]
categories: [技术学习，生活分享]
---
```

需要配置在文章的最上方，会被解析，更多详细参数可以看对应主题的说明，也可以看官网的说明，有一些参数是通用的，想 `title、date、tags、categories`这些，是通用的，几乎每个主题都有这个几个选项。



> 配置到这，就差不多能够在您本地愉快的写文章啦，但是本地写的文章，又不能给别人看，多没意思，我们可以将它发布到`github`上去。



### 发布到 github

首先，需要一个 github 账号，这就不用介绍了吧，没有的话可以[注册](https://github.com/join?source=header-home)一个。

注册完成之后，就登录吧！



![新创建的 github](https://i.loli.net/2019/04/06/5ca8531bf41f9.png)



登录之后，就是这样子的了，什么也没有。

首先，我们先创建一个仓库吧，点击 `Create a repository ` 创建一个仓库。仓库的名字就是你的用户名为为前置，像这样，我这个账号叫做 `bloglearn` ，那么仓库名字就叫做 `bloglearn.github.io`，当然，也可以不叫这个名字，如果仓库的名字叫做`blog` 就需要通过 `https://bloglearn.github.io/blog` 来访问，这个又长又臭，所以，我们创建的仓库名就叫做 `bloglearn.github.io` ，也就是您的`用户名`加上`.github.io` 作为仓库的名字，我们就能够通过 `https://<您的用户名>.github.io` 直接访问博客了。



![创建仓库](https://i.loli.net/2019/04/06/5ca85319255b9.png)

创建好之后：

![创建仓库完成](https://i.loli.net/2019/04/06/5ca8548068ab2.png)

它会有一个指示，教您怎么做，可以将您的本地的仓库上传到 github 上。



首先，我们在本地的博客目录（`…/blog`），也就是上面创建的 `blog` 目录下，执行：

```
$ git init
```

这个命令是将 blog 这个目录初始化为一个仓库。

到这一步，就差不多了。



### 发布博客

可以从[hexo官网](https://hexo.io/zh-cn/docs/deployment)看到如何发布。

在 `blog` 目录下执行：

```
$ npm install hexo-deployer-git --save
```

安装 `hexo-deployer-git` 插件。

然后修改根目录下的配置文件（`blog/_config.yml` ），修改为如下：

```
deploy:
  type: git
  repo: <repository url> #https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch] #published
  message: [message]
```

| `repo`    | 库（Repository）地址                                         |
| --------- | ------------------------------------------------------------ |
| `branch`  | 分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。 |
| `message` | 自定义提交信息 (默认为 `Site updated:  now('YYYY-MM-DD HH:mm:ss') `) |

如果没有的话，就增加上去吧，如果有的话就修改吧，如果有两个的话，会报错的。

首先是这样配置的：

```
deploy:
  type: git 
  repo: https://github.com/bloglearn/bloglearn.github.io.git
  branch: hexo
```

记得，deploy 下面的每行都必须`缩进`，不然会报错，使用https是需要输入密码来验证的，没有登录过的话，可能无法登录。

#### 生成 SSH 密钥对

```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"  # -C 是加上注释文字
```

然后将公钥，也就是在`%USERPROFILE%/.ssh` （`%USERPROFILE%` 是系统环境变量）目录下，有刚刚创建的文件，以`.pub` 为后缀的，就是公钥了，将公钥问价打开，复制公钥，一会再在 github 上粘贴。

#### 创建 Deploy keys

#### 1. 打开 blog 存放的仓库

![创建步骤](https://i.loli.net/2019/04/06/5ca85319ad95c.png)



#### 2. 先输入一个标题，用于区分公钥串，再将刚刚复制的公钥串粘贴上去。

![填写 deploy keys](https://i.loli.net/2019/04/06/5ca854afd2485.png)

#### 3. 添加完成

![添加完成](https://i.loli.net/2019/04/06/5ca854d5726f4.png)

#### 4. 测试

```
$ ssh -T git@github.com 
Hi bloglearn/bloglearn.github.io! You've successfully authenticated, but GitHub does not provide shell access.
```

如果这个测试显示的是成功认证，那么就是成功的，反之则失败了，就得找找问题了。

##### 如果有多个密钥串怎么办？

可以指定使用某个密钥串来连接的。

```
$ ssh -i <指定密钥串的路径> -T git@github.com 

$ ssh -i %userprofile%/config/.ssh/id_github -T git@github.com      # 我的私钥文件是id_github
PTY allocation request failed on channel 0
Hi bloglearn/bloglearn.github.io! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```



### 上传 blog 到 github 上

#### 准备

如果有多个 **SSH**密钥的话，可以通过在`.ssh` 目录下新建一个 `config` 文件，来管理不同的密钥。

##### config 文件内容：

 ```config
Host blog                                            # 这个是一会要用的
HostName github.com                                  # 这个是主机名，也就是 git@github.com 的后面部分
User git											 # 用户名，也就是 git
IdentityFile ~/.ssh/id_github                        # 然后就是对应的 私钥文件路径了

Host dev                                             
HostName github.com                                  
User git											 
IdentityFile ~/.ssh/id_dev                       
 ```

当有多个的时候，就像上面那样写就可以了



然后修改配置文件（`_config.yml`），修改如下（`注意缩进`）：

```
deploy:
  type: git 
  # 方法1：
  repo: git@github.com:bloglearn/bloglearn.github.io.git
  
  # 方法2：
  repo: blog:bloglearn/bloglearn.github.io.git 
  branch: hexo
```

> 可以看到，repo 从原来的 https 改为了 `git@github.com:bloglearn/bloglearn.github.io.git`，但是由于我配置了多个密钥，所以需要指定，如果没有配置多个密钥的，可以使用`方法1`，配置了多个密钥的使用`方法2`，方法2，由`方法1`，的 `git@github.com:<github 用户名>/<仓库名>.git` 修改为了 `blog:bloglearn/bloglearn.github.io.git ` 这里前面的 `blog` 就是 `.ssh/config` 文件内的 `Host` 字段，然后后面的一样，只是用 `Host` 替换了 `git@github.com` 。



#### 上传

一切都准备好了，那就开始上传吧！切换回到 blog 目录下，执行下面的命令：

```
$ hexo clean 
```

清空掉之前生成的文件，像`public，db.json` 这些文件和目录。然后使用：

```
$ hexo g           # 等同于 hexo generate
```

```
$ hexo d           # 等同于 hexo deploy
```

其实，可以使用 `hexo d` 即可完成 `hexo g` 的工作了。

等一小会，就上传完成了，如果网速慢的话，那就多等会。



#### 完成

如果上面的步骤都没有问题的话，那就已经完成啦！可以打开 <`you_username`>`.github.io`， 像我的就是：https://bloglearn.github.io/



> 写作的话是在 `source` 目录下的 `_posts` 目录下。当然，也可以使用命令：`hexo new <文件名>` ，它会根据模板为您在`blog/source/_posts` 目录下生成相应的文件，您可以修改其内的参数。

有关写作的一些参数可以看看这里 <https://hexo.io/zh-cn/docs/front-matter>





#### 相关

使用：

```
$ hexo new draft <filename> 
```

会在 `source` 目录下创建`_drafts` 目录，这个目录是用来打草稿的，其内的文章在发布的时候不会显示在页面上。

当执行：

```
$ hexo publish <filename>       # filename 不需要加后缀
```

它会将对于的文章移动到`source/_posts` 目录下，进行发布。



#### 图床使用

可以使用[PicGo](https://picgo.github.io/PicGo-Doc/zh/guide/#%E5%BA%94%E7%94%A8%E8%AF%B4%E6%98%8E)



[相关的命令](https://hexo.io/zh-cn/docs/commands)

[主题](https://hexo.io/themes/)



#### 参考文章：

[ssh命令指定密钥连接远程主机](https://www.ktanx.com/blog/p/4016)

[多个密钥管理多个github/gitlab](https://www.cnblogs.com/logchen/p/10543808.html)