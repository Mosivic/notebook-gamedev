#### 预先设置 ####
1.项目设置里 Display/Size 设置 Width, Height（游戏像素）
2.设置 Test Width, Test Heigh （测试时窗口大小）
3.设置 Stretch/mode 为2d （拉伸模式）

#### 人物创建 ####
##### 创建结点 #####
创建 KinematicBody2D ，挂载Sprite和CollisionShape2D作为贴图与碰撞检测


##### 移动脚本 ######
``` python

var velocity = Vector2.ZERO 
const MAX_SPEED = 10 
# 加速 
const ACCELERATION = 80 
# 摩擦力 
const FRICTION = 80 

func _physics_process(delta): 
    var input_vector = Vector2.ZERO
    input_vector.x = Input.get_action_strength("ui_right") - Input.get_action_strength("ui_left")
    input_vector.y = Input.get_action_strength("ui_down") - Input.get_action_strength("ui_up") 
    # 归一化，解决对角线加速问题
    input_vector = input_vector.normalized()
    
    if input_vector != Vector2.ZERO: 
        velocity = velocity.move_toward(input_vector * MAX_SPEED, ACCELERATION * delta)
    else: 
    # 摩擦力缓慢停止 
        velocity = velocity.move_toward(Vector2.ZERO, FRICTION * delta) 
        move_and_collide(velocity)
```

##### 添加碰撞 ######
##### 添加动画 ######
**方法一** : AnimationPlayer 动画创建 + AnimationTree 动画管理过渡
```python

extends KinematicBody2D 
onready var animationPlayer = $AnimationPlayer 
onready var animationTree = $AnimationTree 
onready var animationState = animationTree.get("parameters/playback") 

if input_vector != Vector2.ZERO: 
    # 设置动画树的状态 
     animationTree.set("parameters/Idle/blend_position", input_vector)
     animationTree.set("parameters/Run/blend_position", input_vector) 
     animationState.travel("Run") 
    velocity = velocity.move_toward(input_vector * MAX_SPEED, ACCELERATION * delta) 
else: 
    animationState.travel("Idle")
    # 摩擦力缓慢停止 
    velocity = velocity.move_toward(Vector2.ZERO, FRICTION * delta)
```
**方法二** ：AnimationSprite 简单动画