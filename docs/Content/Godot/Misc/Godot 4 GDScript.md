
## 一、新特性
### 1.文档注释功能
### 2.Performance对象
## 二、可替代变化 
### 1.Callable对象
用于代替funcref

var callable1 = Callable(self,'do_something')
var callable2 = do_something
callable1.call()
callable2.call()
### 2.await关键字
代替yield函数
### 3.super关键字
父级调用
## 4.场景加载函数

## 5.@符号
@export
@onready
@export_range
### 6.connect函数
更改为可以直接从对象上调用具体信号，再进行connect
![[Pasted image 20230529214917.png]]
### 7.tween对象
## 三、其他
### 1.可以将自定义资源作为@export
### 2.以指定Array的类型
### 3.YSort节点直接作为CanvasItem的功能
## 四、Bug相关
### 解决循环引用问题