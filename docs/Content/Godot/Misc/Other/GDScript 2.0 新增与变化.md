#GDScript
### 新增: Lamada函数
``` js
//1
var my_lambda = func(x): 
	print(x) 
my_lambda.call("hello")

//2
button_down.connect(func(): print("button was pressed"))

//3
var my_lambda = func this_is_lambda(x): 
	print("Hello") 
	print("This is %s" % x)
```

### 新增：类型数组
``` js
var my_array: Array[int] = [1, 2, 3]
```

### 新增：内建类型静态方法
``` js
var x = Color.html_is_valid("00ffff") # true
```
### 改变：@修饰
``` js
//变化，前缀@
export -> @export
tool -> @tool
//新增
@icon
@rpc
@onready 绑定一个函数在ready阶段执行
@warning_ignore
```
### 改变：await关键字
``` js
yield -> await
```

### 改变：Callable方法与Signal机制
### 改变： instantiate创建
``` js
instance() -> instantiate()
```

### 改变：Control组件
``` js
1. delete offset property
2. min_rect_size -> minum_size
```