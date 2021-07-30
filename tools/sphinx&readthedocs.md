# Sphinx和ReadtheDocs

了解如何使用Sphinx+ReadtheDocs给自己的项目添加文档。主要参考的资料有：

- [Getting Started with Sphinx](https://docs.readthedocs.io/en/stable/intro/getting-started-with-sphinx.html)
- [Introduction to Sphinx](https://www.writethedocs.org/guide/tools/sphinx/)
- [reStructuredText 语法](https://3vshej.cn/rstSyntax/)
- [An idiot’s guide to Python documentation with Sphinx and ReadTheDocs](https://samnicholls.net/2016/06/15/how-to-sphinx-readthedocs/)
- [Awesome Sphinx (Python Documentation Generator)](https://github.com/yoloseem/awesome-sphinxdoc)
- [如何用Sphinx 、GitHub 、ReadtheDocs、搭建写书环境](https://wtf.readthedocs.io/en/latest/)
- [使用ReadtheDocs给项目添加教程文档](https://www.iamlightsmile.com/articles/%E4%BD%BF%E7%94%A8ReadtheDocs%E7%BB%99%E9%A1%B9%E7%9B%AE%E6%B7%BB%E5%8A%A0%E6%95%99%E7%A8%8B%E6%96%87%E6%A1%A3/)
- [如何给自己的 python 项目添加免费在线文档 ?](https://juejin.im/post/6844903955059884046)

Sphinx 是一个文档生成工具，可以把自己写的markdown或者RST的文件转成html以供在浏览器端显示，本地可以安装它来写文档测试生成html

ReadtheDocs 是一个在线文档托管服务，可以从各种版本控制系统中导入文档，那么每次提交代码后可以自动构建并上传至readthedocs网站，然后github项目的md文件只需给个链接即可访问到该文档。

简而言之，就是自己在自己代码项目里写RST文件，用本地的Sphinx来使对文档做配置，也可以预览自己的文件效果，然后上传到github，github和 ReadtheDocs 联系起来，ReadtheDocs就会自动监测github的更新，然后会自动生成html文档并给地址链接，将链接放到自己github项目的md文件下即可。

## 基本配置

首先本地安装使用sphinx。

直接使用conda安装即可：

```Shell
# 如果已经使用本文件夹下的 environment.yml 安装过了就不必再执行了
conda install -c conda-forge sphinx
```

可以再安装一些sphinx的相关插件：

```Shell
conda install -c conda-forge sphinx-autobuild sphinx_rtd_theme recommonmark
```

接下来以为 某项目 xxx 添加文档为例说明。

首先进入项目根目录，创建一个专门放文档的文件夹，并进入该文件夹。

```Shell
mkdir docs
cd docs
```

然后执行:

```Shell
sphinx-quickstart
```

这样就进入sphinx的配置。参考的配置如下：

```Shell
> Separate source and build directories (y/n) [n]: y
> Project name: xxx
> Author name(s): XXX
> Project release []: 0.1.0
> Project language [en]: zh_CN
```

windows下直接看目录即可，ubuntu下可以通过以下命令查看文件目录：

```Shell
$ sudo apt install tree
$ tree --dirsfirst

.
├── build
├── source
│   ├── _static
│   ├── _templates
│   ├── conf.py
│   └── index.rst
├── Makefile
└── make.bat
```

可以看到：

- index.rst文件：这是文档的索引文件，可以将其视为包含供用户导航到子主题的登录页面。它通常包含一个目录，用于链接文档中的其他主题；
- conf.py：允许自定义输出。在大多数情况下，这不需要更改；
- Makefile：这个是sphinx附带的，是本地开发的主要接口，不要修改；
- \*.rst 文件：就是各个子主题的文档。

可以build项目来看看效果，docs目录下执行，如果Ubuntu还没有安装make工具，需要安装下：

```Shell
# Ubunutu系统下安装make工具
sudo apt install make 
```

如果是在windows下，则有 make.bat 文件，直接执行make就会运行该文件，不必再安装类似linux下的make工具。

```Shell
# 执行make
make html
```

然后进入build/html目录后用浏览器打开index.html，即可看到效果。

接下来看看如何使用 ReadtheDocs 给项目添加教程文档。

进入[readthedocs官网](https://readthedocs.org/)，注册自己的账号，注册之后验证自己的邮箱，然后 Connect your Accounts 连接到自己的github账号。

在导入github项目到readmedocs之前先将刚刚创建好的docs推送上github，然后再来关联。推送的时候注意在ignore文件中添加build/目录：

```.gitignore
/docs/build/
```

然后推送：

```git
git add -A
git commit -m "xxx"
git push
```

进入ReadtheDocs个人面板，点击Import a Project，然后会刷新出自己github下的public项目，如果项目列表为空或者未显示当前项目，点击右上角等待刷新项目即可。这里我选择elks。然后勾选“编辑项目高级选项”，点击“下一页”。

自己填写内容，这里我的填写：

- Description：xxx项目的中文文档
- 语言: Simplified Chinese

这时候如果就直接点击“Build version”，进行项目构建，可能会报错：

Sphinx error: master file [..]/checkouts/latest/contents.rst not found

根据：[Sphinx error: master file [..]/checkouts/latest/contents.rst not found](https://github.com/readthedocs/readthedocs.org/issues/2569)的说明，需要在conf.py文件中加入：

```Python
master_doc = 'index'
```

这样再重新git push，再尝试即可。

## 使用 Sphinx 写文档

下面看看如何使用 Sphinx 编辑文档。首先大致了解下 .rst 文件的结构。

### 目录结构

在 Sphinx 中指定目录 (TOC) 结构的方法有些不寻常，它不是设置一个包含整个项目的 TOC 层次结构的主文件，而是需要在每个有子主题的父主题中包含 toctree [directives](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#rst-directives)。Sphinx 会从 各文件中的toctree 指令来推断整体的 TOC 结构。

注：一个directive 即指令，就是显示标记的一个通用块，比如要做一个表格，会有一个表格块；要写一个目录，就需要一个目录块，也就是 toctree directive ，就是下面看到的 .. toctree::

例如，项目文件夹中的index.rst文件可能包含以下 toctree 指令：

```rst
.. toctree::

   TopLevel1
   TopLevel2
```

这表明有两个顶层主题。如果希望 TopLevel1主题包含子主题，则应在 TopLevel1 中插入以下toctree指令：

```rst
.. toctree::

   Child1
   Child2
   Child3
```

不同的 Sphinx 主题将有不同的方式在侧边栏中显示 TOC。关于主题，可以参考[这里](https://github.com/readthedocs/sphinx_rtd_theme)。

### 编写文档

编写文档的位置将根据项目的布局方式而有所不同。通常，主要主题将放在最上层 docs 目录中一个恰当命名的文件中。如果主题变大，则可以将其分解为目录中的多个文件。当你写一个文档时，想好它在项目中的位置是比较好的。

如果创建一个新文件，请确保它包含在TOC中的某个文件的 toctreeTOC 指令中。因为当构建文档时，Sphinx 将为不在 TOC 中的每个文档显示警告。

要编写漂亮的文档，需要对 reStructuredText (RST) 语言有基本的了解。[reStructuredText Primer](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html)中涵盖了大部分常用语法。

主要包括：

- 段落：RST文档中最基本的块，由一个或多个空行分隔的文本块。注意 RST 缩进很重要，同一段落的所有行都必须左对齐到相同的缩进级别；
- 内联标记：和markdown一样，一个星号是斜体，两个是粗体
- 列表：和markdown类似，不过是使用 星号（无序），数字（顺序）
- 文字块：通过用特殊标记::结束段落来引入。引入时，必须缩进
- Doctest块：>>> 开头的代码块
- 表格：比markdown稍微麻烦一点，可以参考[这里](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#grid-tables)
- 超链接：'x链接名 <链接地址>'_  
- 部分：长串的符号包括的部分称为section，! " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ ` { | } ~ 这些符号都可以
- 域列表：和python代码中的注释里写参数意思的时候一样，形如 :fieldname: Field content
- 显示标记：用于需要特殊处理的地方。行首由 .. 开始，然后紧跟着空格，一直到有相同缩进的下一段结束。指令就是显示标记的一个块。有很多，可以参考[这里](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#directives)

可以在网络上实时预览 RST：http://rst.ninjs.org/ 。不过它不能理解 Sphinx 特定的标记。

开始RST前，可以先用这个实时预览的工具随意尝试一下，以确保了解它是如何工作的。

Sphinx文档也支持markdown，但是[不推荐使用](https://www.ericholscher.com/blog/2016/mar/15/dont-use-markdown-for-technical-docs/)。如果想要自动转换 markdown 到 reStructuredText，可以试试[这个工具](https://pandoc.org/index.html)

### API文档

代码的文档是从代码注释中自动导出的，所以需要在配置 sphinx-quickstart 时设置 autodoc: automatically insert docstrings from modules (y/n) [n]: y  

如果没设置，则可以在 docs文件夹下面的 source/conf.py 中的extensions 中添加 'sphinx.ext.autodoc'：

```Python
extensions = [
    'sphinx.ext.autodoc'
]
```

然后将命令行切换到docs目录下，执行以下命令即可：

```Shell
sphinx-apidoc -o source/ ../xxx（想要生成API文档的文件夹名）/
```

这样就生成注释对应的API文档的 rst 文件了，接着记得将生成的 module.rst 文件加入到 index.rst 的目录树中，这样就将注释文件纳入到文档体系里了。

如果不太清楚代码注释、文档应该怎么写，可以参考[这里](https://render.githubusercontent.com/view/ipynb?color_mode=dark&commit=c3e90de15ac3ad144023c892a42e8ec37bab3fea&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f7761746572444c75742f6879647275732f633365393064653135616333616431343430323363383932613432653865633337626162336665612f312d6c6561726e2d707974686f6e2f322d707974686f6e2d636f6e636570742e6970796e62&nwo=waterDLut%2Fhydrus&path=1-learn-python%2F2-python-concept.ipynb&repository_id=213807817&repository_type=Repository#python%E4%BB%A3%E7%A0%81%E6%96%87%E6%A1%A3)

然后就可以利用make工具构建文档生成html文件了。

### 构建文档

编写了文档后，我们希望将其转换为 HTML，这非常简单，只需运行make语句即可。

不过要注意，根据[这里](https://stackoverflow.com/questions/10324393/sphinx-build-fail-autodoc-cant-import-find-module)的介绍，如果自己在配置的时候选择了 “Separate source and build directories (y/n) [n]: y”，那么这时候，应该在conf.py文件里，将 以下语句修改：

```Python
# import os
# import sys
# sys.path.insert(0, os.path.abspath('.'))
```

修改为

```Python
import os
import sys
sys.path.insert(0, os.path.abspath('../..'))
```

然后再执行make：

```Shell
# Inside top-level docs/ directory.
make html
```

这就会在 shell 中运行 Sphinx，并输出 HTML。最后，它会提示一些关于准备好的文件的信息。现在可以通过键入以下内容在浏览器中打开它们：

```Shell
open build/html/index.html
```

## 更多灵活配置

另外，除了自己手动撰写的部分，其他部分的流程是可以自动化的。
