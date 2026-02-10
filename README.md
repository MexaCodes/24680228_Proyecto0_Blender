# Descripción general
El siguiente script en Python para Blender crea un polígono regular en 2D (por ejemplo, un triángulo, cuadrado, hexágono, etc.) a partir del número de lados y un radio.
Además, limpia la escena antes de crear el objeto.
## 1. Importación de librerías
bpy:
Es la API de Python de Blender. Permite crear y manipular objetos, mallas y escenas.

math:
Se utiliza para operaciones matemáticas, en este caso:

cos() → coseno

sin() → seno

pi → valor de π

Estas funciones son necesarias para calcular la posición de los vértices del polígono.

```python
import bpy
import math
```
## 2. Definición de la función
```python
def crear_poligono_2d(nombre, lados, radio):
```
Parámetros

nombre: Nombre del objeto en Blender.

lados: Número de lados del polígono.

radio: Distancia del centro a cada vértice (tamaño del polígono).

Esta función genera un polígono regular centrado en el origen (0,0,0).

## 3. Crear la malla y el objeto
```python
malla = bpy.data.meshes.new(nombre)
objeto = bpy.data.objects.new(nombre, malla)
```
Explicación:

bpy.data.meshes.new(nombre)

Crea una nueva estructura de malla vacía.

bpy.data.objects.new(nombre, malla)

Crea un objeto en Blender que utilizará esa malla.

En Blender:

Mesh = geometría (vértices, aristas, caras)

Object = entidad visible en la escena

## 4. Vincular el objeto a la escena
```python
bpy.context.collection.objects.link(objeto)
```
Esto agrega el objeto a la colección activa para que sea visible en la escena.
Si no se hace este paso, el objeto existe en memoria pero no aparece en Blender.
## 5. Crear listas para la geometría
```python
vertices = []
aristas = []
```
vertices: almacenará las coordenadas (x, y, z)

aristas: almacenará las conexiones entre vértices

## 6. Calcular los vértices (Geometría del polígono)
```python
for i in range(lados):
    angulo = 2 * math.pi * i / lados
    x = radio * math.cos(angulo)
    y = radio * math.sin(angulo)
    vertices.append((x, y, 0))
```
Explicación matemática

Para crear un polígono regular:

Fórmulas:

x = r · cos(θ)

y = r · sin(θ)

Donde:

r = radio

θ = ángulo del vértice

El ángulo se distribuye uniformemente:
```python
angulo = (2π / lados) * i
```
Esto coloca los vértices alrededor de un círculo.t5
El valor z = 0 indica que el polígono es 2D (plano XY).
## 7. Definir las aristas
```python
for i in range(lados):
    aristas.append((i, (i + 1) % lados))
```
Qué hace:

Conecta cada vértice con el siguiente.

(i + 1) % lados permite que el último vértice se conecte con el primero, cerrando la figura.

Ejemplo (hexágono):
```
0-1
1-2
2-3
3-4
4-5
5-0
```
## 8. Crear la geometría en la malla
```python
malla.from_pydata(vertices, aristas, [])
malla.update()
```
from_pydata() carga:

Lista de vértices

Lista de aristas

Lista de caras (vacía en este caso)

update() actualiza la malla para que Blender la muestre correctamente.

## 9. Limpiar la escena
```python
bpy.ops.object.select_all(action="SELECT")
bpy.ops.object.delete()
```
Esto:

Selecciona todos los objetos

Los elimina

Sirve para empezar con una escena vacía.

## 10. Crear el polígono
```python
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```
Resultado:

Nombre: Poligono2D

Tipo: Polígono regular

Lados: 6 (hexágono)

Radio: 5 unidades

## 11. Resultado final
El script:

1. Limpia la escena

2. Calcula vértices usando trigonometría

3. Conecta los vértices

4. Crea un polígono 2D centrado en el origen
