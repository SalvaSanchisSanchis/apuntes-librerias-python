# 🚀 Guías de Referencia y Apuntes Técnicos

Bienvenido a mi repositorio de recursos técnicos. Aquí encontrarás guías rápidas, sintaxis clave y ejemplos prácticos orientados al desarrollo y análisis de datos.

## 📑 Índice General

*   **Pandas (Python)**
    *   [Carga y Exploración de Datos](./pandas/README.md#1-carga-y-exploración-de-datos)
    *   [Selección con `loc`, `iloc` y `at`](./pandas/README.md#2-selección-y-filtrado)
    *   [Máscaras Booleanas y Filtros Avanzados](./pandas/README.md#3-máscaras-booleanas-y-filtros)
    *   [Gestión de Valores Nulos y Duplicados](./pandas/README.md#4-nulos-y-duplicados)
    *   [Agrupaciones y Agregaciones (`groupby`)](./pandas/README.md#5-agrupaciones-y-agregaciones)
    *   [Uniones de DataFrames (`merge` y `concat`)](./pandas/README.md#6-uniones-y-concatenación)



# 📚 Chuletas Técnicas de Python: Matplotlib, Pandas y NumPy

---

# 📊 1. Chuleta: Visualización de Datos con Matplotlib y Pandas (`tecno.csv`)

Esta guía rápida cubre la carga de datos estructurados y la creación de gráficos de barras avanzados (verticales, horizontales, agrupados y apilados) utilizando `pandas`, `numpy` y `matplotlib.pyplot`.

## 1.1 Carga del conjunto de datos (`tecno.csv`)
Para leer ficheros separados por tabuladores (`\t`), se utiliza el parámetro `sep="\t"` en la función `read_csv` de Pandas.

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Cargar el archivo delimitado por tabuladores
df_tecno = pd.read_csv("./Recursos/tecno.csv", sep="\t")

# Vista previa opcional
print(df_tecno.head())
```
## 1.2 Gráfico de Barras Verticales (plt.bar)

Permite representar una variable numérica continua en función de una variable categórica.
range(len(...)): Crea una secuencia numérica para la posición de cada barra en el eje X.
rotation=90: Rota las etiquetas del eje X para evitar solapamientos cuando los nombres de las categorías son largos.
grid(axis='y'): Añade líneas de guía horizontales para facilitar la lectura de los valores.

```Python
fig, ax = plt.subplots(figsize=(10, 5))
x = range(len(df_tecno))

ax.bar(x, df_tecno["Employees"], color="tab:cyan", alpha=0.8)

ax.set_xticks(x)
ax.set_xticklabels(df_tecno["Company"], rotation=90)
ax.set_title("Número de empleados por empresa tecnológica")
ax.set_xlabel("Empresa")
ax.set_ylabel("Empleados")
ax.grid(axis="y", linestyle="--", alpha=0.7)

plt.tight_layout()
plt.show()
```
## 1.3 Gráfico de Barras Horizontales Ordenadas (plt.barh)
Ideal para mostrar rankings o cuando los nombres de las categorías son extensos y verticales dificultan la lectura.
sort_values(by=..., ascending=True): Ordena el DataFrame previamente de menor a mayor para que el gráfico quede visualmente ordenado de arriba a abajo.
barh(): Dibuja barras horizontales.
set_yticks() / set_yticklabels(): Configura las etiquetas en el eje vertical Y.


```Python
# Ordenar de menor a mayor ingresos
df_tecno_ordenado = df_tecno.sort_values(by="Revenue", ascending=True)

fig, ax = plt.subplots(figsize=(10, 5))
y_pos = range(len(df_tecno_ordenado))

ax.barh(y_pos, df_tecno_ordenado["Revenue"], color="tab:orange", alpha=0.8)

ax.set_yticks(y_pos)
ax.set_yticklabels(df_tecno_ordenado["Company"])
ax.set_title("Ingresos de empresas tecnológicas (ordenados de menor a mayor)")
ax.set_xlabel("Ingresos")
ax.set_ylabel("Empresa")
ax.grid(axis="x", linestyle="--", alpha=0.7)

plt.tight_layout()
plt.show()
```
## 1.4 Gráfico de Barras Agrupadas (Múltiples series con desplazamiento)
Permite comparar dos métricas diferentes para un subconjunto de datos (por ejemplo, las top 6 empresas con más empleados).
nlargest(6, ...): Selecciona de forma automática los 6 registros con valores más altos en una columna.
Desplazamiento horizontal (x_top - ancho/2 y x_top + ancho/2): Separa visualmente las dos series de barras para que no se superpongan en la misma coordenada X.


```Python
# Filtrar top 6 y escalar unidades para mejorar la visualización conjunta
df_top6 = df_tecno.nlargest(6, "Employees").copy()
df_top6["Emp_miles"] = df_top6["Employees"] / 1000
df_top6["Rev_miles"] = df_top6["Revenue"] / 1000

x_top = np.arange(len(df_top6))
ancho = 0.35

fig, ax = plt.subplots(figsize=(10, 5))

# Serie 1 desplazada a la izquierda
ax.bar(
    x_top - ancho / 2,
    df_top6["Emp_miles"],
    width=ancho,
    label="Empleados (miles)",
    color="tab:blue",
)
# Serie 2 desplazada a la derecha
ax.bar(
    x_top + ancho / 2,
    df_top6["Rev_miles"],
    width=ancho,
    label="Ingresos (miles de millones)",
    color="tab:green",
)

ax.set_xticks(x_top)
ax.set_xticklabels(df_top6["Company"], rotation=45)
ax.set_title("Top 6 empresas: Empleados vs Ingresos (Escalados)")
ax.set_xlabel("Empresa")
ax.set_ylabel("Magnitud escalada")
ax.legend()
ax.grid(axis="y", linestyle="--", alpha=0.7)

plt.tight_layout()
plt.show()
```

## 1.5 Gráfico de Barras Apiladas (bottom=)
Muestra la composición o la suma acumulada de métricas sobre la misma categoría.
Parámetro bottom=: Indica la altura base desde la cual empieza a dibujarse la segunda serie. Se le pasa como valor la serie inferior (df_top6["Emp_miles"]) para que se apile justo encima.


```Python
fig, ax = plt.subplots(figsize=(10, 5))

# Barra base inferior
ax.bar(
    x_top,
    df_top6["Emp_miles"],
    width=ancho,
    label="Empleados (miles)",
    color="tab:blue",
)

# Barra superior apilada utilizando bottom=
ax.bar(
    x_top,
    df_top6["Rev_miles"],
    width=ancho,
    bottom=df_top6["Emp_miles"],
    label="Ingresos (miles de millones)",
    color="tab:green",
)

ax.set_xticks(x_top)
ax.set_xticklabels(df_top6["Company"], rotation=45)
ax.set_title("Top 6 empresas: Gráfico de barras apiladas")
ax.set_xlabel("Empresa")
ax.set_ylabel("Suma de magnitudes escaladas")
ax.legend()
ax.grid(axis="y", linestyle="--", alpha=0.7)

plt.tight_layout()
plt.show()
```
# 📊 2. Chuleta: Guía de Referencia y Apuntes Técnicos de NumPy (Python)
Bienvenido a la sección de NumPy. Aquí encontrarás guías rápidas, sintaxis clave y ejemplos prácticos orientados a la creación y manipulación eficiente de arrays y computación numérica.

## 2.1 Índice General NumPy
Creación y Propiedades de Arrays
Indexación y Slicing
Operaciones Matemáticas y Estadísticas
Máscaras Booleanas y Filtrado
Modificación de Formas

## 2.2 Creación y Propiedades de Arrays
Python
import numpy as np

# Crear un array a partir de una lista de Python
a = np.array([1, 2, 3, 4, 5])
print(a)

# Arrays especiales
ceros = np.zeros((3, 3))  # Matriz de 3x3 llena de ceros
unos = np.ones((2, 4))  # Matriz de 2x4 llena de unos
rango = np.arange(10, 20, 2)  # Secuencia con inicio, fin y salto
espacio = np.linspace(0, 1, 5)  # 5 valores equiespaciados entre 0 y 1
identidad = np.eye(3)  # Matriz identidad de 3x3

# Propiedades principales
print("Dimensiones (ndim):", a.ndim)
print("Forma (shape):", a.shape)
print("Tamaño total (size):", a.size)
print("Tipo de dato (dtype):", a.dtype)
2.3 Indexación y Slicing
Acceso a elementos individuales y porciones de un array multidimensional.

```Python
matriz = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Elemento en fila 0, columna 2
print(matriz[0, 2])

# Slicing: todas las filas, columna 1 en adelante
print(matriz[:, 1:])

# Slicing de filas específicas
print(matriz[0:2, 0:2])
```
## 2.4 Operaciones Matemáticas y Estadísticas
NumPy permite operaciones vectorizadas muy eficientes sin necesidad de bucles explícitos for.

```Python
arr = np.array([1, 2, 3, 4])

# Operaciones elemento a elemento
print(arr + 10)  # Suma escalar
print(arr * 2)  # Multiplicación escalar
print(np.sqrt(arr))  # Raíz cuadrada

# Estadísticas globales o por ejes
print("Suma total:", np.sum(arr))
print("Media:", np.mean(arr))
print("Valor máximo:", np.max(arr))
print("Índice del valor máximo:", np.argmax(arr))
```

## 2.5 Máscaras Booleanas y Filtrado
Permiten filtrar elementos que cumplan una condición lógica de forma nativa.

```Python
datos = np.array([15, 8, 22, 4, 30, 12])

# Crear máscara booleana (elementos mayores a 10)
mascara = datos > 10
print(mascara)  # [ True False  True False  True  True]

# Aplicar máscara para extraer los valores filtrados
filtrados = datos[mascara]
print(filtrados)  # [15 22 30 12]

# En una sola línea:
print(datos[datos > 10])
```

## 2.6 Modificación de Formas (reshape y flatten)
Cambiar la estructura de las dimensiones de un array.

```Python
# Array plano de 12 elementos
plano = np.arange(12)

# Convertir a matriz de 3 filas x 4 columnas
matriz_3x4 = plano.reshape(3, 4)
print(matriz_3x4)

# Aplanar un array multidimensional de vuelta a 1D
plano_nuevamente = matriz_3x4.ravel()  # o .flatten()
print(plano_nuevamente)
```
