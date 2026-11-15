# Chuleta de NumPy

## Índice
1. [Importación y Creación de Arrays](#1-importación-y-creación-de-arrays)
2. [Inspección de Arrays](#2-inspección-de-arrays)
3. [Subsetting, Slicing e Indexación](#3-subsetting-slicing-e-indexación)
4. [Operaciones Matemáticas y Álgebra](#4-operaciones-matemáticas-y-álgebra)
5. [Funciones de Agregación](#5-funciones-de-agregación)
6. [Manipulación y Forma de Arrays](#6-manipulación-y-forma-de-arrays)
7. [Ordenación de Arrays](#7-ordenación-de-arrays)
8. [Copiar Arrays](#8-copiar-arrays)
9. [Guardar y Cargar Datos en Disco](#9-guardar-y-cargar-datos-en-disco)

---

## 1. Importación y Creación de Arrays

### Importación estándar
```python
import numpy as np
```

### Creación de Arrays
```python
a = np.array([1, 2, 3])
b = np.array([(1.5, 2, 3), (4, 5, 6)], dtype=float)
c = np.array([[(1.5, 2, 3), (4, 5, 6)], [(3, 2, 1), (4, 5, 6)]], dtype=float)
```

### Placeholders Iniciales
```python
np.zeros((3, 4))                          # Array de ceros
np.ones((2, 3, 4), dtype=np.int16)        # Array de unos
np.arange(10, 25, 5)                      # Valores espaciados por paso
np.linspace(0, 2, 9)                      # Valores espaciados por número de muestras
np.full((2, 2), 7)                        # Array constante
np.eye(2)                                 # Matriz identidad 2x2
np.random.random((2, 2))                  # Array con valores aleatorios
np.empty((3, 2))                          # Array vacío
```

---

## 2. Inspección de Arrays

```python
a.shape         # Dimensiones del array
len(a)          # Longitud del array
b.ndim          # Número de dimensiones
b.size          # Número de elementos totales
b.dtype         # Tipo de dato de los elementos
b.dtype.name    # Nombre del tipo de dato
b.astype(int)   # Convertir un array a otro tipo
```

---

## 3. Subsetting, Slicing e Indexación

```python
a[2]              # Elemento en el índice 2
b[1, 2]           # Fila 1, columna 2 (equivalente a b[1][2])
a[0:2]            # Elementos de índice 0 y 1
b[0:2, 1]         # Filas 0 y 1 de la columna 1
b[:1]             # Todos los elementos de la primera fila
c[1, ...]         # Equivalente a c[1, :, :]
a[::-1]           # Array invertido
```

### Indexación Booleana
```python
a[a < 2]          # Elementos de a menores que 2
```

### Fancy Indexing
```python
b[[1, 0, 1, 0], [0, 1, 2, 0]]       # Selecciona coordenadas específicas (1,0), (0,1), etc.
b[[1, 2, 1, 0]][:, [0, 1, 2, 0]]    # Subconjunto de filas y columnas
```

---

## 4. Operaciones Matemáticas y Álgebra

### Aritmética
```python
a - b             # Resta (también np.subtract(a, b))
b + a             # Suma (también np.add(b, a))
a / b             # División (también np.divide(a, b))
a * b             # Multiplicación elemento a elemento (también np.multiply(a, b))
np.exp(b)         # Exponenciación
np.sqrt(b)        # Raíz cuadrada
np.sin(a)         # Seno elemento a elemento
np.cos(b)         # Coseno elemento a elemento
np.log(a)         # Logaritmo natural elemento a elemento
e.dot(f)          # Producto escalar (dot product)
```

### Comparaciones
```python
a == b            # Comparación elemento a elemento
a < 2             # Comparación elemento a elemento
np.array_equal(a, b) # Comparación de arrays completos
```

---

## 5. Funciones de Agregación

```python
a.sum()           # Suma total
a.min()           # Valor mínimo
b.max(axis=0)     # Valor máximo por columna (eje 0)
b.cumsum(axis=1)  # Suma acumulativa por filas (eje 1)
a.mean()          # Media
np.median(b)      # Mediana
np.corrcoef(a)    # Coeficiente de correlación
np.std(b)         # Desviación estándar
```

---

## 6. Manipulación y Forma de Arrays

### Transposición y Redimensión
```python
np.transpose(b)   # Transponer array (también b.T)
b.ravel()         # Aplanar el array (flatten)
g.reshape(3, 2)   # Cambiar forma sin alterar datos
h.resize((2, 6))  # Devuelve un nuevo array con la forma indicada
```

### Añadir, Insertar y Eliminar
```python
np.append(h, g)   # Añadir elementos
np.insert(a, 1, 5) # Insertar elementos en la posición 1
np.delete(a, [1])  # Eliminar elementos
```

### Combinar Arrays
```python
np.concatenate((a, d), axis=0)  # Concatenar arrays
np.vstack((a, b))               # Apilar verticalmente (row-wise)
np.hstack((e, f))               # Apilar horizontalmente (column-wise)
np.column_stack((a, d))         # Apilar como columnas (también np.c_[a, d])
```

### Dividir Arrays
```python
np.hsplit(a, 3)   # Dividir horizontalmente en el índice 3
np.vsplit(c, 2)   # Dividir verticalmente en el índice 2
```

---

## 7. Ordenación de Arrays

```python
a.sort()          # Ordenar un array
c.sort(axis=0)    # Ordenar los elementos a lo largo del eje 0
```

---

## 8. Copiar Arrays

```python
h.view()          # Crear una vista del array con los mismos datos
np.copy(a)        # Crear una copia superficial
a.copy()          # Crear una copia profunda (deep copy)
```

---

## 9. Guardar y Cargar Datos en Disco

### Archivos Binarios (.npy / .npz)
```python
np.save("my_array", a)
np.savez("array.npz", a=a, b=b)
np.load("my_array.npy")
```

### Archivos de Texto
```python
np.loadtxt("myfile.txt")
np.genfromtxt("my_file.csv", delimiter=",")
np.savetxt("myarray.txt", a, delimiter="")
```
