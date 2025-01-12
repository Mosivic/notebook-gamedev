
#### 图片某处是否透明
**bool is_pixel_opaque ( Vector2 pos ) const**
Returns true, if the pixel at the given position is opaque and false in other case.
Note: It also returns false, if the sprite's texture is null or if the given position is invalid.

```python
if Sprite.is_pixel_opaque(get_global_mouse_position()):
    prints('鼠标点击在不透明处')
```


#### 获取帧间隔时间
```python
var delta := self.get_physics_process_delta_time()
        self.move_and_collide(Vector2(100, 0) * delta)
```


#### 控制窗口关闭行为
get_tree().set_auto_accept_quit(false)
```python
func _ready() -> void:
    get_tree().set_auto_accept_quit(false) # 取消自动关闭行为


func _notification(what):
    if what == MainLoop.NOTIFICATION_WM_QUIT_REQUEST:
        get_tree().quit() # 手动控制退出
```


#### 打开应用程序
**OS ->Error shell_open ( String uri )**
Requests the OS to open a resource with the most appropriate program. For example:
* OS.shell_open("C:\\Users\name\Downloads") on Windows opens the file explorer at the user's Downloads folder.    打开资源浏览器/应用
* OS.shell_open("https://godotengine.org") opens the default web browser on the official Godot website.          打开网页
* OS.shell_open("mailto:example@example.com") opens the default email client with the "To" field set to example@example.com.    打开邮件发送


#### 设置窗口标题
OS.set_window_title(String title)


#### 设置窗口是否全屏
  OS.set_window_fullscreen


#### 设置窗口分辨率
 OS.set_window_size


#### 设置窗口是否最小化 ####
OS.set_window_minimized(true) 

###### 文件拖入窗口
``` js
func on_file_drag(files,screen) virtual
```

###### 判断node是否已经被释放/删除
``` js
对于引用类型:
var wr = weakref(node) 
if (!wr.get_ref()):
# freed 
else:
# not freed

对于一般:
is_instance_valid(node)

```

###### 输出err
To display errors in the debug panel:

``` js
push_error("string")
push_warning("string")
```

To display errors in the output panel:

``` js
printerr("string")
```
