---
title: "使用hugo来建立自己的博客"
date: 2022-04-11T14:26:46+08:00
draft: false
---

# 使用hugo来建立自己的博客
## 一、hugo初始化博客

```bash
# 安装
brew install hugo
hugo version
# 初始化博客目录
hugo new site /Volumes/Work/blog
cd /Volumes/Work/blog
git init
git config user.name b4c5
git config user.email blaket@qq.com

touch .nojekyll		# 关掉github pages 自带的jekyll
# 使用LoveIt主题，当然也可以用其它主题
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
# 覆盖默认配置
cp themes/LoveIt/exampleSite/config.toml .
vim config.toml # 修改内容参见修改config.toml
hugo -d docs		# 将静态文件生成到docs目录下，与后面的Github Pages Source的设置要对应
hugo server -D	# 在本地运行hugo, 访问：http://localhost:1313/
```

### 修改config.toml

- baseURL，改成网站域名
- 注释themesDir
- 修改title为网站名字

修改后的完整内容见下：

```toml
baseURL = "https://b4c5.github.io"
# [en, zh-cn, fr, pl, ...] determines default content language
# [en, zh-cn, fr, pl, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"
# theme
# 主题
theme = "LoveIt"
# themes directory
# 主题目录
# themesDir = "../.."

# website title
# 网站标题
title = "b4c5"
# 生成HTML的目录， 默认为public
publishDir = "docs"

```
#### 参考
[1] [Configure Hugo | Hugo](https://gohugo.io/getting-started/configuration/)


## 二、配置github Pages

也可以参考：[官方手册](https://docs.github.com/en/pages)

1. 创建仓库b4c5.github.io
   - 仓库的名字，必须是`<username>.github.io`，其中`username`必须是你的github名字。
   - 仓库必须为公开
2. 仓库Pages设置：settings → Pages
   - 页面路径指定（Source）
     - 建议选择：`Branch:main`, `/docs`,这样在该仓库的docs目录下，为网站的根目录
     - 注：根目录放index.html可以验证是否可以访问网站

## 三、ssh添加密钥

1. 生成公私钥，并完成相关配置

```bash
ssh-keygen -t rsa -f id_rsa_github_b4c5	# 生成公私鈅对
vim ~/.ssh/config		# 添加b4c5 ssh配置，内容见后面
```

**b4c5 ssh配置**

```ini
Host github-b4c5
		# 部分需要代理的环境才需要
    ProxyCommand corkscrew 127.0.0.1 12679 %h %p
    Port 22
    # 指定证书，用于b4c5的网站发布
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_github_b4c5
```

2. 在github上，添加ssh公钥
   - settings → SSH and GPG keys → SSH keys → New SSH key
   - Key一栏，填入 id_rsa_github_b4c5.pub的文件内容

经过上述配置后，b4c5的github，使用git的方式为：

`git clone git@github-b4c5:b4c5/b4c5.github.io.git`

## 三、上传github Pages文件

```bash
git remote add origin git@github-b4c5:b4c5/b4c5.github.io.git # 将本地 git 项目与 github 项目相关联
git fetch origin # 拉取 github 项目
git checkout main #切换到主分支 main
git add . 	# 在这一步可能会报警告，处理方法见后面。
git commit -m "init github pages"
git push origin	# 推送成功后，一般10～20分钟后，github page才会正常工作，但也有可能会报错，这些报错的定位，参见后面内容
```

#### **git add . 出现警告的解决方案**

ref: [No url found for submodule path](https://www.deployhq.com/support/common-repository-errors/no-url-found-for-submodule)

**警告内容：**

>hint: You've added another git repository inside your current repository.
>hint: Clones of the outer repository will not contain the contents of
>hint: the embedded repository and will not know how to obtain it.
>hint: If you meant to add a submodule, use:
>hint:
>hint: 	git submodule add <url> themes/LoveIt
>hint:
>hint: If you added this path by mistake, you can remove it from the
>hint: index with:
>hint:
>hint: 	git rm --cached themes/LoveIt
>hint:
>hint: See "git help submodule" for more information.

**根本原因：**

项目根目录不存在`.gitmodules`文件，或者该文件中的submodule没有url。

**解决方法：**

其实就是按提示操作就可以。

```bash
git rm --cached themes/LoveIt
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

**最终效果：**

在项目根目录存在`.gitmodules`文件，该文件内容如下：

```ini
[submodule "themes/LoveIt"]
	path = themes/LoveIt
	url = https://github.com/dillonzq/LoveIt.git
```

#### github Pages 仍然404的原因

在实操中，我因为上订完git submodule的问题，导致使用https://b4c5.github.io访问显示404的情况，这种问题该怎么定位呢？

在github仓库的commit上，可以看到详细的日志。如下图所示：

![image-20220412110101758](https://raw.githubusercontent.com/b4c5/b4c5-images1/main/img/image-20220412110101758.webp)



## 参考

[1] [Hugo官方文档](https://gohugo.io/getting-started/usage/)
