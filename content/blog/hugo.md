+++
title = "Hogo技术文档"
date = "2023-07-13T21:19:28+08:00"

#
# description is optional
#
# description = "An optional description for SEO. If not provided, an automatically created summary will be used."

tags = []
+++

## 目录
* [MacOS搭建Hugo进行Github托管](#hugo01)
* [Windows搭建Hugo](#hugo02)
* [在Hugo中嵌入视频](#hugo03)
* [Markdown超链接在新窗口中打开](#hugo04)
* [Hugo中调整Markdown字体大小](#hugo05)
* [在Hugo中锚点跳转的应用](#hugo06)

---


{{< anchor id="hugo01" >}}
## MacOS搭建Hugo进行Github托管

**1.搭建环境**

MacOS下需要安装Homebrew，其它操作系统访问[https://gohugo.io/installation](https://gohugo.io/installation/ "Title") 查询安装文档。

安装Homebrew
```
##安装时如弹出443错误,则需将终端配置网络代理
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```
##终端代理配置格式
export https_proxy=http://127.0.0.1:7890
```
安装hugo并进行配置
```
##安装Hugo
brew install hugo
```

```
##创建blog
hugo new site blog
```

```
##初始化仓库
git init
```

```
##下载主题,也可访问https://themes.gohugo.io/ 获取其它主题
git submodule add https://github.com/jinyekey/hugo-y.git themes/hugo-y
```
使用VScode打开创建的blog文件夹，找到hogo.toml 设置主题
```
baseURL = '设置域名'
languageCode = 'en-us'
title = 'y Site'
theme = 'hugo-y'
```
运行hogo
```
##网站预览地址http://127.0.0.1:1313
hugo server -D
```
创建文章
```
hugo new blog/test.md
```

**2.托管到Github**

登陆Github-创建新存储库，存储库中点击Settings-Pages, Source项选择GitHub Actions点击browse all workflows 找到Hugo点击Configure，再点击Commit changes...提交更改。

使用Github Desktop将本地blog文件夹push到存储库

[Github Desktop下载](https://desktop.github.com/ "Title")

使用Github Pages提供的二级域名便可访问托管在Github上的Hogo网站


**3.托管到Cloudflare Pages**

登陆Cloudflare点击Workers和Pages，创建应用程序-Pages-连接到Git，选择之前创建的Github存储库-开始设置。

在 Pages 项目 >设置>环境变量中创建环境变量。将值设置为当前使用的Hugo版本。

```
##查看当前Hugo版本
hugo version
```

设置环境变量
* 变量名称:&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;HUGO_VERSION
* 值:&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;0.115.1

---

{{< anchor id="hugo02" >}}
### Windows搭建Hugo

1.下载Hugo:  
[访问Hugo的GitHub发布页面](https://github.com/gohugoio/hugo/releases "Title")

2.解压:  
下载的 .zip 文件，得到一个 hugo.exe 文件

3.配置环境变量:
* 将 hugo.exe 文件移动到你希望放置的目录（例如 D:\hugo）。
* 右键点击“此电脑”或“计算机”，选择“属性”。
* 选择“高级系统设置”，然后点击“环境变量”。
* 在系统变量部分，找到“Path”变量，选择“编辑”。
* 点击“新建”，然后添加 Hugo 的路径（例如 D:\hugo）。
* 点击“确定”保存更改。

4.验证安装:  
* 打开命令提示符（CMD）或 PowerShell。
* 输入 hugo version，如果显示版本信息，说明安装成功。

5.创建Hugo站点  
在命令提示符或 PowerShell 中，导航到你想要创建站点的目录。
```
cd D:\hugo
hugo new site my-site
```
my-site 是你站点的名称。

6.选择主题:  
* 访问 [Hugo Themes](https://github.com/gohugoio/hugo/releases "Title")，选择一个你喜欢的主题。
* 根据主题的文档，将其克隆或下载到你的站点的 themes 目录下。例如：
```
cd D:\hugo
git clone https://github.com/theocean/beautiful-hugo.git themes/beautiful-hugo
```

7.配置站点:  
* 打开 config.toml 文件（在 my-site 目录下）。
* 按照主题的说明配置 config.toml 文件，设置站点标题、描述、主题等。

8.创建内容:  
```
cd D:\hugo
hugo new posts/my-first-post.md
```

9.启动本地服务器:  
在 my-site 目录下，使用 hugo server 命令启动本地开发服务器。
```
hugo server
```
打开浏览器，访问 http://localhost:1313，可以查看你的站点预览效果。


---

{{< anchor id="hugo03" >}}
## 在Hugo中嵌入视频
**1.在 /layouts/shortcodes/ 目录下创建youtube.html 并粘贴以下代码**
```
<div class="embed video-player">
  <iframe 
          class="youtube-player" 
          type="text/html" 
          width="640" 
          height="385"
          src="https://www.youtube.com/embed/{{ index .Params 0 }}?autoplay=1"
          style="
                 position: absolute; 
                 top: 0; 
                 left: 0; 
                 width: 100%; 
                 height: 100%; 
                 border:0;"
          allowfullscreen frameborder="0">
  </iframe>
</div>
```

**2.创建一个markdown 嵌入youtube视频参数为：**
```
##xxx为youtube视频链接watch?v=后面的数值
##编辑时将@删除
{@{< youtube xxx >}}

##自动播放
{@{< youtube id="xxx" autoplay="true" >}}
```

**效果:**
{{< youtube eIzqQJqyZr4 >}}


### 嵌入Bilibili视频
**1.在 /layouts/shortcodes/ 目录下创建bilibili.html 并粘贴以下代码**
```
<!DOCTYPE HTML>
<html>

  <head>
    <!-- style 样式 是为了让网页上的视频框按比例显示而非固定的大小 -->
    <style type="text/css">
      .aspect-ratio {
        position: relative;
        width: 100%;
        height: 0;
        padding-bottom: 75%;
      }

      .aspect-ratio iframe {
        position: absolute;
        width: 100%;
        height: 100%;
        left: 0;
        top: 0;
      }
    </style>
  </head>

  <body>
    <div class="aspect-ratio">
      <iframe
              src="https://player.bilibili.com/player.html?bvid={{.Get 0 }}&page={{ if .Get 1 }}{{.Get 1}}{{ else }}1&high_quality=1&danmaku=0{{end}}"
              scrolling="no" 
              border="0" 
              frameborder="no" 
              framespacing="0" 
              allowfullscreen="true"
              sandbox="allow-top-navigation allow-same-origin allow-forms allow-scripts"
              >
      </iframe>
      <!-- src 中的 &high_quality=1&danmaku=0 设定了高清程度并默认屏蔽弹幕 -->
      <!-- sandbox 阻止了点击视频中的按钮跳转到B站的行为 -->
    </div>
  </body>

</html>
```
**2.创建一个markdown 嵌入bilibili视频参数为：**
```
##xxx为bilibili视频链接video/后的数值
##编辑时将@删除
{@{< bilibili xxx >}} 

##指定集数
{@{< bilibili xxx 8 >}}
```

**效果:**
{{< bilibili BV13t411S77V >}} 


---

{{< anchor id="hugo04" >}}

## Markdown超链接在新窗口中打开

**新建目录layouts/_default/_markup/ 并创建render-link.html**

***##粘贴以下参数***

```
<a href="{{ .Destination | safeURL }}"{{ with .Title}} title="{{ . }}"{{ end }}{{ if strings.HasPrefix .Destination "http" }} target="_blank" rel="noopener"{{ end }}>{{ .Text | safeHTML }}</a>
```

```
[链接](https://www.github.com "Title")
```

---

{{< anchor id="hugo05" >}}

## Hugo中调整Markdown字体大小

在 layouts/shortcodes 目录下创建一个名为 fontsize.html 的文件，内容如下：
```
<span style="font-size: {{ .Get "size" }};">{{ .Inner }}</span>
```

然后在Markdown中这样使用：
```
##编辑时将@删除
{@{< fontsize size="20px" >}}这是20像素的文本。{@{< /fontsize >}}
```

---

{{< anchor id="hugo06" >}}

## 在Hugo中锚点跳转的应用

Hugo常规的锚点跳转Markdown语法为：

1.  
```
# 目录
* [目录一](#标题)
```

2.  
```
## 标题
//跳转目的地
```


而锚点跳转到页面指定位置，并非某段文章标题时，可以使用Hugo Shortcodes，操作如下：

***1.创建Shortcode文件***

Hugo项目的根目录下，找到或创建一个layouts/shortcodes 目录。

***2.编辑Shortcode文件***

编辑或创建anchor.html文件，并添加如下内容：
```
<a id="{{ .Get "id" }}"></a>
```
Shortcode会生成一个带有指定ID的锚点

***3.在Markdown文件中插入Shortcode***

在Markdown文件中，使用Shortcode来插入跳转到目的地的锚点：

```
##编辑时将@删除
{@{< anchor id="display" >}@}
```

这行代码会在生成的HTML中插入一个带有ID:display的锚点。

***4.链接到锚点***

在Markdown文件中的其他位置创建链接，指向这个锚点：
```
[跳转到锚点](#display)
```

***5.完整示例***

```
## 目录

- [跳转到锚点](#display)

## 显示部分

{{< anchor id="display" >}}
这里是显示部分的内容。
```
