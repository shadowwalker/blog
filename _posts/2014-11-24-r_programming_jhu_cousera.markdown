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

[R website](http://cran.r-project.org/)

[RStudio website](http://www.rstudio.com)（安装好后配置work directory）

##Writting code / Setting your work directory

<pre class="code-container"><code class="r">getwd() #当前目录
ls() #列出当前载入模块
dir() #列出当前目录文件
read.csv("xxx.csv")
source("r_file.R") #载入脚本
help.start()
?`:`  #查看 : 的文档
?c  #查看c的文档

myfunction &lt;- function(x){  #写一个函数
  y &lt;- rnorm(100) #伪随机数
  mean(y)
}</code></pre>

#Week1
##Overview and history of R

R is a dialect of S language.

Drawbacks

* 数据都是加载进内存
* 3D性能较差

##Getting help

r-help@r-project.org

##Data type

* character
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

创建vector，a, b, c是任意相同类型元素。如果类型不同，会发生**自动类型转化**。用as.type()来进行类型转化，如果转换不成功，会出现NA和警告。

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

* is.na()和is.nan()讲产生一个logical向量
* NA 有类型，integer NA, character NA ...
* NaN是一个NA，反之不成立
* NA和别的量做加减乘除还是NA
* NA&FALSE为FALSE，NA&TRUE为NA
* NA|FALSE为NA，NA|TRUE为TRUE

###Data frame

* 等长列表
* 和矩阵不同的是，它里面可以存不同类型的元素
* row.names用来标识每一列的名称
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

###[]

访问向量元素，类似python分片语法。但在[]中还可以放一个logical向量来取出对应位置的元素。

<pre class="code-container"><code class="r">&gt; x &lt;- c("a","b","c","c","d","a")
&gt; x[1]
[1] "a"
&gt; x[2]
[1] "b"
&gt; x[1:4]
[1] "a" "b" "c" "c"
&gt; x[x > "a"]
[1] "b" "c" "c" "d"
&gt; u &lt;- x &gt; "a"
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

###[[]]

访问list或data frame，与[]不同的是，[[]]取出的是向量。

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

用name属性访问，但是不用引号。$后面不能是变量，但[[]]当中可以用变量。

###Partial Matching

可以用部分的name匹配整个name，这允许你简写代码。需要增加exact=FALSE参数，例如<code class="r">x[["a",exact=FASLE]]</code>。

###Removing NA Values

利用is.na(x)产生逻辑序列bad，再用x[!bad]。这是一种思维方法。利用complete.cases()可以产生logical序列判断哪些行缺少元素。

##Vectorized operations

* 矩阵相乘是%*%
* 长度不等向量计算，短的序列会循环填充

##Reading and writting data

###读取数据

The read.table function is one of the most commonly used functions for reading data. It has a few
important arguments:

* file, the name of a file, or a connection
* header, logical indicating if the file has a header line
* sep, a string indicating how the columns are separated
* colClasses, a character vector indicating the class of each column in the dataset
* nrows, the number of rows in the dataset
* comment.char, a character string indicating the comment character
* skip, the number of lines to skip from the beginning
* stringsAsFactors, should character variables be coded as factors?

With much larger datasets, doing the following things will make your life easier and will prevent R
from choking.

* Read the help page for read.table, which contains many hints
* Make a rough calculation of the memory required to store your dataset. If the dataset is larger
* than the amount of RAM on your computer, you can probably stop right here.
* Set comment.char = "" if there are no commented lines in your file.
* 估算内存消耗，一般以数据大小的两倍为准

读取部分之后，进行自动类型推断，再读取全部数据。这样性能会好一些。

<pre class="code-container"><code class="r">
initial &gt;- read.table("datatable.txt", nrows = 100)
classes &gt;- sapply(initial, class)
tabAll &gt;- read.table("datatable.txt",
colClasses = classes)</code></pre>

###序列化dump/dput

类似json字符串和json数据变量之间的转化。

* dump - source
* deput - dget

<pre class="code-container"><code class="r">
&gt; y &lt;- data.frame(a = 1, b = "a")
&gt; dput(y)
structure(list(a = 1,
 b = structure(1L, .Label = "a",
 class = "factor")),
 .Names = c("a", "b"), row.names = c(NA, -1L),
 class = "data.frame")
&gt; dput(y, file = "y.R")
&gt; new.y &lt;- dget("y.R")
&gt; new.y
 a b
1 1 a

&gt; x &lt;- "foo"
&gt; y &lt;- data.frame(a = 1, b = "a")
&gt; dump(c("x", "y"), file = "data.R")
&gt; rm(x, y)
&gt; source("data.R")
&gt; y
 a b
1 1 a
&gt; x
[1] "foo"
</code></pre>

###文本链接connections

Data are read in using connection interfaces. Connections can be made to files (most common) or to
other more exotic things.

* file, opens a connection to a file
* gzfile, opens a connection to a file compressed with gzip
* bzfile, opens a connection to a file compressed with bzip2
* url, opens a connection to a webpage

<pre class="code-container"><code class="r">
&gt; str(file)
function (description = "", open = "", blocking = TRUE,
 encoding = getOption("encoding"))
&gt; con &lt;- gzfile("words.gz")
&gt; x &lt;- readLines(con, 10)
&gt; x
 [1] "1080" "10-point" "10th" "11-point"
 [5] "12-point" "16-point" "18-point" "1st"
 [9] "2" "20-point"</code></pre>

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


