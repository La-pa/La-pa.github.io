---
title: LaTeX模板使用说明
tags: [latex]
categories: 科研
date: 2023-03-01 19:20:00
---
# ![](https://g.yuque.com/gr/latex?LaTeX#card=math&code=LaTeX&id=Y69ip)模板使用说明

## 使用注意

- .cls为环境配置文件 
   - 初学者不要随意修改
   - 文件主要记录了一些排版参数
   - 如：一级标题的字号，字体，排列方式等
- .tex为项目源代码 
   - 用来编写正文
- 要采用**XeLaTeX**模式编译

## 模板默认参数

### 纸张

- 论文页面为A4纸纵向
- 上下左右各留出2.5厘米的页边距
- 论文题目和**摘要**在第一页上
- 第一页没有页码，从目录开始设置页码
- 有设置目录
- 有**关键词**，没有设置英文摘要
- 没有设置页眉
- 正文从第二页开始，页码位于**每页页脚中部**，用**阿拉伯数字**从"1"开始连续编号
```
\thispagestyle{empty}
```
```
\pagenumbering{arabic}
```
```
\setcounter{page}{12}
```
### 字体

-  **论文题目用三号黑体字**,并**居中** 
-  一级标题用**四号黑体字**,并**居中** 
-  二级、三级标题用**小四号黑体字**，**左端对齐** 
-  论文中其他汉字一律采用**小四号宋体字**，行距用**单倍行距** 
-  英文和公式默认设置小四号 
- 论文里面的英文单词字母也要用$$包括
> 题目要求没有硬性规定,可以自由调整大小

 

## 数学公式

### 数学符号
| LaTex表达形式 | 对应的希腊字母 | LaTex表达形式 | 对应的希腊字母 |
| --- | --- | --- | --- |
| \\alpha | ![](https://g.yuque.com/gr/latex?%5Calpha#card=math&code=%5Calpha&id=xmaVV) | \\Alpha | ![](https://g.yuque.com/gr/latex?%5CAlpha#card=math&code=%5CAlpha&id=KOzjx) |
| \\beta | ![](https://g.yuque.com/gr/latex?%5Cbeta#card=math&code=%5Cbeta&id=Iz8DE) | \\Beta | ![](https://g.yuque.com/gr/latex?%5CBeta#card=math&code=%5CBeta&id=ourp8) |
| \\gamma | ![](https://g.yuque.com/gr/latex?%5Cgamma#card=math&code=%5Cgamma&id=CpPgq) | \\Gamma | ![](https://g.yuque.com/gr/latex?%5CGamma#card=math&code=%5CGamma&id=XsvxS) |
| \\delta | ![](https://g.yuque.com/gr/latex?%5Cdelta#card=math&code=%5Cdelta&id=G1iUz) | \\Delta | ![](https://g.yuque.com/gr/latex?%5CDelta#card=math&code=%5CDelta&id=A3ik0) |
| \\epsilon | ![](https://g.yuque.com/gr/latex?%5Cepsilon#card=math&code=%5Cepsilon&id=tRHvz) | \\Epsilon | ![](https://g.yuque.com/gr/latex?%5CEpsilon#card=math&code=%5CEpsilon&id=nN9Xy) |
| \\zeta | ![](https://g.yuque.com/gr/latex?%5Czeta#card=math&code=%5Czeta&id=iwTME) | \\Zeta | ![](https://g.yuque.com/gr/latex?%5CZeta#card=math&code=%5CZeta&id=fviTA) |
| \\eta | ![](https://g.yuque.com/gr/latex?%5Ceta#card=math&code=%5Ceta&id=GCOYa) | \\Eta | ![](https://g.yuque.com/gr/latex?%5CEta#card=math&code=%5CEta&id=pSgdU) |
| \\theta | ![](https://g.yuque.com/gr/latex?%5Ctheta#card=math&code=%5Ctheta&id=JYIUo) | \\Theta | ![](https://g.yuque.com/gr/latex?%5CTheta#card=math&code=%5CTheta&id=SzWrr) |
| \\iota | ![](https://g.yuque.com/gr/latex?%5Ciota#card=math&code=%5Ciota&id=SxEt7) | \\Iota | ![](https://g.yuque.com/gr/latex?%5CIota#card=math&code=%5CIota&id=SCsz2) |
| \\kappa | ![](https://g.yuque.com/gr/latex?%5Ckappa#card=math&code=%5Ckappa&id=osGUi) | \\Kappa | ![](https://g.yuque.com/gr/latex?%5CKappa#card=math&code=%5CKappa&id=PLaLu) |
| \\lambda | ![](https://g.yuque.com/gr/latex?%5Clambda#card=math&code=%5Clambda&id=NapwG) | \\Lambda | ![](https://g.yuque.com/gr/latex?%5CLambda#card=math&code=%5CLambda&id=a4THd) |
| \\mu | ![](https://g.yuque.com/gr/latex?%5Cmu#card=math&code=%5Cmu&id=pdJFj) | \\Mu | ![](https://g.yuque.com/gr/latex?%5CMu#card=math&code=%5CMu&id=QrBcs) |
| \\nu | ![](https://g.yuque.com/gr/latex?%5Cnu#card=math&code=%5Cnu&id=vjSKl) | \\Nu | ![](https://g.yuque.com/gr/latex?%5CNu#card=math&code=%5CNu&id=CvbHt) |
| \\xi | ![](https://g.yuque.com/gr/latex?%5Cxi#card=math&code=%5Cxi&id=PPZxo) | \\Xi | ![](https://g.yuque.com/gr/latex?%5CXi#card=math&code=%5CXi&id=MBqkv) |
| \\omicron | ![](https://g.yuque.com/gr/latex?%5Comicron#card=math&code=%5Comicron&id=zMR5D) | \\Omicron | ![](https://g.yuque.com/gr/latex?%5COmicron#card=math&code=%5COmicron&id=LP5k7) |
| \\pi | ![](https://g.yuque.com/gr/latex?%5Cpi#card=math&code=%5Cpi&id=TftD7) | \\Pi | ![](https://g.yuque.com/gr/latex?%5CPi#card=math&code=%5CPi&id=NfAvR) |
| \\rho | ![](https://g.yuque.com/gr/latex?%5Crho#card=math&code=%5Crho&id=QcOfE) | \\Rho | ![](https://g.yuque.com/gr/latex?%5CRho#card=math&code=%5CRho&id=FBZEm) |
| \\sigma | ![](https://g.yuque.com/gr/latex?%5Csigma#card=math&code=%5Csigma&id=inco6) | \\Sigma | ![](https://g.yuque.com/gr/latex?%5CSigma#card=math&code=%5CSigma&id=OTY3x) |
| \\tau | ![](https://g.yuque.com/gr/latex?%5Ctau#card=math&code=%5Ctau&id=k4aFC) | \\Tau | ![](https://g.yuque.com/gr/latex?%5CTau#card=math&code=%5CTau&id=Uh1F8) |
| \\upsilon | ![](https://g.yuque.com/gr/latex?%5Cupsilon#card=math&code=%5Cupsilon&id=Fh0lg) | \\Upsilon | ![](https://g.yuque.com/gr/latex?%5CUpsilon#card=math&code=%5CUpsilon&id=PqqdB) |
| \\varphi | ![](https://g.yuque.com/gr/latex?%5Cvarphi#card=math&code=%5Cvarphi&id=DxCKy) | \\Phi | ![](https://g.yuque.com/gr/latex?%5CPhi#card=math&code=%5CPhi&id=ePRHQ) |
| \\chi | ![](https://g.yuque.com/gr/latex?%5Cchi#card=math&code=%5Cchi&id=oxuWT) | \\Chi | ![](https://g.yuque.com/gr/latex?%5CChi#card=math&code=%5CChi&id=a6aZz) |
| \\psi | ![](https://g.yuque.com/gr/latex?%5Cpsi#card=math&code=%5Cpsi&id=xnezO) | \\Psi | ![](https://g.yuque.com/gr/latex?%5CPsi#card=math&code=%5CPsi&id=j6HHB) |
| \\omega | ![](https://g.yuque.com/gr/latex?%5Comega#card=math&code=%5Comega&id=DbgWT) | \\Omega | ![](https://g.yuque.com/gr/latex?%5COmega#card=math&code=%5COmega&id=wMgLc) |


### 数学公式
| 公式名称 | ![](https://g.yuque.com/gr/latex?LaTex#card=math&code=LaTex&id=KRavA)表达形式 | 实际效果 | 备注 |
| --- | --- | --- | --- |
| 分数 | \\frac{a}{b} | ![](https://g.yuque.com/gr/latex?%5Cfrac%7Ba%7D%7Bb%7D#card=math&code=%5Cfrac%7Ba%7D%7Bb%7D&id=WSLgJ) | 字体更小 |
| 分数 | \\cfrac{a}{b} | ![](https://g.yuque.com/gr/latex?%5Ccfrac%7Ba%7D%7Bb%7D#card=math&code=%5Ccfrac%7Ba%7D%7Bb%7D&id=rlzE1) | 字体更大 |
| 开方 | \\sqrt{x^3} | ![](https://g.yuque.com/gr/latex?%5Csqrt%7Bx%5E3%7D#card=math&code=%5Csqrt%7Bx%5E3%7D&id=Ifkek) |  |
| 开方 | \\sqrt[3]{\\frac xy} | ![](https://g.yuque.com/gr/latex?%5Csqrt%5B3%5D%7B%5Cfrac%20xy%7D#card=math&code=%5Csqrt%5B3%5D%7B%5Cfrac%20xy%7D&id=dmyJ5) |  |
| 对数 | \\log_{21} {xy} | ![](https://g.yuque.com/gr/latex?%5Clog_%7B21%7D%20%7Bxy%7D#card=math&code=%5Clog_%7B21%7D%20%7Bxy%7D&id=OAbRK) |  |
| 求和 | \\sum | ![](https://g.yuque.com/gr/latex?%5Csum#card=math&code=%5Csum&id=wMWrj) |  |
| 导数 | {f}’(x) = x^2 + x | ![](https://g.yuque.com/gr/latex?%7Bf%7D%E2%80%99(x)%20%3D%20x%5E2%20%2B%20x#card=math&code=%7Bf%7D%E2%80%99%28x%29%20%3D%20x%5E2%20%2B%20x&id=O65fO) |  |
| 极限 | \\lim_{x \\to 0} \\frac {3x ^2 +7x^3} {x^2 +5x^4} = 3 | ![](https://g.yuque.com/gr/latex?%5Clim_%7Bx%20%5Cto%200%7D%20%5Cfrac%20%7B3x%20%5E2%20%2B7x%5E3%7D%20%7Bx%5E2%20%2B5x%5E4%7D%20%3D%203#card=math&code=%5Clim_%7Bx%20%5Cto%200%7D%20%5Cfrac%20%7B3x%20%5E2%20%2B7x%5E3%7D%20%7Bx%5E2%20%2B5x%5E4%7D%20%3D%203&id=eNhJ8) |  |
| 积分 | \\int_a^b f(x) dx  | ![](https://cdn.nlark.com/yuque/__latex/5a406d68accdcb981f883c36e54f04d0.svg#card=math&code=%5Cint_a%5Eb%20f%28x%29%20dx%20&id=X7F0p) |  |
| 积分 | \\int_0^{+\\infty} x^n e^{-x} dx = n!  | ![](https://cdn.nlark.com/yuque/__latex/559254ac774adc51a3cca1a3edbf9fc1.svg#card=math&code=%5Cint_0%5E%7B%2B%5Cinfty%7D%20x%5En%20e%5E%7B-x%7D%20dx%20%3D%20n%21%20%0A&id=mHaUk) |  |


### 特殊符号
| LaTex 表达式 | 实际效果 | LaTex 表达式 | 实际效果 |
| --- | --- | --- | --- |
| \\lt | < | \\gt | ![](https://cdn.nlark.com/yuque/__latex/f68d1ec1fc5a20a354d2fd017bea7b4c.svg#card=math&code=%5Cgt&id=lqNir) |
| \\le | ≤ | \\leq | ![](https://cdn.nlark.com/yuque/__latex/099d95b2a7b10bbfd2d4725d9b68eec1.svg#card=math&code=%5Cleq&id=wGCGZ) |
| \\leqq | ![](https://cdn.nlark.com/yuque/__latex/0991836aeed031b0776b32fef3686d18.svg#card=math&code=%5Cleqq&id=XUoox) | \\leqslant | ![](https://cdn.nlark.com/yuque/__latex/cccc6146fd5f1329c6074cef0e3e51ef.svg#card=math&code=%5Cleqslant&id=nurk3) |
| \\ge | ≥ | \\geq | ![](https://cdn.nlark.com/yuque/__latex/566be5f7e4e2c11eee07d4d7f80810c6.svg#card=math&code=%5Cgeq&id=vwfWu) |
| \\geqq | ![](https://cdn.nlark.com/yuque/__latex/02355be35ee9fe00e2f251c20b5ad408.svg#card=math&code=%5Cgeqq&id=XBgf2) | \\geqslant | ![](https://cdn.nlark.com/yuque/__latex/e94e398336ef10e2a4058b7126a88064.svg#card=math&code=%5Cgeqslant&id=gTlD8) |
| \\neq | ≠ | \\not\\lt | ![](https://cdn.nlark.com/yuque/__latex/83c49a78f88e710d3581b9adade8ce89.svg#card=math&code=%5Cnot%5Clt&id=WqGls) |
| \\not | 在几乎 所有的 | 符号上划出 | 一个斜线 |
| \\times | × | \\div | ![](https://cdn.nlark.com/yuque/__latex/96a522b9753f0cfafb269471e6cb3b6a.svg#card=math&code=%5Cdiv&id=ZH77O) |
| \\pm | ± | \\mp | ![](https://cdn.nlark.com/yuque/__latex/550fa21d7f26efbbdba9a320998e9408.svg#card=math&code=%5Cmp&id=MaysC) |
| \\cdot | · |  |  |
| \\cup | ![](https://cdn.nlark.com/yuque/__latex/b7a5f9ac73cf4375693727ecdd011b75.svg#card=math&code=%5Ccup&id=weFL7) | \\cap | ![](https://cdn.nlark.com/yuque/__latex/3d2c7ef54fdc6f33d2f05767c6bb74d9.svg#card=math&code=%5Ccap&id=Af0Dr) |
| \\setminus | \\ | \\subset | ![](https://cdn.nlark.com/yuque/__latex/5bc159fa1c150eff8eef9fee091f1138.svg#card=math&code=%5Csubset&id=qUZuC) |
| \\subseteq |  ⊆   | \\subsetneq | ![](https://cdn.nlark.com/yuque/__latex/8120b457264516f9be40c77f6919765f.svg#card=math&code=%5Csubsetneq&id=ZDeio) |
| \\supset |  ⊃   | \\in | ![](https://cdn.nlark.com/yuque/__latex/470e99d8c9c2615aacaa30dbf04f9f50.svg#card=math&code=%5Cin&id=tGqaU) |
| \\notin |  ∉   | \\emptyset | ![](https://cdn.nlark.com/yuque/__latex/0dac1faa31e319949178b040b44ed6ea.svg#card=math&code=%5Cemptyset&id=AOQbU) |
| \\varnothing |  ∅   |  |  |
| {n+1 \\choose 2k } | ![](https://cdn.nlark.com/yuque/__latex/76f5aec9de8f8b3ddefb355a0bff0a66.svg#card=math&code=%7Bn%2B1%20%5Cchoose%202k%20%7D&id=VeDv6) | \\binom{n+1}{2k} | ![](https://cdn.nlark.com/yuque/__latex/ba8c7e63f37a61ac67a7068b39217341.svg#card=math&code=%5Cbinom%7Bn%2B1%7D%7B2k%7D&id=reZQQ) |
| \\to | ![](https://cdn.nlark.com/yuque/__latex/d59084f8d5c6229967c0d93bd3204205.svg#card=math&code=%5Cto&id=foB4g) | \\rightarrow | ![](https://cdn.nlark.com/yuque/__latex/33b44e34aa35b8c4ecd0606453ee68e9.svg#card=math&code=%5Crightarrow&id=wIKrf) |
| \\leftarrow | ![](https://cdn.nlark.com/yuque/__latex/1c4b6f6d50a08c763be1abeca063a01f.svg#card=math&code=%5Cleftarrow&id=DZxWi) | \\Rightarrow | ![](https://cdn.nlark.com/yuque/__latex/d7e62ff1f9ebb4d97584ede054a0dca9.svg#card=math&code=%5CRightarrow&id=FQVWn) |
| \\Leftarrow | ![](https://cdn.nlark.com/yuque/__latex/1f5819e54ef9505019e7ac60143758f4.svg#card=math&code=%5CLeftarrow&id=xy2Ak) | \\mapsto | ![](https://cdn.nlark.com/yuque/__latex/4e813c345f577894b0c1e053a637e97d.svg#card=math&code=%5Cmapsto&id=BBWEK) |
| \\land | ![](https://cdn.nlark.com/yuque/__latex/c5054f61f70dc1af3f7a40e385a0c24b.svg#card=math&code=%5Cland&id=Zmpq3) | \\lor | ![](https://cdn.nlark.com/yuque/__latex/d9004dc46d0311f74a6567057b5e94d5.svg#card=math&code=%5Clor&id=CfV8U) |
| \\lnot | ![](https://cdn.nlark.com/yuque/__latex/ae9d7b286388a0a28c6ef1f8c4100af4.svg#card=math&code=%5Clnot&id=NFDKw) | \\forall | ![](https://cdn.nlark.com/yuque/__latex/e74dbb51ca536167508e8a301e28a7bc.svg#card=math&code=%5Cforall&id=KbteM) |
| \\exists | ![](https://cdn.nlark.com/yuque/__latex/a121a1a192df6a8b98900d70c34aafd2.svg#card=math&code=%5Cexists&id=yb1Ma) | \\top | ![](https://cdn.nlark.com/yuque/__latex/2df6c7e64ac032c3ceae0ff3542333e6.svg#card=math&code=%5Ctop&id=XcaHb) |
| \\bot | ![](https://cdn.nlark.com/yuque/__latex/2905452b4866b721490103eeee0332e0.svg#card=math&code=%5Cbot&id=N78GF) | \\vdash | ![](https://cdn.nlark.com/yuque/__latex/61c7fdc8da248b8ce4f89e11242aa60b.svg#card=math&code=%5Cvdash&id=RcD2R) |
| \\vDash | ![](https://cdn.nlark.com/yuque/__latex/a28089a40fff6bc633abe4823b4ee899.svg#card=math&code=%5CvDash&id=eMdH3) |  |  |
| \\star | ![](https://cdn.nlark.com/yuque/__latex/c50ec45c6548ceb5e58d718da748aa92.svg#card=math&code=%5Cstar&id=GhHV2) | \\ast | ![](https://cdn.nlark.com/yuque/__latex/4465370cd3e46f95c5393f7b4723adb0.svg#card=math&code=%5Cast&id=zZMbo) |
| \\oplus | ![](https://cdn.nlark.com/yuque/__latex/df723412b927e0f7659c7e766b3bb463.svg#card=math&code=%5Coplus&id=T3tIN) | \\circ | ![](https://cdn.nlark.com/yuque/__latex/a11083d0ac107184134b2863c7f7c469.svg#card=math&code=%5Ccirc&id=w77Hx) |
| \\bullet | ![](https://cdn.nlark.com/yuque/__latex/cea23f6fe174625795dbf85521e854ab.svg#card=math&code=%5Cbullet&id=phaDy) |  |  |
| \\approx | ![](https://cdn.nlark.com/yuque/__latex/b87101e34279ae8a2c60bd9080f20799.svg#card=math&code=%5Capprox&id=fU5lU) | \\sim | ![](https://cdn.nlark.com/yuque/__latex/99cc18bdbb17061cd85ddd580ff06806.svg#card=math&code=%5Csim&id=GwDTR) |
| \\simeq | ![](https://cdn.nlark.com/yuque/__latex/21138d23000a095edaa1fac1fd665a18.svg#card=math&code=%5Csimeq&id=cqFB0) | \\cong | ![](https://cdn.nlark.com/yuque/__latex/ada489f5c3cd62847f64f20abe5559dd.svg#card=math&code=%5Ccong&id=aubj6) |
| \\equiv | ![](https://cdn.nlark.com/yuque/__latex/ed8cd2d9bdadeb1b9324ca9eb8e07f7d.svg#card=math&code=%5Cequiv&id=J5MxN) | \\prec | ![](https://cdn.nlark.com/yuque/__latex/d46b0d0e48fa2183b9e6d5433c0fcf2f.svg#card=math&code=%5Cprec&id=l9AS1) |
| \\lhd | ![](https://cdn.nlark.com/yuque/__latex/e562bd2d453bac2ce5f4612c8137704a.svg#card=math&code=%5Clhd&id=imN0O) | \\therefore | ![](https://cdn.nlark.com/yuque/__latex/1c7fffe977a87d3cf4f617046bc561df.svg#card=math&code=%5Ctherefore&id=z9Kde) |
| \\infty | ![](https://cdn.nlark.com/yuque/__latex/ded19e461c6fc600ca2fdb1aef1faeb0.svg#card=math&code=%5Cinfty&id=d8oJW) | \\aleph_0 | ![](https://cdn.nlark.com/yuque/__latex/aa9297ba83836329c7060c4945d32b4c.svg#card=math&code=%5Caleph_0&id=caN1T) |
| \\nabla | ![](https://cdn.nlark.com/yuque/__latex/ad18d6dc75c75bb836990d63c34d4027.svg#card=math&code=%5Cnabla&id=IYnBA) | \\partial | ![](https://cdn.nlark.com/yuque/__latex/3d63d510ef7ee224dcdee37c761d1c26.svg#card=math&code=%5Cpartial&id=MsKtl) |
| \\Im | ![](https://cdn.nlark.com/yuque/__latex/4d8393cdc1015b0ebc19ee8af6140dc6.svg#card=math&code=%5CIm&id=CVjZt) | \\Re | ![](https://cdn.nlark.com/yuque/__latex/051ca2fbd32f4bcaceaccb61d0093a32.svg#card=math&code=%5CRe&id=DVZhJ) |
|  a \\equiv b \\pmod n | ![](https://cdn.nlark.com/yuque/__latex/ac1b639ec5961bff82f3203491d6e563.svg#card=math&code=a%20%5Cequiv%20b%20%5Cpmod%20n&id=VIYId) |  |  |
| \\Idots | ![](https://cdn.nlark.com/yuque/__latex/94204d8b96b40787978cee295fb11454.svg#card=math&code=%5Cldots&id=F9Skg) | \\cdots | ![](https://cdn.nlark.com/yuque/__latex/50f96a17cb49da63abb51134b6f8324c.svg#card=math&code=%5Ccdots&id=ROdcF) |
| \\epsilon | ![](https://cdn.nlark.com/yuque/__latex/7c102e7a7d231bf935f9bc23417779a8.svg#card=math&code=%5Cepsilon&id=h5Nay) | \\varepsilon | ![](https://cdn.nlark.com/yuque/__latex/c57c5f0e31d8960d9406bb149fced9e0.svg#card=math&code=%5Cvarepsilon&id=KEwwU) |
| \\phi | ![](https://cdn.nlark.com/yuque/__latex/f547e08b7db926659f19deee4b4363d1.svg#card=math&code=%5Cphi&id=YaCmA) | \\varphi | ![](https://cdn.nlark.com/yuque/__latex/881403b34487b79bc2ceeeed2c8ea87c.svg#card=math&code=%5Cvarphi&id=fOmd2) |
| \\ell | ![](https://cdn.nlark.com/yuque/__latex/08843ad184698b2d78d7a39e060b9574.svg#card=math&code=%5Cell&id=J1qTA) |  |  |


## 文字操作

#### 公式加粗

```latex
$\bm{ .... }$
```

#### 文字加粗

```latex
{\bf ...}
```

#### 调整字体

```latex
字体大小：
七号 　　5.25pt 　　   1.845mm　　　　\tiny
六号 　　7.875pt 　　 2.768mm　　　　 \scriptsize
小五号 　9pt 　　　　  3.163mm　　　　 \footnotesize
五号 　　10.5pt 　　   3.69mm　　　　 \small
小四号 　12pt 　　　　4.2175mm　　　   \normalsize
四号 　　13.75pt 　　 4.83mm　　　　   \large
三号 　　15.75pt 　　 5.53mm　　　　   \Large
二号 　　21pt 　　　　7.38mm          \LARGE
一号 　　27.5pt 　　   9.48mm　　　　  \huge
小初号 　36pt 　　　　12.65mm　　　　  \Huge
初号 　　42pt 　　　　14.76mm		  

使用方法：替换下面代码中的small即可

\begin{small}

\ldots

\end{small}
```

### 其他操作

#### 上下标
对于上标使用 下划线表示“ _ ” ；对于上标使用 “ ^ ”表示。比如 x i 2 x_i^2x 
i
2
	
 的LaTex表达式为 $x_i^2$ 。

LaTex表达式中的上下标可以叠加的，就比如 x y z {x^y}^zx 
y

z
 的LaTex表达式为 ${x^y}^z$ 或者 $x^{y^z}$

在此需要注意的是：LaTex表达式默认的是 “ _ ” “ ^ ” 之后的一位才是上下标的内容，对于超过一个字母的上下标需要使用 { } 将它括起来，比如x 2 i 2 + b x_{2i}^{2+b}x 
2i
2+b
	
 的LaTex表达式为$x_{2i}^{2+b}$。
[
](https://blog.csdn.net/ViatorSun/article/details/82826664)x上加横线
 加^号：\hat{x}

加横线：\overline{x}

加宽^：\widehat{x}

加波浪线：\widetilde{x}

加一个点：\dot{x}

加两个点：\ddot{x}  

#### 表格

- 没有竖线

```latex
\textbf{表格格式}

\begin{tabular}{cc}
 \hline
 \makebox[0.4\textwidth][c]{符号}	&  \makebox[0.5\textwidth][c]{意义} \\ \hline
 D	    & 宽度（cm） \\ \hline
 L	    & 长度（cm）  \\ \hline
\end{tabular}
```

- 有竖线

```latex
\begin{table}[htbp] % htbp代表表格浮动位置
% 表格居中
\centering
% 添加表头
\caption{表格}
% 创建table环境
\begin{tabular}{|cc|c|} % 3个c代表3列都居中，也可以设置l或r，|代表竖线位置
%|CC|c|表示对其方式
% 表格的输入
\hline  % 一条水平线
%$P$ & 最优$k$值 \\ % \\为换行符 
%\hline
\makebox[0.4\textwidth][c]{$P$}	&  \makebox[0.5\textwidth][c]{最优$k$值} \\ \hline
0.1 & 32 \\
\hline
0.2 & 23 \\
\hline
1 & 10 \\
\hline
\end{tabular}
\end{table}
```

#### 图片

```latex
\textbf{图片格式}
\begin{figure}[h]
\centering
\includegraphics[width=5cm]{xxx.jpg}
\caption{图片标题}
\end{figure}
```

#### 参考文献

#### 代码块

### 页面操作

- 新建页面

### 常见问题

- 目录要编译两遍才能正常显示

### 页的基本操作
```latex
%%%想要另起一页
\newpage

%%%一般下面这种用的多
\clearpage

```
```latex
\pagestyle{empty}%%整篇文章不显示

\thispagestyle{empty}%%仅当前页

\setcounter{page}{1}%%页号从正文开始

```

```latex
%设置页码数字类型
\pagenumbering{digit type}
%其中digit type有：
arabic      %阿拉伯数字(1,2,3,4)，默认样式
roman      小写罗马数字(i,ii,iii,iv)
Roman     大写罗马数字(I,II,III,IV)
alph         小写拉丁字母(a,b,c,d)
Aiph         大写拉丁字母(A,B,C,D)

%如果想让当前页不标页码，可使用
\thispagestyle{empty}

```
