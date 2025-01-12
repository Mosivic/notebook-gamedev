> From: https://www.bilibili.com/video/BV1JZ4y1H7BN

### 文字简述 ###
1.创建背景Background，需要遮罩处理的图形GodotLogo，以及Light2D
2.设置Light2D的遮罩处理图层为Layer1(默认)，设置背景的图层不为Layer1，Logo的Layer为Layer1(与Light2D相同)
->即完成了只对Logo进行光照处理
3..设置Logo图形的材质为Canvas，将其光照模式设为Light Only->即Logo图形为被光照处不显现
![[00..png]]

![[01.png]]

![[02.png]]

### 灯光阴影 ###
用Light2D做灯光阴影
1.打开Light2D的Shadow
2.给受灯光物体加上LightOccluder2D结点，并绘制形状

![[03.png]]