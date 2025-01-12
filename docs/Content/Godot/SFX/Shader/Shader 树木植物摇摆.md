``` js
shader_type canvas_item; 

uniform float strength = 5.; 
uniform float speed = 2.0; 

void vertex() {
	float offset = sin(TIME * speed) * strength; 
	VERTEX.x += offset * step(1., 1. - VERTEX.y); 
} 
```