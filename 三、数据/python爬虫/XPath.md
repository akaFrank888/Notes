# XPath

## 一、简介

> XPath（XML Path Language），即xml路径语言，通过使用路径表达式在xml中进行导航。
>
> XPath解析：是最常用且最便捷高效的一种解析方式，而且通用。



## 二、解析步骤

1. 对目标页面的源码进行解析，并将结果封装到etree对象中。

   > 实例化etree的方式：
   >
   > ​			etree.parse(path) 				--> 		本地html文档
   >
   > ​			etree.HTML('page_text')      -->        互联网上的源码

2. 调用etree对象中的xpath方法，并结合xpath表达式实现 **标签的定位和内容的捕获。**

   > xpath('xpath表达式')



## 三、XPath表达式

#### （1）/

- 表示从根节点开始定位
- 表示一个层级

#### （2）//

- 表示从任意位置开始定位
- 表示多个层级

#### （3）属性定位：     

​			//div[@class="***"]

#### （4）索引定位：

​		    //div[@class="***"/p[3]]  （索引是从1开始的）

#### （5）取文本：

- /text()：获取标签中直系的文本内容
- //text()：标签中非直系的文本内容

#### （6）取属性：

- /@attrName     ，如/src



```python

from lxml import etree
if __name__ == "__main__":
    #实例化好了一个etree对象，且将被解析的源码加载到了该对象中
    tree = etree.parse('test.html')
    # r = tree.xpath('/html/body/div')
    # r = tree.xpath('/html//div')
    # r = tree.xpath('//div')
    # r = tree.xpath('//div[@class="song"]')
    # r = tree.xpath('//div[@class="tang"]//li[5]/a/text()')[0]
    # r = tree.xpath('//li[7]//text()')
    # r = tree.xpath('//div[@class="tang"]//text()')
    r = tree.xpath('//div[@class="song"]/img/@src')

    # print(r)

```



```html
<html lang="en">
<head>
	<meta charset="UTF-8" />
	<title>测试bs4</title>
</head>
<body>
	<div>
		<p>百里守约</p>
	</div>
	<div class="song">
		<p>李清照</p>
		<p>王安石</p>
		<p>苏轼</p>
		<p>柳宗元</p>
		<a href="http://www.song.com/" title="赵匡胤" target="_self">
			<span>this is span</span>
		宋朝是最强大的王朝，不是军队的强大，而是经济很强大，国民都很有钱</a>
		<a href="" class="du">总为浮云能蔽日,长安不见使人愁</a>
		<img src="http://www.baidu.com/meinv.jpg" alt="" />
	</div>
	<div class="tang">
		<ul>
			<li><a href="http://www.baidu.com" title="qing">清明时节雨纷纷,路上行人欲断魂,借问酒家何处有,牧童遥指杏花村</a></li>
			<li><a href="http://www.163.com" title="qin">秦时明月汉时关,万里长征人未还,但使龙城飞将在,不教胡马度阴山</a></li>
			<li><a href="http://www.126.com" alt="qi">岐王宅里寻常见,崔九堂前几度闻,正是江南好风景,落花时节又逢君</a></li>
			<li><a href="http://www.sina.com" class="du">杜甫</a></li>
			<li><a href="http://www.dudu.com" class="du">杜牧</a></li>
			<li><b>杜小月</b></li>
			<li><i>度蜜月</i></li>
			<li><a href="http://www.haha.com" id="feng">凤凰台上凤凰游,凤去台空江自流,吴宫花草埋幽径,晋代衣冠成古丘</a></li>
		</ul>
	</div>
</body>
</html>
```





