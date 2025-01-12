### 案例代码
``` js

a.gd:
extends EditorInspectorPlugin


# 虚方法重写
# 如果此插件可以处理此对象返回 true。
# 只处理有 "_parse_begin" 方法的对象
func can_handle(object):
	if object.has_method("_parse_begin"):
		return true
	return false

# 虚方法重写
# 允许被调用在列表的开头添加控件。
# 调用对象自身的方法处理
func parse_begin(object):
	if object.has_method("_parse_begin"):
		object._parse_begin(self)

b.gd:
# 处理对象的中方法
func _parse_begin(plugin:EditorInspectorPlugin):
	# 创建标题栏
	plugin.add_custom_control(UIBuilder.create_category_header())
	# 创建节点类型选择
	plugin.add_custom_control(HSeparator.new())
	
	var vbox = HBoxContainer.new()
	var dropdown = OptionButton.new()
	for key in BTClassBD.BTNodeClass:
		dropdown.add_item(key)
	vbox.add_child(dropdown)
	var createNodebutton = Button.new()
	createNodebutton.text = "Create"
	createNodebutton.connect("pressed",self,"_on_createNodeButton_pressed",[dropdown])
	createNodebutton.size_flags_horizontal = Control.SIZE_EXPAND + Control.SIZE_SHRINK_END
	vbox.add_child(createNodebutton)
	plugin.add_custom_control(vbox)
	
	plugin.add_custom_control(HSeparator.new())
	# 创建保存与加载按钮
	var hbox = HBoxContainer.new()
	var saveButton = Button.new()
	var loadButton = Button.new()
	saveButton.text = "Save"
	loadButton.text = "Load"
	saveButton.connect("button_down",ui,"_on_SaveBtn_pressed")
	loadButton.connect("button_down",ui,"_on_LoadBtn_pressed")
	hbox.add_child(saveButton)
	hbox.add_child(loadButton)
	plugin.add_custom_control(hbox)
	plugin.add_custom_control(HSeparator.new())
	# 创建构建Actions按钮
	var buildActionsButtion = Button.new()
	buildActionsButtion.text = "Build Actions"
	buildActionsButtion.connect("button_down",self,"build_actions")
	plugin.add_custom_control(buildActionsButtion)
```

### API 使用效果:
#### 插入到头部 parse_begin

案例代码演示:
![[Pasted image 20220720082719.png]]


#### 插入到指定目录  parse_category
![[Pasted image 20220720083244.png]]


#### 插入到指定属性 parse_property
![[Pasted image 20220720083556.png]]

#### 添加自定义控件 add_custom_control
#### 添加自定义属性  add_property_editor
1. 自定义属性
![[Pasted image 20220720083920.png]]
2. 解析插入
 ![[Pasted image 20220720083945.png]]
3. 效果
 ![[Pasted image 20220720084214.png]]