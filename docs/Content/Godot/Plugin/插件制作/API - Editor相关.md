# EditorExportPlugin
## 介绍
当用户导出项目文件时, 该插件自动激活, 主要决定应该导出文件以及类型.
### 函数

``` js
void _export_begin ( PoolStringArray features, bool is_debug, String path, int flags ) virtual
```
虚方法 文件导出开始时执行
 `features` is the list of features for the export, `is_debug` is `true` for debug builds, `path` is the target path for the exported project. `flags` is only used when running a runnable profile, e.g. when using native run on Android.

``` js
void _export_end ( ) virtual
```
虚方法 文件导出完毕时执行

``` js
void _export_file ( String path, String type, PoolStringArray features ) virtual
```
虚方法 导出文件时执行
Called for each exported file, providing arguments that can be used to identify the file. `path` is the path of the file, `type` is the [Resource](https://docs.godotengine.org/en/stable/classes/class_resource.html#class-resource) represented by the file (e.g. [PackedScene](https://docs.godotengine.org/en/stable/classes/class_packedscene.html#class-packedscene)) and `features` is the list of features for the export.



void add_file ( String path, PoolByteArray file, bool remap )

void

add_ios_bundle_file ( String path )

void

add_ios_cpp_code ( String code )

void

add_ios_embedded_framework ( String path )

void

add_ios_framework ( String path )

void

add_ios_linker_flags ( String flags )

void

add_ios_plist_content ( String plist_content )

void

add_ios_project_static_lib ( String path )

void

add_shared_object ( String path, PoolStringArray tags )

void

skip ( )```