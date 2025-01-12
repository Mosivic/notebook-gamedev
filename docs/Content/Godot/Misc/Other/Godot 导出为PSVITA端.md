![[Pasted image 20220717191336.png]]
导出设置
1. parental level 值为0-11之间
2. 版本格式必须为 XX.YY
3. Assets设置图片位深必须为 8 或者更低 (使用pngquant处理图片)
4. 图形API目前只支持GLES2 , 切在设置中需要勾选 "Fallback to GLES2"

目前问题:
1. 需要改变与之前不同的title id才能成功安装
2. 导出vpk时选择路径与重命名无效, 默认为工程根目录
3. 安装后运行闪退