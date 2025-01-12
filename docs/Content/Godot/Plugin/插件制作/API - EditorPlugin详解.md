### 描述
编辑器使用插件来扩展功能。最常见的插件类型是那些编辑给定节点或资源类型、导入插件和导出插件的插件。

### 方法
#### 虚方法,主要对插件的一些设置
 * void apply_changes() virtual
	* 当编辑器即将保存项目、切换到另一个标签页卡(Tab)等时，会调用此方法。它要求插件应用任何挂起的状态更改以确保一致性。
	* 例如，在着色器编辑器中使用此选项可以让插件知道它必须将用户编写的着色器代码应用于对象。
* bool build() virtual
	* 当编辑器即将运行项目时，将调用此方法。插件可以在项目运行之前执行所需的操作。
* void clear() virtual
	*	清除所有状态并将正在编辑的对象重置为零。这可确保插件不会继续编辑当前存在的节点，或编辑错误场景中的节点。
* void disable_plugin() virtual
	*	当用户在“项目设置”窗口的“插件”选项卡中禁用EditorPlugin时，由引擎调用。
* void enable_plugin() virtual
	*	当用户在“项目设置”窗口的“插件”选项卡中启用EditorPlugin时，由引擎调用。
* void edit(object: Object) virtual
	*	此函数用于编辑特定对象类型（节点或资源）的插件。它要求编辑器编辑给定的对象。
* void forward_canvas_draw_over_viewport(overlay: Control) virtual
	*	更新2D编辑器的视口时由引擎调用。对图形使用覆盖控件。可以通过调用update_overlays（）手动更新视口。
``` js
func forward_canvas_draw_over_viewport(overlay):
    # Draw a circle at cursor position.
    overlay.draw_circle(overlay.get_local_mouse_position(), 64, Color.white)

func forward_canvas_gui_input(event):
    if event is InputEventMouseMotion:
        # Redraw viewport when cursor is moved.
        update_overlays()
        return true
    return false

```
* void forward_canvas_force_draw_over_viewport(overlay: Control) virtual
	*	此方法与上方法上绘制相同，只是它在所有对象的顶部绘制。当您需要一个额外的图层来显示其他任何内容时，此功能非常有用。
您需要使用set_force_draw_over_forwarding_enabled（）启用此方法的调用。
* bool forward_canvas_gui_input(event: InputEvent) virtual
	*	当当前编辑的场景中存在根节点时调用，将实现handles（），并在2D视口中发生InputEvent。如果返回真 EditorPlugin使用该事件，则截取InputEvent，否则将事件转发给其他编辑器类。例子：
``` js
# Prevents the InputEvent to reach other Editor classes
func forward_canvas_gui_input(event):
    var forward = true
    return forward
```
必须返回false才能将InputEvent转发到其他编辑器类。例子：
``` js
# Consumes InputEventMouseMotion and forwards other InputEvent types
func forward_canvas_gui_input(event):
    var forward = false
    if event is InputEventMouseMotion:
        forward = true
    return forward
```
*  void forward_spatial_draw_over_viewport(overlay: Control) virtual
	* Called by the engine when the 3D editor's viewport is updated. Use the overlay Control for drawing. You can update the viewport manually by calling update_overlays().
``` js
func forward_spatial_draw_over_viewport(overlay):
    # Draw a circle at cursor position.
    overlay.draw_circle(overlay.get_local_mouse_position(), 64)

func forward_spatial_gui_input(camera, event):
    if event is InputEventMouseMotion:
        # Redraw viewport when cursor is moved.
        update_overlays()
        return true
    return false
``` 
*   void forward_spatial_force_draw_over_viewport(overlay: Control) virtual
	*	This method is the same as forward_spatial_draw_over_viewport(), except it draws on top of everything. Useful when you need an extra layer that shows over anything else.
	*	You need to enable calling of this method by using set_force_draw_over_forwarding_enabled().
* bool forward_spatial_gui_input(camera: Camera, event: InputEvent) virtual
	*	Called when there is a root node in the current edited scene, handles() is implemented and an InputEvent happens in the 3D viewport. Intercepts the InputEvent, if return true EditorPlugin consumes the event, otherwise forwards event to other Editor classes. Example:
``` js
# Prevents the InputEvent to reach other Editor classes
func forward_spatial_gui_input(camera, event):
    var forward = true
    return forward

Must return false in order to forward the InputEvent to other Editor classes. Example:

# Consumes InputEventMouseMotion and forwards other InputEvent types
func forward_spatial_gui_input(camera, event):
    var forward = false
    if event is InputEventMouseMotion:
        forward = true
    return forward
```
* PoolStringArray get_breakpoints() virtual
	* 这适用于编辑基于脚本的对象的编辑器。您可以以以下格式返回断点列表（script：line），例如：res://path_to_script.gd:25.
* Texture get_plugin_icon() virtual
	* 在插件中重写此方法以返回纹理，从而为其提供一个图标。
	* 对于主屏幕插件，它显示在屏幕顶部“2D”、“3D”、“脚本”和“AssetLib”按钮的右侧。
	* 理想情况下，插件图标应为白色，背景透明，大小为16x16像素。
``` js
func get_plugin_icon():
    # You can use a custom icon:
    return preload("res://addons/my_plugin/my_plugin_icon.svg")
    # Or use a built-in icon:
    return get_editor_interface().get_base_control().get_icon("Node", "EditorIcons")
```
* String get_plugin_name() virtual
	* 在插件中重写此方法，以在Godot编辑器中显示插件时提供插件的名称。
	* 对于主屏幕插件，它显示在屏幕顶部“2D”、“3D”、“脚本”和“AssetLib”按钮的右侧。
* void set_state(state: Dictionary) virtual
	* 还原get_state（）保存的状态。
* Dictionary get_state() virtual
	* 获取插件编辑器的状态。这用于保存场景（以便再次打开场景时保持状态）和切换选项卡（以便选项卡返回时可以恢复状态）。
* void set_window_layout(layout: ConfigFile) virtual
	* 恢复get_window_layout（）保存的插件GUI布局。
*  void get_window_layout(layout: ConfigFile) virtual
	* 获取插件的GUI布局。这用于在调用queue_save_layout（）或更改编辑器布局（例如更改停靠位置）时保存项目的编辑器布局。
*  bool handles(object: Object) virtual
	*  如果插件编辑特定类型的对象（资源或节点），则实现此功能。如果返回true，则将在编辑器请求时调用函数edit（）和make_visible（）。如果您已经声明了forward_canvas_gui_input（）和forward_spatial_gui_input（）方法，这些方法也将被调用。
*  bool has_main_screen() virtual
	* 如果这是一个主屏幕编辑器插件，则返回true（它与2D、3D、脚本和AssetLib一起进入工作区选择器）。	
*  void make_visible(visible: bool) virtual
	* 当请求编辑器变为可见时，将调用此函数。它用于编辑特定对象类型的插件。请记住，您必须手动管理所有编辑器控件的可见性。
* void save_external_data() virtual
	* 编辑器保存项目后或关闭项目时调用此方法。它要求插件保存已编辑的外部场景/资源。

#### 实方法-编辑器扩展
##### 添加操作
* void add_autoload_singleton(name: String, path: String)
	* 将路径处的脚本作为名称添加到autoload_singleton列表中。
* ToolButton add_control_to_bottom_panel(control: Control, title: String)
	* 将控件添加到底部面板（以及输出、调试、动画等）。返回对添加的按钮的引用。需要时，您可以隐藏/显示按钮。当您的插件被停用时，请确保使用remove_control_from_bottom_panel（）删除自定义控件，并使用Node.queue_free()。
* void add_control_to_container(container:CustomControlContainer, control: Control)
	* 将自定义控件添加到容器（请参见CustomControlContainer）。有许多位置可以在编辑器UI中添加自定义控件。
	* 请记住，您必须自己管理自定义控件的可见性（并可能在添加后将其隐藏）。
	* 当您的插件被停用时，请确保使用remove_control_from_container（）删除自定义控件，并使用Node.queue_free()。
* void add_control_to_dock(slot: DockSlot, control: Control)
	* 将控件添加到特定的dock插槽（有关选项，请参见DockSlot）。
	* 如果dock被重新定位，并且只要插件处于活动状态，编辑器就会在以后的会话中保存dock位置。
	* 当您的插件被停用时，请确保使用remove_control_from_docks（）删除自定义控件，并使用Node.queue_free()。
* void add_custom_type(type: String, base: String, script: Script, icon: Texture)
	* 添加自定义类型，该类型将显示在节点或资源列表中。可以选择传递图标。
	* 选择给定节点或资源时，将实例化基本类型（即"Spatial", "Control", "Resource），然后加载脚本并将其设置为此对象。
	* 你可以使用virtual方法 handles()来检查是否正在编辑自定义对象。或者通过使用is关键字，
	* 在运行时，这将是一个带有脚本的简单对象，因此不需要调用此函数。
*  void add_import_plugin(importer: EditorImportPlugin)
	* 注册一个新的EditorImportPlugin。导入插件用于将自定义和不支持的资产作为自定义资源类型导入。
	* 注意：如果要导入自定义三维资源格式，请改用add_scene_import_plugin()代替使用
	* 有关如何注册插件的示例，请参见add_inspector_plugin（）。
* void add_inspector_plugin(plugin: EditorInspectorPlugin)
	* 注册一个新的 EditorInspectorPlugin。Inspector插件用于扩展EditorInspector，并为对象的属性提供自定义配置工具。
	* 注意：禁用EditorPlugin以防止泄漏和意外行为时，请始终使用remove_inspector_plugin（）删除已注册的 EditorInspectorPlugin。
``` js
const MyInspectorPlugin = preload("res://addons/your_addon/path/to/your/script.gd")
var inspector_plugin = MyInspectorPlugin.new()

func _enter_tree():
    add_inspector_plugin(inspector_plugin)

func _exit_tree():
    remove_inspector_plugin(inspector_plugin)
```
* void add_scene_import_plugin(scene_importer: EditorSceneImporter)
注册一个新的EditorSceneImporter。场景导入器用于将自定义3d asset格式导入为场景。
* void add_spatial_gizmo_plugin(plugin:EditorSpatialGizmoPlugin)
注册一个新的编辑器PatialigzMoplugin。Gizmo插件用于将自定义Gizmo添加到空间对象的三维预览视口中。
有关如何注册插件的示例，请参见add_inspector_plugin（）。
* void add_tool_menu_item(name: String, handler: Object, callback: String, ud: Variant = null)
添加一个自定义菜单项目到到Project>Tools，当用户激活该菜单项时，该菜单项使用一个参数ud调用处理程序实例上的回调。
* void add_tool_submenu_item(name: String, submenu: Object)
在“ Project > Tools > name”下添加自定义子菜单。子菜单应该是类PopupMenu的对象。应使用remove_tool_menu_item(name)清理此子菜单。
--- 
##### 删除操作
* void remove_autoload_singleton(name: String)
Removes an Autoload name from the list.
* void remove_control_from_bottom_panel(control: Control)
Removes the control from the bottom panel. You have to manually Node.queue_free() the control.
* void remove_control_from_container(container: CustomControlContainer, control: Control)
Removes the control from the specified container. You have to manually Node.queue_free() the control.
* void remove_control_from_docks(control: Control)
Removes the control from the dock. You have to manually Node.queue_free() the control.
* void remove_custom_type(type: String)
Removes a custom type added by add_custom_type().
* void remove_export_plugin(plugin: EditorExportPlugin)
Removes an export plugin registered by add_export_plugin().
* void remove_import_plugin(importer: EditorImportPlugin)
Removes an import plugin registered by add_import_plugin().
* void remove_inspector_plugin(plugin: EditorInspectorPlugin)
Removes an inspector plugin registered by add_import_plugin()
* void remove_scene_import_plugin(scene_importer: EditorSceneImporter)
Removes a scene importer registered by add_scene_import_plugin().
* void remove_spatial_gizmo_plugin(plugin: EditorSpatialGizmoPlugin)
Removes a gizmo plugin registered by add_spatial_gizmo_plugin().
* void remove_tool_menu_item(name: String)
Removes a menu name from Project > Tools.
#### 其他
* EditorInterface get_editor_interface()
	* 返回EditorInterface对象，该对象使您可以控制Godot编辑器的窗口及其功能。
* ScriptCreateDialog get_script_create_dialog()
	* 获取用于制作脚本的编辑器对话框。
	* 注意：用户可以在使用前进行配置。
	* 警告：删除和释放此节点将使编辑器的一部分无效，并可能导致崩溃。
* UndoRedo get_undo_redo()
	* 获取撤消/重做对象。编辑器中的大多数操作都是可撤消的，因此请使用此对象确保在值得的情况下执行此操作。
---
* void hide_bottom_panel()
	* 最小化底部面板。
* void make_bottom_panel_item_visible(item: Control)
	* 使底部面板中的特定项目可见。
* void queue_save_layout() const
	* 清除保存项目的编辑器布局
---
* void set_force_draw_over_forwarding_enabled()
	* 启用 forward_canvas_force_draw_over_viewport() for the 2D editor 和 forward_spatial_force_draw_over_viewport() for the 3D editor when their viewports are updated
	* 你只需要调用这个方法一次，它将永久地为这个插件工作。
*  void set_input_event_forwarding_always_enabled()
	*  如果您始终希望从forward_spatial_gui_input() 内的3D视图屏幕接收输入，请使用此方法。如果插件希望在场景中使用光线投射，那么它可能特别有用。
*  int update_overlays() const
	*  更新二维和三维编辑器视口的覆盖。导致以下方法被调用forward_canvas_draw_over_viewport（）、forward_canvas_force_draw_over_viewport（）、forward_spatial_draw_over_viewport（）和forward_spatial_force_draw_over_viewport（）。

###  信号
* main_screen_changed(screen_name: String)
	* Emitted when user changes the workspace (2D, 3D, Script, AssetLib). Also works with custom screens defined by plugins.
*  resource_saved(resource: Resource)
* scene_changed(scene_root: Node)
	* Emitted when the scene is changed in the editor. The argument will return the root node of the scene that has just become active. If this scene is new and empty, the argument will be null.
* scene_closed(filepath: String)
	* Emitted when user closes a scene. The argument is file path to a closed scene.
### 枚举
* enum  CustomControlContainer:
● CONTAINER_TOOLBAR = 0    
● CONTAINER_SPATIAL_EDITOR_MENU = 1    
● CONTAINER_SPATIAL_EDITOR_SIDE_LEFT = 2   
● CONTAINER_SPATIAL_EDITOR_SIDE_RIGHT = 3    
● CONTAINER_SPATIAL_EDITOR_BOTTOM = 4    
● CONTAINER_CANVAS_EDITOR_MENU = 5  
● CONTAINER_CANVAS_EDITOR_SIDE_LEFT = 6   
● CONTAINER_CANVAS_EDITOR_SIDE_RIGHT = 7    
● CONTAINER_CANVAS_EDITOR_BOTTOM = 8    
● CONTAINER_PROPERTY_EDITOR_BOTTOM = 9    
● CONTAINER_PROJECT_SETTING_TAB_LEFT = 10    
● CONTAINER_PROJECT_SETTING_TAB_RIGHT = 11    
* enum  DockSlot:
● DOCK_SLOT_LEFT_UL = 0   
● DOCK_SLOT_LEFT_BL = 1    
● DOCK_SLOT_LEFT_UR = 2   
● DOCK_SLOT_LEFT_BR = 3    
● DOCK_SLOT_RIGHT_UL = 4  
● DOCK_SLOT_RIGHT_BL = 5   
● DOCK_SLOT_RIGHT_UR = 6   
● DOCK_SLOT_RIGHT_BR = 7   
● DOCK_SLOT_MAX = 8  –  Represents the size of the DockSlot enum.
