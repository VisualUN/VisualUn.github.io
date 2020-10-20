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

void draw() {

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




### Discusión
## 1 Complete la tabla
| Ilusión     | Categoria   | Referencia     | Tipo de interactividad (si aplica)   | URL código base |
| ----------- | ----------- | -----------    | ------------------------------------ | ----------------------------                   |
| 1           | Percepción de color | https://twistedsifter.com/2017/12/horizontal-bar-is-single-shade-of-gray-bezold-effect| Click del mouse | https://processing.org/examples/lineargradient.html
| Paragraph   | Text        |

## 2 Describa brevememente las referencias estudiadas y los posibles temas en los que le gustaría profundizar
