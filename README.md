### 介绍

1. sphinx：是一个文档生成器，可以将一组纯文本源文件转换成各种输出格式，并且自动生成交叉引用、索引等。默认使用ReStructuredText格式，也可以安装 recommonmark插件，使用markdown语法，sphinx-markdown-tables插件使用markdown表格语法
2. ReStructuredText：是一种轻量级标记语言，文件后缀.rst
   1. 相比markdown优势项：
   2. 使用sphnix能够自动生成目录和索引文件，方便查询和检索
   3. 有大量漂亮的HTML书籍主题模版，可为书籍轻松换肤（类似Wordpress的网站模版）
   4. 对于参考手册类书籍的编写在语法上更为便利（python官方帮助文档的使用者）
3. Readthedocs: 是一个基于Sphinx 的免费文档托管项目

### 流程

sphinx生成文档样式，推送到GitHub，触发readthedocs自动构建、生成在线文档

### 操作步骤

- 初始化sphinx

```Bash
# 先新建一个文件夹，例如Sphinx_GitHub_ReadtheDocs
# 安装sphinx及相关插件 
pip3 install sphinx sphinx_rtd_theme recommonmark sphinx-markdown-tables sphinxemoji
# 进入Git根目录
cd ~/Sphinx_GitHub_ReadtheDocs
# 开始快速配置sphinx
sphinx-quickstart

# 选择把源文件和删除文件分开（y）> Separate source and build directories (y/n) [n]:y
# 项目名称> Project name: Sphinx_GitHub_ReadtheDocs
# 作者姓名> Author name(s): Tsanfer
# 版本号> Project release []: 0.2
# 语言> Project language [en]: zh_CN
```

目录结构如下：

```Go
.
├── Makefile   # 可以看作是一个包含指令的文件，在使用 make 命令时，可以使用这些指令来构建文档输出
├── build    # 生成的文件的输出目录
├── make.bat  # Windows 用命令行
└── source     
    ├── _static        # 静态文件目录，比如图片等
    ├── _templates        # 模板目录
    ├── conf.py        # 存放 Sphinx 的配置，包括在 sphinx-quickstart 时选中的那些值，可以自行定义其他的值
    └── index.rst        # 文档项目起始文件
```

- 验证配置是否正确：

```Bash
cd ~/Sphinx_GitHub_ReadtheDocs
sphinx-autobuild source build/html
# 默认启动 8000 端口，在浏览器输入 http://127.0.0.1:8000，能访问页面
```

- 修改主题及支持markdown格式

```Bash
# 打开conf.py，修改html_theme和extensions
html_theme = 'sphinx_rtd_theme'
extensions = [
        'recommonmark',
        'sphinx_markdown_tables'
]
```

- 添加markdown文件

```Bash
# 进入source目录，新建文件夹存放.md文档的文件夹，例如documents
# 进入documents,如果有多个二级菜单，则新建多个文件夹，并在其创建.md文件，并且需要在该目录下创建index.rst文件
# 例如结果如下：
documents
├── index.rst
├── privacy
│   └── contents.md
├── validate
│   └── contents.md
├── syn
│   └── contents.md

# 在index.rst文件中添加如下内容：
接口详情
=================================

.. toctree::
:maxdepth: 5

privacy/contents
validate/contents
syn/contents

# 在contents.md文件中填写内容，如：

# 隐私计算

## get_txs

获取区块高度查询区块内的交易

### Parameters

+ `height`: `uint256` 类型 区块高度


### Returns

+ `hash`: `string` 类型，交易哈希

+ `tx_type`: `int32` 类型，交易类型

+ `transaction`: `` 类型，交易信息

+ `signatures`: `` 类型，签名信息

+ `error_code`: `string` 类型，错误码

+ `error_msg`: `string` 类型，错误提示信息

+ `ledger_seq`: `string` 区块高度

+ `close_time`: `` 类型，时间

+ `actual_fee`: `string` 类型，手续费

+ `events`: `string` 类型，事件


## get_tx

### Parameters

+ `hash`: `string` 类型，交易哈希


### Returns

+ `hash`: `string` 类型，交易哈希

+ `tx_type`: `int32` 类型，交易类型

+ `transaction`: `` 类型，交易信息

+ `signatures`: `` 类型，签名信息

+ `error_code`: `string` 类型，错误码

+ `error_msg`: `string` 类型，错误提示信息

+ `ledger_seq`: `string` 区块高度

+ `close_time`: `` 类型，时间

+ `actual_fee`: `string` 类型，手续费

+ `events`: `string` 类型，事件

# 回到source目录，修改index.rst内容：
.. toctree::
:maxdepth: 2

documents/index
```

- 重新启动http服务，查看内容是否正确

```Bash
sphinx-autobuild source build/html
```

- 在根目录创建.readthedocs.yaml，内容如下：

```Bash
# Read the Docs configuration file
# See https://docs.readthedocs.io/en/stable/config-file/v2.html for details

# Required
version: 2

# Set the OS, Python version, and other tools you might need
build:
  os: ubuntu-22.04
  tools:
    python: "3.8"

# Build documentation in the "docs/" directory with Sphinx
sphinx:
   configuration: source/conf.py

# Optionally, but recommended,
# declare the Python requirements required to build your documentation
# See https://docs.readthedocs.io/en/stable/guides/reproducible-builds.html
python:
    install:
    - requirements: source/requirements.txt
```

- 在source目录下创建requirements.txt文件，内容如下：

```Bash
sphinx
sphinx_rtd_theme
recommonmark
sphinx_markdown_tables
```

- 在根目录下新建.gitignore文件，忽略上传目录，内容如下：

```Bash
build/
```

- 在GitHub新建仓库，上传代码

```Bash
git init
git add .
git commit -m "init"
git branch -M master
git remote add origin 仓库连接
git push -u origin master
```

- readthedocs（https://app.readthedocs.org/）上使用GitHub账户登录，添加项目，构建成功后，使用项目名称.readthedocs.io/访问

 示例：https://sphinx-demo-test-geno.readthedocs.io/

![img](https://kcnwpmm8llug.feishu.cn/space/api/box/stream/download/asynccode/?code=OTg1NzU4ODlkMGEwZjIwMmVhZTI2MTBiMGRjNjY5NzBfQWxHTVdZMk96Zkl2VFp6N2FwNzk0bnAyT0VKSmZRM2NfVG9rZW46VXRnOGJhazNrb3l6aWd4STgzUGNBWWtibnNoXzE3NTYxMDE3NzE6MTc1NjEwNTM3MV9WNA)

