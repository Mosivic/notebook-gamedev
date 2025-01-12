### 描述
EditorInterface使您可以控制Godot编辑器的窗口。它允许自定义窗口、保存和（重新）加载场景、渲染网格预览、检查和编辑资源和对象，并提供对编辑器设置、编辑器文件系统、编辑器资源预览、脚本编辑器、编辑器视口和有关场景的信息的访问。
(inspecting and editing resources and objects, and provides access to EditorSettings, EditorFileSystem, EditorResourcePreview, ScriptEditor, the editor viewport, and information about scenes.)
注意：这个类不应该直接实例化。相反，使用EditorPlugin.get_editor_interface()获取这个单例

### 属性
*  bool distraction_free_mode
	*  set_distraction_free_mode(value) setter
	*  is_distraction_free_mode_enabled() getter
	*  如果为true，则启用无干扰模式(distraction_free_mode)，该模式隐藏侧坞(Side Dock)以增加主视图的可用空间。

### 方法
* void edit_node(node: Node)
	* 编辑给定节点。如果节点位于场景树中，也将选中该节点。
* void edit_resource(resource: Resource)
	* 编辑给定资源
---
* Control  get_base_control()
	* 返回Godot编辑器窗口的主容器。例如，您可以使用它来检索容器的大小并相应地放置控件。
	* 警告：删除和释放此节点将使编辑器无效，并可能导致崩溃。
* String  get_current_path() const 
	* 获取当前文件路径(FileSystem)
* Node  get_edited_scene_root()
	* 返回当前编辑场景的根节点
* float  get_editor_scale() const 
	* 获取编辑器UI缩放大小(1.0为100%缩放)
* EditorSettings get_editor_settings()
	* 返回EditorSettings实例
* Control get_editor_viewport()
	* 返回主编辑器控件。将其用作主屏幕的父屏幕。
	* 注意：这将返回包含整个编辑器的主编辑器控件，而不是特定的二维或三维视口。
	* 警告：删除和释放此节点将使编辑器的一部分无效，并可能导致崩溃。
* FileSystemDock get_file_system_dock()
	* * 返回FileSystemDock实例
* EditorInspector get_inspector() const
	* 返回 EditorInspector实例
	* 警告：删除和释放此节点将使编辑器的一部分无效，并可能导致崩溃
* Array get_open_scenes() const
	* 返回当前打开的场景以Array
* String get_playing_scene() const
	* 返回当前运行的场景,如果没有插件在运行,返回空字符串
* EditorFileSystem get_resource_filesystem()
	* 返回 EditorFileSystem实例
* EditorResourcePreview get_resource_previewer()
	* 返回EditorResourcePreview实例
* ScriptEditor get_script_editor()
	* 返回ScriptEditor实例
	* 警告：删除和释放此节点将使编辑器的一部分无效，并可能导致崩溃
* String get_selected_path() const
	* 返回当前在FileSystemDock中选择的文件路径, 如果文件被选择, 将使用String.get_base_dir()代替返回路径
* EditorSelection get_selection()
	* 返回EditorSelection实例
---
* void inspect_object(object: Object, for_property: String = "", inspector_only: bool = false)
	* 显现选定物体在编辑器检测栏选定的属性, 如果 inspector_only 为 true, 插件则无法编辑物体
---
* bool is_playing_scene() const
	* 如果当前正在播放场景，则返回true，否则返回false。暂停的场景被视为正在播放。
* bool is_plugin_enabled(plugin: String) const
	* 如果指定的插件已启用，则返回true。插件名与其目录名相同。
---
* Array make_mesh_previews(meshes: Array, preview_size: int)
	* 返回以给定大小渲染为纹理数组的网格预览。
* void open_scene_from_path(scene_filepath: String)
	* 打开给定路径场景
* void play_current_scene()voidplay_custom_scene(scene_filepath: String)
	* 启动当前激活场景
* void play_main_scene()voidreload_scene_from_path(scene_filepath: String)
	* 启动指定路径场景
* Error save_scene()
	* 保存场景, 返回 OK 或 ERR_CANT_CREAT
* void save_scene_as(path: String, with_preview: bool = true)
	* 保存场景在指定路径
* void select_file(file: String)
	* 选择在FileSystem Dock中的文件
* void set_main_screen_editor(name: String)
	* 将编辑器的当前主屏幕设置为名称中指定的主屏幕。名称必须与相关选项卡的文本完全匹配（2D、3D、脚本、AssetLib）。
* void set_plugin_enabled(plugin: String, enabled: bool)
	* 设置插件的启用状态。插件名与其目录名相同。
* void stop_playing_scene()
	* 停止当前启动的场景
