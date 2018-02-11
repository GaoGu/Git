##正则表达式

	正则表达式(Regular Expression)是一种文本模式
	包括普通字符（例如，a 到 z 之间的字母）和特殊字符（称为"元字符"）
	正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串
### 普通字符

	普通字符包括没有显式指定为元字符的所有可打印和不可打印字符。
	这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号。
### 非打印字符
非打印字符也可以是正则表达式的组成部分。下表列出了表示非打印字符的转义序列：

|字符|描述|  
|-|-|
|\cx|匹配由x指明的控制字符。<br/>例如， \cM 匹配一个 Control-M 或回车符。<br/>x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。|
|\f|匹配一个换页符。等价于\xz0c和\cL|
|\n|匹配一个换行符。等价于 \x0a 和 \cJ。|
|\r|匹配一个回车符。等价于 \x0d 和 \cM。|
|\t|匹配一个制表符。等价于 \x09 和 \cI。|
|\v|匹配一个垂直制表符。等价于 \x0b 和 \cK。|
|\s|匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。|
|\S|匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。|
### 特殊字符
所谓特殊字符，就是一些有特殊含义的字符.
如果要配匹配特殊字符本身，则需要用\进行转义
例如要匹配 `$` 字符本身，请使用 `\$`。

|特别字符|描述|
|-|-|
|^|匹配输入字符串的开始位置，除非在方括号表达式中使用，此时它表示不接受该字符集合|
|$|匹配输入字符串的结尾位置。<br/>如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。|
|()|	标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用|
|*|匹配前面的子表达式零次或多次|
|+|匹配前面的子表达式一次或多次|
|.|匹配除换行符 \n 之外的任何单字符|
|[|标记一个中括号表达式的开始|
|?|匹配前面的子表达式零次或一次，或指明一个非贪婪限定符|
|\|将下一个字符标记为特殊字符、原义字符、或向后引用、或八进制转义符<br/>例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\\' 匹配 "\"，而 '\(' 则匹配 "("。|
|{|标记限定符表达式的开始|
|竖线|指明两项之间的一个选择|

竖线`|`指明两项之间的一个选择
### 限定符
限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 * 或 + 或 ? 或 {n} 或 {n,} 或 {n,m} 共6种。

|字符|描述|
|-|-|
|*|匹配前面的子表达式零次或多次,* 等价于{0,}|
|+|	匹配前面的子表达式一次或多次,+ 等价于 {1,}|
|?|匹配前面的子表达式零次或一次, 等价于 {0,1}|
|{n}|n 是一个非负整数。匹配确定的 n 次。|
|{n,}|n 是一个非负整数。至少匹配n 次|
|{n,m}|m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。|
*和+限定符都是贪婪的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个?就可以实现非贪婪或最小匹配。
### 定位符
|字符|描述|
|-|-|
|^|	匹配输入字符串开始的位置。<br/>如果设置了 RegExp 对象的 Multiline 属性，^ 还会与 \n 或 \r 之后的位置匹配|
|$|匹配输入字符串结尾的位置。<br/>如果设置了 RegExp 对象的 Multiline 属性，$ 还会与 \n 或 \r 之前的位置匹配|
|\b|匹配一个字边界，即字与空格间的位置|
|\B|	非字边界匹配|
注意：不能将限定符与定位符一起使用。由于在紧靠换行或者字边界的前面或后面不能有一个以上位置，因此不允许诸如 ^* 之类的表达式
`/\bCha/`匹配单词 Chapter 的开头三个字符，因为这三个字符出现在字边界后面
`/ter\b/`匹配单词 Chapter 中的字符串 ter，因为它出现在字边界的前面
`/\Bapt/`匹配 Chapter 中的字符串 apt，但不匹配 aptitude 中的字符串 apt
### 选择
用圆括号将所有选择项括起来，相邻的选择项之间用|分隔。但用圆括号会有一个副作用，使相关的匹配会被缓存，此时可用`?:`放在第一个选项前来消除这种副作用。
其中 `?:` 是非捕获元之一，还有两个非捕获元是 `?=` 和` ?!`，这两个还有更多的含义，前者为正向预查，在任何开始匹配圆括号内的正则表达式模式的位置来匹配搜索字符串，后者为负向预查，在任何开始不匹配该正则表达式模式的位置来匹配搜索字符串。
### 反向引用
对一个正则表达式模式或部分模式两边添加圆括号将导致相关匹配存储到一个临时缓冲区中，所捕获的每个子匹配都按照在正则表达式模式中从左到右出现的顺序存储。
缓冲区编号从 1 开始，最多可存储 99 个捕获的子表达式。
每个缓冲区都可以使用 \n 访问，其中 n 为一个标识特定缓冲区的一位或两位十进制数。
可以使用非捕获元字符 ?:、?= 或 ?! 来重写捕获，忽略对相关匹配的保存。

查找文本中两个相同的相邻单词的匹配项	

	var str = "Is is the cost of of gasoline going up up";
	var patt1 = /\b([a-z]+) \1\b/ig;
	document.write(str.match(patt1));
运行结果

	Is is,of of,up up
正则表达式后面的全局标记 g 指定将该表达式应用到输入字符串中能够查找到的尽可能多的匹配。
表达式的结尾处的不区分大小写 i 标记指定不区分大小写。

反向引用还可以将通用资源指示符 (URI) 分解为其组件

	var str = "http://www.runoob.com:80/html/html-tutorial.html";
	var patt1 = /(\w+):\/\/([^/:]+)(:\d*)?([^# ]*)/;
	arr = str.match(patt1);
	for (var i = 0; i < arr.length ; i++) {
	    document.write(arr[i]);
	    document.write("<br>");
	}
运行结果

	http://www.runoob.com:80/html/html-tutorial.html
	http
	www.runoob.com
	:80
	/html/html-tutorial.html

##元字符
### 下表包含了元字符的完整列表以及它们在正则表达式上下文中的行为：
[http://www.runoob.com/regexp/regexp-metachar.html](http://www.runoob.com/regexp/regexp-metachar.html)
##运算符优先级
运算符|描述
-|-
\|转义字符
(), (?:), (?=), []|圆括号和方括号
*, +, ?, {n}, {n,}, {n,m}|限定符
^, $, \任何元字符、任何字符|定位点和序列（即：位置和顺序）
最后
|
替换，"或"操作
字符具有高于替换运算符的优先级，使得"m|food"匹配"m"或"food"。若要匹配"mood"或"food"，请使用括号创建子表达式，从而产生"(m|f)ood"。
## 字符簇

	[a-z] //匹配所有的小写字母 
	[A-Z] //匹配所有的大写字母 
	[a-zA-Z] //匹配所有的字母 
	[0-9] //匹配所有的数字 
	[0-9\.\-] //匹配所有的数字，句号和减号 
	[ \f\r\t\n] //匹配所有的白字符