# Resource 资源详解 #

## 描述 ##

在本教程之前，我们关注的是Godot中的Node类，因为它是您用来编写行为代码的类，并且大多数引擎功能都依赖于它。还有另一种同样重要的数据类型：资源。

节点为你提供了功能：它们绘制精灵、三维模型、模拟物理、安排用户界面等。资源是数据容器。它们自己不做任何事情：相反，节点使用资源中包含的数据。

Godot从磁盘保存或加载的任何内容都是一种资源。无论是一个场景（一个.tscn或一个.scn文件），一个图像，一个脚本。。。以下是一些资源示例：纹理、脚本、网格、动画、音频流、字体、翻译。

当引擎从磁盘加载资源时，它只加载一次。如果内存中已存在该资源的副本，则尝试再次加载该资源时，每次都会返回相同的副本。由于资源只包含数据，因此不需要复制它们。

每个对象，无论是节点还是资源，都可以导出属性。有很多类型的属性，比如String、integer、Vector2等，**这些类型中的任何一个都可以成为资源**。这意味着节点和资源都可以包含资源作为属性

### 存在形式：外部与内置 ###

有两种方法可以节省资源。它们可以是：
1.在场景外部，作为单独的文件保存在磁盘上，形式为tres或res。
2.在场景文件内，保存在.tscn或它们所附加的.scn文件中。



Resource是所有Godot特定资源类型的基类，主要用作数据容器。与对象不同，它们是**reference-counted**的，**不再使用时释放**。一旦从磁盘加载，它们也会被缓存，因此任何进一步从给定路径加载资源的尝试都将返回相同的引用（这与节点不同，节点不计算引用，可以根据需要从磁盘实例化多次）。**资源可以保存在外部磁盘上，也可以绑定到另一个对象中，例如节点或其他资源。**

 ## 基本使用 ## 

### Resource资源的获取与保存

引用类型, 可以设置路径获取到

```python
var res:Resource
res.set_path(path:String)

```

ResourceLoader资源加载 

```python
#获取路径资源
ResourceLoader.load("res://someresource.res")
```

ResourceSaver资源保存，保存到本地

```python
#将res 资源保存到本地路径的资源
ResourceSaver.save("res://someresource.res", res) 
```

### 其他类型资源的获取 ###

图片，声音等

```python
var res = load("res://robi.png") # Godot loads the Resource when it reads the line.
var res = preload("res://robi.png") # Godot loads the resource at compile-time
get_node("sprite").texture = res
```

场景tscn

```python
var bullet = preload("res://bullet.tscn").instance()
add_child(bullet)
```

## 自定义Resource ##

```python
# 继承自 Resource 说明这是一个资源脚本
extends Resource
class_name CustomResource, 'res://CustomResource/custom_icon.svg'

# 资源也可以定义普通的属性
export var variable1 := ''
export var variable2 := 0
export(Resource) var sub_res #资源可以嵌套
# ...

# 资源也可以定义一些方法
func _init():  #初始化资源操作
    pass
func printInfo() -> void:
    # ...
```

## 注意 ##

### 1. 不能使用自定义 Resource 为变量类型,只能以父类 Resource 为变量类型 ###

### 2.**使用 Resouce 要注意资源是引用类型** ###

### 3.自定义 Resource 资源时属性使用 export ，否则资源的创建时无法包含其属性的 ###

### 4.子类定义的资源是无效的 ###

请注意，**资源文件（*.tres/*.res）将存储它们在文件中使用的脚本的路径**。加载时，它们将获取并加载此脚本作为其类型的扩展。这意味着试图分配一个子类，即脚本的一个内部类（例如在GDScript中使用class关键字）是行不通的。Godot不会正确序列化脚本子类上的自定义属性。

在下面的示例中，Godot将加载节点脚本，查看它没有扩展资源，然后确定由于类型不兼容，脚本无法为资源对象加载。

```python
extends Node

class MyResource:
    extends Resource
    export var value = 5

func _ready():
    var my_res = MyResource.new()

    # This will NOT serialize the 'value' property.
    ResourceSaver.save("res://my_res.tres", my_res)
```

## 使用：以Resource 来管理脚本资源 ##

```python
# bot_stats_table.gd
extends Resource

const BotStats = preload("bot_stats.gd")

var data = {
    "GodotBot": BotStats.new(10), # Creates instance with 10 health.
    "DifferentBot": BotStats.new(20) # A different one with 20 health.
}

func _init():
    print(data)
```

