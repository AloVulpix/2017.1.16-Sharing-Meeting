# 2017.1.16-Sharing-Meeting  
主要的内容有：  
1. 常用的数据交换格式与处理接口；  
2. 集合、树、列表、队列、Map的迭代与排序算法；  
3. 数据的分组计算与比例尺映射；  
4. 应用1 ：深度优先、广度优先遍历的差异与应用场景；  
5. 应用2 : 树形结构的两种表现形式与遍历方式；  
## 1、常用的数据交换格式与处理接口  
主要有json，xml，yml  
### json，规则包括：  
* 并列数据之间用逗号（,）分隔  
* 映射用冒号（:）表示  
* 并列数据的集合用方括号（[]）表示  
* 映射的集合用大括号（{}）表示  
如：
```
{
  "name": "STI",
  "age": 1,
  "member": ["LiZhengHua", "ZhengHuaLi", "HuaLiZheng"]
}
```
在javascript中，使用JSON.stringify和JSON.parse能够方便的在Object和JSON之间转换  
```
let sti = {
  "name": "STI",
  "age": 1,
  "member": ["lizhenghua", "zhenghuali", "hualizheng"]
};

//Object -> JSON
let stiJson = JSON.stringify(sti);   

//JSON -> Object
let stiObject = JSON.parse(stiJson)
```

### xml  
一、规则包括： 
*  XML 是一种标记语言，很类似 HTML
*  XML 的设计宗旨是传输数据，而非显示数据
*  XML 标签没有被预定义。您需要自行定义标签。
*  XML 被设计为具有自我描述性。
*  XML 是 W3C 的推荐标准

```
<sti>
<name>STI</name>
<age>1</age>
<member>
  <lizhenghua></lizhenghua>
  <zhenghuali></zhenghuali>
  <hualizheng></hualizheng>
</member>
</sti>
```  

* 大小写敏感  
* 正确嵌套  
* 可配置属性  

二、解析的语法  
```

let xmlhttp=new XMLHttpRequest();

xmlhttp.open("GET","/example/xmle/note.xml",false);
xmlhttp.send();
xmlDoc=xmlhttp.responseXML;

//解析得到XML DOM，可以像处理DOM一样处理  

document.getElementById("to").innerHTML = xmlDoc.getElementsByTagName("to")[0].childNodes[0].nodeValue;
document.getElementById("from").innerHTML = xmlDoc.getElementsByTagName("from")[0].childNodes[0].nodeValue;
document.getElementById("message").innerHTML = xmlDoc.getElementsByTagName("body")[0].childNodes[0].nodeValue;  

```  

* 注意：XML DOM和HTML DOM是不同的数据类型  
XPath:用来确定XML文档中某部分位置，如：

```
/bookstore  = xmlDoc.getElementByTagName("bookstore")
```
