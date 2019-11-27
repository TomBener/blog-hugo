---
tags:
  - LaTeX
date: 2019-11-08
title: macOS 上安装 MathTime Professional 2 字体
---

世界上最好看的数学字体！推荐使用 [一步到位安装方法](#一步到位安装方法)。

<!-- more -->

------

[MathTime Professional 2](https://www.pctex.com/mtpro2.html)（简称 MTPro2）是由 [Michael Spivak](https://en.wikipedia.org/wiki/Michael_Spivak) 设计的一款商业数学字体，版权归 [PCTeX](https://www.pctex.com) 公司所有。MTPro2 被很多人称为「世界上最好看的数学字体」[^1]，被许多 [著名期刊使用](https://www.pctex.com/MTProUsers.html)，包括 [美国数学协会](https://maa.org)、[数学科学出版社](https://msp.org) 等。

[^1]: 当然，字体好看与否存在很大的主观性，一定也有很多人认为 MTPro2 并不太好看。

MTPro2 完整版 [售价](https://www.pctex.com/mtpro2.html#Pricing) 179 美元（其中学术版 129 美元），但它也提供免费版本，即 MTPro2 Lite，也就是我将要安装的版本。其实之前我是成功安装过 MTPro2 字体的，不过 10 月下旬重装了 macOS，就不在电脑中了。也不记得当时是怎么安装的了，因此把这次安装过程记录下来，以供之后回顾或者为他人提供参考。

在 $\mathrm{\LaTeX{}}$ 中，西文字体和数学字体我一直使用 [newtx](https://ctan.org/pkg/newtx)，newtx 是一种类 Times 字体，是在 $\mathrm{\LaTeX{}}$ 中使用 Times New Roman[^2] 的常用方式。虽然它可以用 `newtxtext` 和 `newtxmath` 分别设置西文字体和数学字体，但数学字体中的巨运算符很丑，不得不使用下面的代码将求和 $\displaystyle \sum$、积分 $\displaystyle \int$、偏微分符号 $\partial$、圆周率 $\pi$ 等符号改回默认的 Computer Modern Font[^a]。

[^2]: 这里不得不吐槽一下，因为 Microsoft Word，很多人对 Times New Roman 字体 [存在误解](https://liam.page/2017/01/10/Times-New-Roman-and-LaTeX/)。严格来说，Times New Roman 不是**一种**字体，就像 $\mathrm{\TeX{}}$ 发行版一样，不同的操作系统存在一些差异。

[^a]: 由于 newtx 的更新，使用下面的代码可能会报错。

```latex
% 将数学字体宏包的求和、积分等改回默认的 Computer Modern Font
% \sum
\DeclareSymbolFont{largesymbolsCM}{OMX}{cmex}{m}{n}
\let\txsum\sum
\let\sum\relax
\DeclareMathSymbol{\sum}{\mathop}{largesymbolsCM}{"50}
% \infty
\DeclareSymbolFont{symbolsCM}{OMS}{cmsy}{m}{n}
\SetSymbolFont{symbolsCM}{bold}{OMS}{cmsy}{b}{n}
\let\txinfty\infty
\DeclareMathSymbol{\infty}{\mathord}{symbolsCM}{"31}
% \partial, \pi
\DeclareSymbolFont{lettersCM}{OML}{cmm} {m}{it}
\SetSymbolFont{lettersCM}{bold}{OML}{cmm} {b}{it}
\let\txpartial\partial
\DeclareMathSymbol{\partial}{\mathord}{lettersCM}{"40}
\let\txpi\pi
\DeclareMathSymbol{\pi}{\mathord}{lettersCM}{"19}
% int
\RequirePackage{esint}
```

newtx 宏包（准确的说是 newtxmath）最近的几次更新导致了很多 Bug，不是 [和 ctex 冲突](https://github.com/CTeX-org/ctex-kit/issues/454) 就是 [和 amsmath 冲突](https://github.com/xueruini/thuthesis/pull/434#issuecomment-539456257)，导致我原来写的模板一编译就报错，于是今天我下定决心放弃 newtx，改用 MTPro2，但是 MTPro2 不包含在 MacTeX 中，需要自行安装。

## 官方安装方法

1. 打开 [下载页面](http://www2.pctex.com/downloads.php?product=MTP2L)，按照提示进行下载，就会获取一个名为 `mtp2lite.zip.tpm` 的文件[^b]，用解压缩软件打开是下图中的这个样子。

[^b]: 刚开始我没有看出来这是一个压缩文件。可以去掉后缀 `.tpm` 使其成为一个 `.zip` 格式的压缩文件，然后用常用的解压缩软件打开。

![](https://cdn.jsdelivr.net/gh/TomBener/image-hosting/images/mtp2lite-zip.png)

2. 解压缩之后，将 `texmf` 目录下的四个文件夹：`doc`、`fonts`、`source`、`tex`，也就是上图选中的四个文件夹，移动到 `/usr/local/texlive/texmf-local` 目录下，可能系统会要求输入密码。

3. 打开终端，输入：

   ```sh
   sudo texhash
   ```

4. 然后输入密码，接着再输入：

   ```sh
   sudo updmap-sys --enable Map=mtpro2.map
   ```

5. （可选） 移除 belleek Times 字体：

   ```sh
   sudo updmap-sys --disable Map=belleek.map
   ```

不出问题的话，到这一步就安装好了。但是，如果有问题的话，就使用下面的「一步到位安装方法」吧。

## 一步到位安装方法

实际上，按照官方安装方法，我没有安装成功，可能是由于最开始文件移动的问题。不过，经过我的不懈搜索，在 GitHub 上找到了一个 [项目](https://github.com/jamespfennell/mathtime-installer)，可以一步到位，在 macOS 或 Linux 上快速地安装完成 MTPro2。

1. 下载 [MTPro2 Lite](http://www2.pctex.com/downloads.php?product=MTP2L) 和 [mathtime-installer-master](https://github.com/jamespfennell/mathtime-installer/archive/master.zip)，这里假设都下载到 `Downloads` 目录下。
2. 将下载好的 `mtp2lite.zip.tpm` 移动到 `mathtime-installer-master` 文件夹里，因此 `mathtime-installer-master` 文件夹的结构如下：

```
Downloads/mathtime-installer-master
├── LICENSE
├── mtp2lite.zip.tpm
├── mtpro2-texlive.sh
├── README.md
```

3. 在终端中输入如下命令，使 `mtpro2-texlive.sh` 成为可执行文件：

```sh
cd Downloads/mathtime-installer-master
chmod +x ./mtpro2-texlive.sh
```

4. 输入以下命令，回车即开始运行 `mtpro2-texlive.sh`，安装 MTPro2 Lite 字体（需要输入密码）：

```sh
./mtpro2-texlive.sh -i mtp2lite.zip.tpm
```

5. 若输出结果如下，则表示安装成功：

```
Installing MathTime Professional 2 from mtp2lite.zip.tpm.
Unpacking mtp2lite.zip.tpm.
Password:
Copying files.
Installing MathTime Professional 2.
  > running texhash
  > updating map references
  > editing updmap.cfg
  > updating TeX Live databases
TeX Live updated; checking that MTPro2 works...
Succesfully compiled LaTeX file with MTPro2 included.
MathTime Professional 2 installed.
Documentation available at /usr/local/share/texmf/docs/
```

但是我的输出多出了 4 行报错信息：

```
kpathsea: Running mktexpk --mfmode / --bdpi 600 --mag 0+480/600 --dpi 480 mt2mis
mktexpk: don't know how to create bitmap font for mt2mis.
mktexpk: perhaps mt2mis is missing from the map file.
kpathsea: Appending font creation commands to missfont.log.
```

然后我又试了几次，输出结果还是这样。并且在 `mathtime-installer-master` 文件夹里有一个文件 `missfont.log`，打开日志文件 `mtpro2-texlive.sh.log`，发现有报错：

```{2}
updmap [ERROR]: The following map file(s) couldn't be found:
updmap [ERROR]: 	mtpro2.mapp (in /usr/local/texlive/2019/texmf-config/web2c/updmap.cfg)
updmap [ERROR]: Did you run mktexlsr?

	You can disable non-existent map entries using the option
	  --syncwithtrees.
```

在这里看到了 `updmap.cfg`，才想起来我手动修改过这个文件，猜测应该是这个原因导致的，于是我将这个文件删除掉，然后再次执行上述操作，果然就不再报错了。所以不要乱修改这种自动生成的文件啊！🌚

## 使用 MTPro2

```latex
\documentclass{article}
\usepackage{amsmath}
\usepackage[lite,subscriptcorrection,slantedGreek,nofontinfo,amsbb,eucal]{mtpro2}
\usepackage{libertine}
\usepackage{lipsum}

\begin{document}

\title{MathTime Professional 2 Test}
\author{TomBen}
\maketitle

$$\sin{x}=\sum_{n=0}^{\infty}\frac{(-1)^n}{(2n+1)!}{x}^{2n+1}=x-\frac{x^{3}}{3!} +\frac{x^{5}}{5!}-\frac{x^{7}}{7!}+{}\cdots$$

\[\ln{(x+1)}=\sum_{n=1}^{\infty}\frac{(-1)^{n-1}}{n}{x}^{n}=x-\frac{x^{2}}{2!} +\frac{x^{3}}{3!}-\frac{x^{4}}{4!}+{}\cdots\]

Euler Equation:
\begin{align}
\mathrm{e}^{\mathrm{i}x}=\cos{x}+\mathrm{i}\sin{x}
\end{align}

\[
\lim_{x\to0}\frac{\sqrt{1+2\tan{x}}-\sqrt{1+2\sin{x}}}{x\ln(1+x)-x^2}
\]

$$
\iint\limits_{D}\frac{1+xy}{1+x^2++y^2}\mathrm{d}x\mathrm{d}y.
$$

\[
\sum_{i=1}^n \sin x+i^{\sin x}+ i^{i^{\sin x}}
\]

\begin{equation}
    \SQRT{\sum_{i=1}^n (y^i -x^i )^2 }
\end{equation}

\lipsum[2]

\end{document}
```

输出结果如下图所示，点击 [这里](https://tomben.me/files/pdf/mtpro2-test/mtpro2-test.pdf) 可查看或下载 PDF 版本。

![](https://cdn.jsdelivr.net/gh/TomBener/image-hosting/images/mtpro2-test.png)


作为对比，以下是标准 $\mathrm{\LaTeX{}}$ 的 Computer Modern 数学字体样式：


$$\sin{x}=\sum_{n=0}^{\infty}\frac{(-1)^n}{(2n+1)!}{x}^{2n+1}=x-\frac{x^{3}}{3!} +\frac{x^{5}}{5!}-\frac{x^{7}}{7!}+{}\cdots$$

$$\ln{(x+1)}=\sum_{n=1}^{\infty}\frac{(-1)^{n-1}}{n}{x}^{n}=x-\frac{x^{2}}{2!} +\frac{x^{3}}{3!}-\frac{x^{4}}{4!}+{}\cdots$$

$$\mathrm{e}^{\mathrm{i}x}=\cos{x}+\mathrm{i}\sin{x}$$

$$\lim_{x\to0}\frac{\sqrt{1+2\tan{x}}-\sqrt{1+2\sin{x}}}{x\ln(1+x)-x^2}$$

$$\iint\limits_{D}\frac{1+xy}{1+x^2++y^2}\mathrm{d}x\mathrm{d}y$$

$$\sum_{i=1}^n \sin x+i^{\sin x}+ i^{i^{\sin x}}$$

$$\sqrt{\sum_{i=1}^n (y^i -x^i )^2 }$$

MathTime Professional 2 和 Computer Modern 相比，哪种字体更好看，就是仁者见仁，智者见智了。
