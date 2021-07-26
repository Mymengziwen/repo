# 代码仓文档

如果只专注于手头的一点代码工作，并且也没打算后续继续从代码角度使用这些工作成果，那么效率更高的方式就是不写文档，因为对正在做的东西很熟悉，以后不用也不必担心忘记的问题。

但是当面临的复杂问题需要一步步地来推进时，内容上的不断积累就是重要保障了，这时候文档工作就是非常必要的了，尤其是当想要别人使用自己的程序并一起贡献和维护时；而且，无数的实践表明，即便是自己写的程序，过几个月后再看，也跟别人写的没太大区别了，这时候，自己就会想，如果当初把文档写清楚就好了。此外，文档在保障科研成果的可复现性、公开性方面也是十分重要的。

因此，这里参考以下资料，记录下为一个python程序项目创建必要文档时要注意的关键点。

- [How to Write a Good Documentation: Home](https://guides.lib.berkeley.edu/how-to-write-good-documentation)
- [A beginner’s guide to writing documentation](https://www.writethedocs.org/guide/writing/beginners-guide-to-docs/)
- [Read the Docs: Documentation Simplified](https://docs.readthedocs.io/en/stable/index.html)
- [Python最佳实践指南中文版 -- 文档](https://pythonguidecn.readthedocs.io/zh/latest/writing/documentation.html)

## 文档实践建议

一个完整的代码仓文档应该包括：

- README 文件：代码阅读者的主要入口，一般情况下应包括以下内容。
    - 项目简介
    - 安装指令
    - 简短教程
    - 贡献者信息
    - 引用信息
    - 许可证信息
    - 邮件联系方式
    - 版本记录
- 介绍文档：用一两个极其简化的用例，来非常简短地概述该产品 能够用来做什么。这相当于项目的30秒自我陈述（thirty-second pitch）。
- 代码文档：即应用程序接口（API）文档，通常直接从代码产生。它会列出所有的可供公共访问的接口、参数和返回值。
- 开发人员文档：适用于贡献者，可以包括项目的代码惯例和通用设计策略。

另外，涉及到具体代码的时候，还要注意代码规范化，比如文件组织，注释方式，命名规则等。

## 用什么工具

这里以 Python项目为例。主要是两个工具，一个是 Sphinx，一个是 ReadTheDocs。前者帮助我们完成本地的文档工作，后者可以帮助我们将文档发布到网上。

Sphinx 无疑是最流行的Python文档工具。它能把 reStructuredText 标记语言转换为广泛的输出格式，包括HTML、LaTeX(可打印PDF版本)、手册页和纯文本。 reStructuredText 就像是内建扩展功能的 Markdown。

Read The Docs 是一个 超棒的 并且 免费的 文档托管，可以托管 Sphinx 文档。我们可以为它配置提交钩子到您的源库中，这样文档的重新构建将会自动进行。

运行时，Sphinx 将导入代码，并使用Python的内省功能，它将提取所有函数，方法和类签名。 它还将提取附带的文档字符串，并将其全部编译成结构良好且易于阅读的文档。

具体的过程可以参考[这里](https://github.com/waterDLut/WaterResources/blob/master/tools/sphinx%26readthedocs.md)
