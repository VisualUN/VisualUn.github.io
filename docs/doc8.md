---
id: doc8
title: Ilusiones visuales
sidebar_label: Taller de Ilusiones visuales
---

## Ilusión 1: Percepción de color

Link al [código fuente](https://github.com/VisualUN/Processing/tree/master/ilusionColor)
```
int Y_AXIS = 1;
int X_AXIS = 2;
// Define colors
color greyback = color(51,51,51);
color whiteback = color(255);
color grey = color(145,145,145);
boolean reveal = false;

void setup() {
  size(640, 360);
  // Background
  setGradient(0, 0, width, height, greyback, whiteback, X_AXIS);
  // Foreground
  setGradient(50, 190, 540, 80, grey, grey, X_AXIS);
  //noLoop();
}

void setGradient(int x, int y, float w, float h, color c1, color c2, int axis ) {

  noFill();

  if (axis == X_AXIS) {  // Left to right gradient
    for (int i = x; i <= x+w; i++) {
      float inter = map(i, x, x+w, 0, 1);
      color c = lerpColor(c1, c2, inter);
      stroke(c);
      line(i, y, i, y+h);
    }
  }
}

void mouseClicked() {
  if(reveal == false){
    //background
    setGradient(0, 0, width, height, whiteback, whiteback, X_AXIS);
    // Foreground
    setGradient(50, 190, 540, 80, grey, grey, X_AXIS);
    reveal = true;

  }
  else{
    //background
    setGradient(0, 0, width, height, greyback, whiteback, X_AXIS);
    // Foreground
    setGradient(50, 190, 540, 80, grey, grey, X_AXIS);
    reveal = false;
  }

}

```
## Ilusión
El usuario percibe que la barra tiene distintos tonos de gris cuando no es así. Esta ilusión se produce por el gradiente de color
que hay en el fondo. Este fenómeno tiene relación con el efecto Bezold, el cual sugiere que un color puede ser apreciado de manera diferente dependiendo de su relación con los colores adyacentes.

## Interactividad
El usuario puede hacer click, de manera que se elimina el gradiente del fondo quedando este completamente blanco. Esto
le permite al usuario apreciar que la franja gris no tiene variaciones, sino que es completamente del mismo color.

### Resultado:
![alt-text](assets/ilusionGris1.png)

### Interactividad al hacer click
![alt-text](assets/ilusionGris2.png)


### Referencias
- https://processing.org/examples/lineargradient.html
- https://twistedsifter.com/2017/12/horizontal-bar-is-single-shade-of-gray-bezold-effect/


## Ilusión 2: Triángulo de Penrose

Link al [código fuente](https://github.com/VisualUN/Processing/blob/master/penrose.js)
```
var anglex = 42
var angley = -36.65
var anglez = 0
function setup() {
  createCanvas(500, 400, WEBGL);
  angleMode(DEGREES);
  currCamera = createCamera();
  currCamera.ortho();
}

function mouseClicked() {

}

function draw() {

  orbitControl();
  background(215);
  normalMaterial();
  ambientLight("#FF7F15");
  specularMaterial(220);

  let locX = height / 2;
  let locY = width / 2;
  pointLight(220, 220, 220, locX, locY, 150);
  //emissiveMaterial("#FF7F15");
  normalMaterial();
  const numblocks = 7;

  //rotate(angle);
  rotateY(angley);
  rotateX(anglex);
  rotateZ(anglez);
  //angle++;
  //console.log(angle)
  rectMode(CENTER);

  for (let z = 0; z <= numblocks; z++) {
       translate(20, 0, 0);
       push();
       box(15, 15, 15);
       pop();

  }

  for (let y = 0; y < numblocks; y++) {
       translate(0, 20, 0);
       push();
       box(15, 15, 15);
       pop();

  }

  for (let x = 0; x < numblocks; x++) {
       translate(0, 0, 20);
       push();
       box(15, 15, 15);
       pop();

  }

}
```
## Ilusión
El usuario percibe que se forma un triángulo, sin embargo esta figura es un objeto que no puede ser construido en un espacio tridimensional. La ilusión consiste en
observar una disposición de una serie de cubos que desde el ángulo apropiado parecen formar un Triángulo de Penrose.

## Interactividad
El usuario puede girar la figura en 3D usando el mouse para darse cuenta que el Triángulo se forma gracias a un efecto de perspectiva
cuando el ángulo es el apropiado.

### Resultado:
![alt-text](assets/triangulo.gif)

### Referencias
- https://p5js.org/es/reference/
- https://www.yeggi.com/q/penrose/3/

## Ilusión 3: Cuadrícula de Hermann

Link al [código fuente](https://github.com/VisualUN/Processing/tree/master/gridIlusion)
### Código Processing
```
PShader grid;

void setup() {
  size(600, 600, P3D);
  grid = loadShader("grid.glsl");
  //grid.set("resolution", width, height);
}

void draw() {
   filter(grid);
   ellipseMode(RADIUS);  // Set ellipseMode to RADIUS
   fill(255);  // Set fill to white
   ellipse(width/2, height/2, 15 , 15);  // Draw white ellipse using RADIUS mode

}
```
### Fragment Shader
```
precision mediump float;

uniform float vpw = 600; // Width, in pixels
uniform float vph = 600; // Height, in pixels

uniform vec2 offset =  vec2(-1,1);
uniform vec2 pitch = vec2(100,100);

void main() {
  float lX = gl_FragCoord.x / vpw;
  float lY = gl_FragCoord.y / vph;

  float scaleFactor = 60000000.0;

  float offX = (scaleFactor * offset[0]) + gl_FragCoord.x;
  float offY = (scaleFactor * offset[1]) + (1.0 - gl_FragCoord.y);

  if (int(mod(offX, pitch[0])) < 12 || int(mod(offY, pitch[1])) < 12) {
    gl_FragColor = vec4(1.0, 1.0, 1.0, 1.5);
  } else {
    gl_FragColor = vec4(0, 0, 0, 1.0);
  }
}

```

## Ilusión
Cuando el usuario mira la cuadrícula blanca sobre el fondo negro, se tiene la impresión de que surgen manchas o circulos grises en las intersecciones de las líneas. Sin embargo, estos circulos desaparecen cuando se observa directamente la intersección. Esta ilusión fue observada por primera vez por Ludimar Hermann en 1870. Esta ilusión se desarrolló utilizando un fragment shader.

### Resultado:
![alt-text](assets/grid.png)

### Referencias
- https://es.wikipedia.org/wiki/Ilusi%C3%B3n_de_la_cuadr%C3%ADcula
- https://stackoverflow.com/questions/24772598/drawing-a-grid-in-a-webgl-fragment-shader




### Discusión
## 1 Complete la tabla
| Ilusión     | Categoria   | Referencia     | Tipo de interactividad (si aplica)   | URL código base |
| ----------- | ----------- | -----------    | ------------------------------------ | ----------------------------                   |
| Gradiente gris | Percepción de color | https://twistedsifter.com/2017/12/horizontal-bar-is-single-shade-of-gray-bezold-effect| Click del mouse | https://processing.org/examples/lineargradient.html
| Triángulo de Penrose | Perspectiva geométrica| https://es.wikipedia.org/wiki/Tri%C3%A1ngulo_de_Penrose | Girar la figura en 3D | n/a
| Cuadrícula de Hermann   | Simultaneous Lightness Contrast (SLC) | https://es.wikipedia.org/wiki/Ilusi%C3%B3n_de_la_cuadr%C3%ADcula | n/a | https://stackoverflow.com/questions/24772598/drawing-a-grid-in-a-webgl-fragment-shader |
| Paragraph   | Text        | text | text | text |



## 2 Describa brevememente las referencias estudiadas y los posibles temas en los que le gustaría profundizar
