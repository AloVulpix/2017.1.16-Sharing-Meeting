# 2017.1.16-Sharing-Meeting  
主要的内容有：  
1. 常用的数据交换格式与处理接口；  
2. 集合、树、列表、Map的迭代与排序算法；   
3. 应用1 ：深度优先、广度优先遍历的差异与应用场景；  
4. 应用2 : 树形结构的两种表现形式与遍历方式；  
## 1、常用的数据交换格式与处理接口  
有json，xml，yml，主要用于数据交换、配置文件等  
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

* 注意：XML DOM可以直接插入到HTML DOM树中  

XPath:用来确定XML文档中某部分位置，如：

```
/bookstore  = xmlDoc.getElementByTagName("bookstore")
```  

### YAML  
一、基本规则  

* 大小写敏感  
* 使用缩进表示层级关系  
* 缩进时不允许使用Tab键，只允许使用空格。  
* 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可  

二、例子  
```
languages:
 - Ruby
 - Perl
 - Python 
websites:
 YAML: yaml.org 
 Ruby: ruby-lang.org 
 Python: python.org 
 Perl: use.perl.org
 
 //等价于
 
{ 
  languages: [ 'Ruby', 'Perl', 'Python' ],
  websites: { 
    YAML: 'yaml.org',
    Ruby: 'ruby-lang.org',
    Python: 'python.org',
    Perl: 'use.perl.org' 
  } 
}
 ```  
 
*  JavaScript需要借助第三方库来解析，比如js-yaml  

## 2. 集合、树、列表、Map的迭代与排序算法  

### set  

一、基本概念  
集合类似于数组，但是成员的值都是唯一的，没有重复的值。

```javascript
var s = new Set();

s.add(2);
s.add(3);
s.add(5);
s.add(4);
s.add(5);
s.add(2);
s.add(2);

for (let i of s) {
  console.log(i);
}
//2 3 5 4

//操作方法：add、delete、has、clear
```

二、遍历  

*　在数据结构的定义中，set是无序的，即通过iterator输出，每次的顺序可能会不同（比如java）  
*　但是在javascript的描述中：Set objects are collections of values. You can iterate through the elements of a set in insertion order. 

keys：返回键名key的iterator  
values：返回值value的iterator  
entries：返回key-value对的iterator  
forEach：forEach(function(value, key){})  

* 可通过Array.from()转为Array，通过sort排序

### map  
一、基本概念
本质上是键值对的集合，键名可以为任意类型。在JavaScript中，是一个对象Object，但是Object只接受字符串作为键名，因此比Object要强大

```javascript
let keys = {}
var map = new Map([
  ['name', '张三'],
  ['title', 'Author'],
  [keys, 'Object']
]);

//操作方法：get，set，has，clear，delete
```  
二、遍历  
同样是无序的。在JavaScript中：A Map object iterates its elements in insertion order — a for...of loop returns an array of [key, value] for each iteration.  
同样有keys、values、entries、forEach方法  
三、排序  
没有原生的key/value排序的方法（其实就是Object排序）  

```javascript
//按key排序的方法：
var map = new Map();
		map.set('2-1', "foo");
		map.set('0-1', "bar");
		map.set('3-1', "baz");

		var mapAsc = new Map([...map.entries()].sort());

		console.log(mapAsc)；

//按value排序的方法
var maxSpeed = {
    car: 300, 
    bike: 60, 
    motorbike: 200, 
    airplane: 1000,
    helicopter: 400, 
    rocket: 8 * 60 * 60
}
var sortable = [];
for (var vehicle in maxSpeed)
    sortable.push([vehicle, maxSpeed[vehicle]])

sortable.sort(function(a, b) {
    return a[1] - b[1]
})
```

### Array
javascript没有queue、list的实现，可以进行模拟  

### ES6的Iterator  
数组、伪数组、set、map原生具备iterator
```
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
```

* 只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）  
* Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器  
* 通过next方法遍历  

## 3. 树与JavaScript常见的两种结构
一、 Array，通过parent id对关系进行标识

```javascript
let tree = [{
  id: 1,
  text: 'msg1',
  pid: '#'
}, {
  id: 2,
  text: 'msg2',
  pid: 1
}, {
  id: 3,
  text: 'msg3',
  pid: 1,
}, {
  id: 4,
  text: 'msg4',
  pid: 2
}, {
  id: 5,
  text: 'msg5',
  pid: 2
}]
```

二、 Object，通过children对关系进行标识  
```javascript
let tree = {
  id: 1,
  text: 'msg1',
  children: [{
    id: 2,
    text: 'msg2',
    children: [{
      id: 4,
      text: 'msg4',
      children:[] 
    }, {
      id: 5,
      text: 'msg5',
      children:[] 
    }]
  }, {
    id: 3,
    text: 'msg3',
    children: []
  }]
}
```

## 4. 深度遍历(DFS)和广度遍历(BFS)
![树](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1f/Depth-first-tree.svg/390px-Depth-first-tree.svg.png)  
DFS: 1-2-3-4-5-6-7-8-9-10-11-12  
BFS：1-2-7-8-3-6-9-12-4-5-10-11  

两种树结构的DFS和BFS的实现：  

1. Array，BFS

```
let BFSList = [{
		  id: 1,
		  text: 'msg1',
		  pid: '#'
		}, {
		  id: 2,
		  text: 'msg2',
		  pid: 1
		}, {
		  id: 7,
		  text: 'msg7',
		  pid: 1,
		}, {
		  id: 8,
		  text: 'msg8',
		  pid: 1
		}, {
		  id: 3,
		  text: 'msg3',
		  pid: 2
		}, {
		  id: 6,
		  text: 'msg6',
		  pid: 2
		}, {
		  id: 9,
		  text: 'msg9',
		  pid: 8
		}, {
		  id: 12,
		  text: 'msg12',
		  pid: 8
		}, {
		  id: 4,
		  text: 'msg4',
		  pid: 3
		}, {
		  id: 5,
		  text: 'msg5',
		  pid: 3
		}, {
		  id: 10,
		  text: 'msg10',
		  pid: 9
		}, {
		  id: 11,
		  text: 'msg11',
		  pid: 9
		}];

		let searchQueue = [];

		//找根节点
		for(let i = 0; i < BFSList.length; i++){
			if(BFSList[i].pid == '#'){
				searchQueue.push(BFSList[i])
				BFSList.splice(i, 1);
				break;
			}
		}

		//广度优先
		function BFS(){
			while(searchQueue.length != 0){
				let node = searchQueue.shift();
				console.log(node.text);
				for(let i = 0; i < BFSList.length; i++){
					if(BFSList[i].pid == node.id){
						searchQueue.push(BFSList[i])
						BFSList.splice(i, 1);
						i--;
					}
				}
			}
		}
		BFS();
```

2. Array DFS

```
//DFS
		let DFSList = [{
			  id: 1,
			  text: 'msg1',
			  pid: '#'
			}, {
			  id: 2,
			  text: 'msg2',
			  pid: 1
			}, {
			  id: 7,
			  text: 'msg7',
			  pid: 1,
			}, {
			  id: 8,
			  text: 'msg8',
			  pid: 1
			}, {
			  id: 3,
			  text: 'msg3',
			  pid: 2
			}, {
			  id: 6,
			  text: 'msg6',
			  pid: 2
			}, {
			  id: 9,
			  text: 'msg9',
			  pid: 8
			}, {
			  id: 12,
			  text: 'msg12',
			  pid: 8
			}, {
			  id: 4,
			  text: 'msg4',
			  pid: 3
			}, {
			  id: 5,
			  text: 'msg5',
			  pid: 3
			}, {
			  id: 10,
			  text: 'msg10',
			  pid: 9
			}, {
			  id: 11,
			  text: 'msg11',
			  pid: 9
			}];

		let visitedMAP = new Map(),
			root = undefined;
		
		//找根节点
		for(let i = 0; i < DFSList.length; i++){
			if(DFSList[i].pid == '#'){
				visitedMAP.set(DFSList[i], true);
				root = DFSList[i];
				break;
			}
		}

		//深度优先
		function DFS(node){
			console.log(node.text)
			for(let i = 0; i < DFSList.length; i++){
				//判断是否访问过以及是不是需要找的点
				if(DFSList[i].pid == node.id && !visitedMAP[DFSList[i]]){
					visitedMAP.set(DFSList[i], true);
					DFS(DFSList[i]);
				}
			}
		}
		DFS(root);
```

3. Object BFS

```
//BFS
	 	let BFSTree = {
		  id: 1,
		  text: 'msg1',
		  children: [{
			    id: 2,
			    text: 'msg2',
			    children: [{
				      id: 3,
				      text: 'msg3',
				      children:[{
					      	id: 4,
					      	text: 'msg4',
					      	children: []
					      }, {
					      	id: 5,
					      	text: 'msg5',
					      	children: []
					      }] 
				    }, {
				      id: 6,
				      text: 'msg6',
				      children:[] 
				    }]
			  }, {
			    id: 7,
			    text: 'msg7',
			    children: []
			  }, {
			    id: 8,
			    text: 'msg8',
			    children: [{
			      	id: 9,
			      	text: 'msg9',
			      	children: [{
				      	id: 10,
				      	text: 'msg10',
				      	children: []
				      }, {
				      	id: 11,
				      	text: 'msg11',
				      	children: []
				      }]
			      }, {
			      	id: 12,
			      	text: 'msg12',
			      	children: []
			      }]
			  }]
		}

		searchQueue = [];
		searchQueue.push(BFSTree);

		while(searchQueue.length != 0){
			let node = searchQueue.shift();
			console.log(node.text);
			if(node.children.length != 0){
				for(let i = 0; i < node.children.length; i++){
					searchQueue.push(node.children[i]);
				}
			}
		}
```

4. Object DFS

```
//DFS
		let DFSTree = {
		  id: 1,
		  text: 'msg1',
		  children: [{
			    id: 2,
			    text: 'msg2',
			    children: [{
				      id: 3,
				      text: 'msg3',
				      children:[{
					      	id: 4,
					      	text: 'msg4',
					      	children: []
					      }, {
					      	id: 5,
					      	text: 'msg5',
					      	children: []
					      }] 
				    }, {
				      id: 6,
				      text: 'msg6',
				      children:[] 
				    }]
			  }, {
			    id: 7,
			    text: 'msg7',
			    children: []
			  }, {
			    id: 8,
			    text: 'msg8',
			    children: [{
			      	id: 9,
			      	text: 'msg9',
			      	children: [{
				      	id: 10,
				      	text: 'msg10',
				      	children: []
				      }, {
				      	id: 11,
				      	text: 'msg11',
				      	children: []
				      }]
			      }, {
			      	id: 12,
			      	text: 'msg12',
			      	children: []
			      }]
			  }]
		}

		visitedMAP = new Map();
		visitedMAP.set(DFSTree, true);
		function DFST(node){
			console.log(node.text);
			if(node.children.length != 0){
				for(let i = 0; i < node.children.length; i++){
					if(!visitedMAP[node.children[i]]){
						visitedMAP.set(node.children[i], true);
						DFST(node.children[i])
					}
				}
			}
		}

		DFST(DFSTree);
```

5. 两种树结构的转换  
Array -> Object  

```
// array -> object
	let List = [{
			  id: 1,
			  text: 'msg1',
			  pid: '#'
			}, {
			  id: 2,
			  text: 'msg2',
			  pid: 1
			}, {
			  id: 7,
			  text: 'msg7',
			  pid: 1,
			}, {
			  id: 8,
			  text: 'msg8',
			  pid: 1
			}, {
			  id: 3,
			  text: 'msg3',
			  pid: 2
			}, {
			  id: 6,
			  text: 'msg6',
			  pid: 2
			}, {
			  id: 9,
			  text: 'msg9',
			  pid: 8
			}, {
			  id: 12,
			  text: 'msg12',
			  pid: 8
			}, {
			  id: 4,
			  text: 'msg4',
			  pid: 3
			}, {
			  id: 5,
			  text: 'msg5',
			  pid: 3
			}, {
			  id: 10,
			  text: 'msg10',
			  pid: 9
			}, {
			  id: 11,
			  text: 'msg11',
			  pid: 9
			}];

		//直接用DFS
		visitedMAP = new Map();
        root = undefined;

        let obj = {}

        //找根节点
        for(let i = 0; i < List.length; i++){
            if(List[i].pid == '#'){
                visitedMAP.set(List[i], true);
                obj.id = List[i].id;
                obj.text = List[i].text;
                obj.children = [];
                root = List[i];
                break;
            }
        }

        //深度优先
        function Array2Object(node){
            node.children = [];
            for(let i = 0; i < List.length; i++){
                //判断是否访问过以及是不是需要找的点
                if(List[i].pid == node.id && !visitedMAP[List[i]]){
                    visitedMAP.set(List[i], true);
                    node.children.push(List[i])
                    Array2Object(List[i]);
                }
            }
        }
        Array2Object(obj);
        console.log(obj);
```

Object -> Array  

```
太简单，不写了
```
