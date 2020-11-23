---
id: doc9
title: Iluminación
sidebar_label: Taller de Iluminación
---

## Fog: Niebla mediante fragment shaders
Se agrega niebla a una escena de un cilindro que posee una textura. En el primer ejemplo se modifica en cada frame el valor de alpha,
que es el parámetro que se usa para interpolar en la función mix entre el valor de la textura y el color de la niebla. En el segundo
ejemplo el alpha se multiplica por la distancia entre la coordenada del vertex y la posición de la cámara.

Processing
```
PImage label;
PShape can;
float angle;
float alpha;
int alpha_zero_cnt;
boolean increase_alpha;
color fog_color;
PVector fog_colorv;

PShader fogShader;

void setup() {
  size(640, 360, P3D);
  fog_colorv = new PVector(167,48,246);
  PVector norm_fog_color = new PVector(fog_colorv.x/255,fog_colorv.y/255,fog_colorv.z/255);
  alpha = 1.0;
  alpha_zero_cnt = 0;
  increase_alpha = false;
  label = loadImage("leaf.jpg");
  can = createCan(100, 200, 32, label);
  fogShader = loadShader("fogfrag.glsl", "fogvert.glsl");
  fogShader.set("fog_color", norm_fog_color);
}

void draw() {    
  background(fog_colorv.x, fog_colorv.y, fog_colorv.z);
  fogShader.set("u_fogAmount", alpha);
  shader(fogShader);

  pointLight(255, 255, 255, width/2, height, 200);  

  translate(width/2, height/2);
  rotateY(angle);  
  shape(can);  
  angle += 0.01;
  if(alpha==1){
     increase_alpha = false;
  }
  if(alpha<=0){
     alpha_zero_cnt++;
  }

  if(alpha_zero_cnt == 15){
     alpha_zero_cnt = 0;
     increase_alpha = true;
  }

  if(increase_alpha == true){
     alpha = alpha + 0.005;
  }

  if(increase_alpha == false){
     alpha = alpha - 0.005;
  }


}

void onClicked(){

}

PShape createCan(float r, float h, int detail, PImage tex) {
  textureMode(NORMAL);
  PShape sh = createShape();
  sh.beginShape(QUAD_STRIP);
  sh.noStroke();
  sh.texture(tex);
  for (int i = 0; i <= detail; i++) {
    float angle = TWO_PI / detail;
    float x = sin(i * angle);
    float z = cos(i * angle);
    float u = float(i) / detail;
    sh.normal(x, 0, z);
    sh.vertex(x * r, -h/2, z * r, u, 0);
    sh.vertex(x * r, +h/2, z * r, u, 1);    
  }
  sh.endShape();
  return sh;
}
```

Fragment Shader
```
#ifdef GL_ES
precision mediump float;
precision mediump int;
#endif

uniform sampler2D texture;

varying vec4 vertColor;
varying vec4 vertTexCoord;
uniform float u_fogAmount;
uniform vec3 fog_color;

void main() {
  //gl_FragColor = texture2D(texture, vertTexCoord.st) * vertColor;
  gl_FragColor = mix(texture2D(texture, vertTexCoord.st) * vertColor, vec4(fog_color,1.0), u_fogAmount);
}

```

![alt-text](assets/cil.gif)


Link al [código fuente](https://github.com/VisualUN/Processing/tree/master/fog_cilinder)


Fragment Shader
```
#ifdef GL_ES
precision mediump float;
precision mediump int;
#endif

uniform sampler2D texture;

varying vec4 vertColor;
varying vec4 vertTexCoord;
uniform float u_fogAmount;
uniform vec3 fog_color;

void main() {
  //gl_FragColor = texture2D(texture, vertTexCoord.st) * vertColor;
  float d = distance(CameraEye, vertTexCoord);
  gl_FragColor = mix(texture2D(texture, vertTexCoord.st) * vertColor, vec4(fog_color,1.0), u_fogAmount*d);
}

```

![alt-text](assets/w.gif)

El color de la niebla es un parámetro que se le envía al fragment shader y puede ser modificado.

![alt-text](assets/green_fog.gif)

Link al [código fuente](https://github.com/VisualUN/Processing/tree/master/fog_w)


### Referencias
- https://webglfundamentals.org/webgl/lessons/webgl-fog.html
- https://vicrucann.github.io/tutorials/osg-shader-fog/
