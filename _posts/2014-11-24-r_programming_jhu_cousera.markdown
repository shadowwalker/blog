---
layout:     post
published: true
title:      "R 语言编程 笔记"
subtitle:   "R是用于统计分析、绘图的语言和操作环境。R是属于GNU系统的一个自由、免费、源代码开放的软件，它是一个用于统计计算和统计制图的优秀工具。"
date:       2014-11-24 23:00:00
author:     "ShadowWalker"
header-img: "img/sledgehammer_peak.jpeg"
location:	"Shanghai"
---
>R是用于统计分析、绘图的语言和操作环境。R是属于GNU系统的一个自由、免费、源代码开放的软件，它是一个用于统计计算和统计制图的优秀工具。

#Get prepared

##Installing R and RStudio

[R website](http://cran.r-project.org/) | [RStudio website](http://www.rstudio.com)

##Writting code / Setting your work directory

<pre class="code-container"><code class="r">getwd() #当前目录
ls() #列出当前载入模块
dir() #列出当前目录文件
read.csv("xxx.csv")
source("r_file.R") #载入脚本
help.start()

myfunction &lt;- function(x){
  y &lt;- rnorm(100)
  mean(y)
}</code></pre>

#Week1
## Overview and history of R

R is a dialect of S language.

Drawbacks
* 数据都是加载进内存
* 3D性能较差

##Getting help

r-help@r-project.org

##Data type

* char
* numeric (real numbers)
	* 双精度，指定整数后面加L
	* Inf，无限大
	* NaN，missing value
* integer
* complex
* logical (True / False or T / F)
* vector
	* 最基本的类型
	* 只能存同类型的class，但list可以存不同类型
	* 创建空vector：vector()
* Attributes
	* names, dimnames
	* dimensions
	* class
	* length
	* others
	* attributes()获取属性

复制运算是<code>&lt;-</code>。注释是<code>#</code>。显示直接输入变量或者用<code>print(x)</code>。输出在一行之前的<code>[1]</code>表示这是一个vector，后面是第1个元素。

###c(a,b,c,...)

创建vector，a, b, c是任意相同类型元素。如果类型不同，会发生自动类型转化。用as.[type]()来进行类型转化，如果转换不成功，会出现NA和警告。

###vector()

<pre class="code-container"><code class="r">x &lt;- vector("numeric", length = 10) #创建vector
x
[1] 0 0 0 0 0 0 0 0 0 0
</code></pre>

###Matrix

<pre class="code-container"><code class="r">&gt; m &lt;- matrix(nrow = 2, ncol = 3)
&gt; m
     [,1] [,2] [,3]
[1,]   NA   NA   NA
[2,]   NA   NA   NA
&gt; dim(m)
[1] 2 3
&gt; attributes(m)
$dim
[1] 2 3
&gt; m &lt;- matrix(1:6, nrow = 2, ncol = 3) #按列填充
&gt; m
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6</code></pre>

将向量变成矩阵<code>dim(m) &lt;- c(x,y)</code>。两个向量结合成矩阵<code>cbind(x, y)</code>和<code>rbind(x, y)</code>。

###Lists

<pre class="code-container"><code class="r">&gt; x &lt;- list(1, "a", TRUE, 1+4i)
&gt; x
[[1]]
[1] 1

[[2]]
[1] "a"

[[3]]
[1] TRUE

[[4]]
[1] 1+4i</code></pre>

###Factors

Factor可以看成是带有label的整数，像enum枚举类型一样。处理函数lm()和glm()。
<pre class="code-container"><code class="r">&gt; x &lt;- factor(c("yes","yes","no","yes","no"))
&gt; ##x &lt;- factor(c("yes","yes","no","yes","no"), level=c("yes","no"))
&gt; x
[1] yes yes no  yes no 
Levels: no yes
&gt; table(x)
x
 no yes 
  2   3 
&gt; unclass(x)
[1] 2 2 1 2 1
attr(,"levels")
[1] "no"  "yes"</code></pre>

###Missing Value

* is.na()
* is.nan()
* NA 有类型，integer NA, character NA ...
* NaN是一个NA，反之不成立

###Data frame

* 等长列表
* 和矩阵不同的是，它里面可以存不同类型的元素
* row.names
* 通常用来创建data frame: read.table()和read.csv()
* 转化成矩阵data.matrix()

<pre class="code-container"><code class="r">&gt; x &lt;- data.frame(foo = 1:4, bar = c(T,T,F,F))
&gt; x
  foo   bar
1   1  TRUE
2   2  TRUE
3   3 FALSE
4   4 FALSE
&gt; nrow(x)
[1] 4
&gt; ncol(x)
[1] 2</code></pre>

###Names

* 任何变量都可以有name属性，矩阵还有维度dimnames属性
* <code>names(x) &lt;- c("a", "b", ...)</code>
* <code>dimnames(m) &lt;- list(c("a","b"),c("c","d"))</code>

##Subsetting

###[

访问向量，类似python分片语法。但又有别的特性。仔细体会。

<pre class="code-container"><code class="r">&gt; x &lt;- c("a","b","c","c","d","a")
&gt; x[1]
[1] "a"
&gt; x[2]
[1] "b"
&gt; x[1:4]
[1] "a" "b" "c" "c"
&gt; x[x > "a"]
[1] "b" "c" "c" "d"
&gt; u <- x > "a"
&gt; u
[1] FALSE  TRUE  TRUE  TRUE  TRUE FALSE
&gt; x[u]
[1] "b" "c" "c" "d"
&gt; x &lt;- matrix(1:6,2,3)
&gt; x[1,2]
[1] 3
&gt; x[2,1]
[1] 2
&gt; x[1,]
[1] 1 3 5
&gt; x[,2]
[1] 3 4
&gt; x[1,2,drop=FALSE]
      [,1]
[1,]    3</code></pre>

###[[

访问list或data frame

<pre class="code-container"><code class="r">&gt; x &lt;- list(foo = 1:4, bar = 0.6)
&gt; x[1]
$foo
[1] 1 2 3 4

&gt; x[[1]]
[1] 1 2 3 4
&gt; x$bar
[1] 0.6
&gt; x[["bar"]]
[1] 0.6
&gt; x["bar"]
$bar
[1] 0.6</code></pre>

###$

用name属性访问。$后面不能是变量，但[[]]当中可以用变量。

###Partial Matching

可以用部分的name匹配整个name，这允许你简写代码。需要增加exact=FALSE参数。

###Removing NA Values

利用is.na(x)产生逻辑序列bad，再结合[!bad]。这是一种思维方法。

complete.cases()


#R-intro.pdf

##Appendix A. A Sample Session

<pre class="code-container"><code class="r">help.start() #打开帮助网页
x &lt;- rnorm(50)
y &lt;- rnorm(x) #生成两个伪随机数向量
plot(x,y)
ls() #列出R对象
rm(x, y)
x &lt;- 1:20 #x=[1, ... ,20]
w &lt;- 1 + sqrt(x)/2 #权重
dummy &lt;- data.frame(x=x, y=x+rnorm(x)*w)
dummy #产生一个data frame列表，有x，y两列
fm &lt;- lm(y ~ x, data=dummy) #简单线性回归，y=f(x)，数据来自dummy
summary(fm)
fm1 &lt;- lm(y ~ x, data=dummy, weight=1/w^2) #加权线性回归
summary(fm1)
attach(dummy) #数据列成为可见变量
lrf &lt;- lowess(x,y) #Make a nonparametric local regression function ?
plot(x,y)
lines(x,lrf$y) #Add in the local regression
abline(0, 1, lty=3) #The true regression line: (intercept 0, slope 1)
abline(coef(fm)) #Unweighted regression line
abline(coef(fm1), col="red") #Weighted regression line
detach() #Remove data frame from the search path
</code></pre>


