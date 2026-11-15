# Guía Definitiva de Pandas para Ciencia de Datos

Una guía profesional, práctica y fácil de entender para dominar la manipulación y análisis de datos en Python utilizando la librería **Pandas**.

---

## 📈 Índice
1. [Introducción y Configuración](#1-introducción-y-configuración)
2. [Estructuras de Datos Principales](#2-estructuras-de-datos-principales)
3. [Carga y Exportación de Datos](#3-carga-y-exportación-de-datos)
4. [Exploración Inicial](#4-exploración-inicial)
5. [Selección, Indexación y Filtrado](#5-selección-indexación-y-filtrado)
6. [Limpieza y Transformación de Datos](#6-limpieza-y-transformación-de-datos)
7. [Agrupaciones y Agregaciones (GroupBy)](#7-agrupaciones-y-agregaciones-groupby)
8. [Combinación de DataFrames (Merge, Join, Concat)](#8-combinación-de-dataframes-merge-join-concat)
9. [Buenas Prácticas y Consejos de Rendimiento](#9-buenas-prácticas-y-consejos-de-rendimiento)

---

## 1. Introducción y Configuración

Pandas es la librería estándar en Python para el análisis de datos. Está construida sobre NumPy y proporciona estructuras de datos rápidas, flexibles y expresivas.

```python
import pandas as pd
import numpy as np

# Configurar visualización para ver más columnas en la consola
pd.set_option('display.max_columns', 100)
pd.set_option('display.width', 1000)
```

---

## 2. Estructuras de Datos Principales

Pandas maneja principalmente dos estructuras de datos:
* **`Series`**: Un array unidimensional con etiquetas (similar a una columna o una fila).
* **`DataFrame`**: Una estructura tabular bidimensional con filas y columnas etiquetadas (similar a una tabla SQL o una hoja de Excel).

```python
# Crear una Serie
serie = pd.Series([10, 20, 30, 40], index=['a', 'b', 'c', 'd'])

# Crear un DataFrame
datos = {
    'Nombre': ['Ana', 'Carlos', 'Sofía', 'Luis'],
    'Edad': [28, 34, 22, 41],
    'Ciudad': ['Madrid', 'Barcelona', 'Valencia', 'Sevilla']
}
df = pd.DataFrame(datos)
```

---

## 3. Carga y Exportación de Datos

Pandas soporta una gran variedad de formatos de archivo. Los más comunes son CSV, Excel, JSON y bases de datos SQL.

```python
# Cargar datos
df_csv = pd.read_csv('datos.csv')
df_excel = pd.read_excel('datos.xlsx', sheet_name='Hoja1')
df_json = pd.read_json('datos.json')

# Exportar datos (evitando guardar el índice por defecto)
df.to_csv('resultado.csv', index=False)
df.to_excel('resultado.xlsx', index=False)
```

---

## 4. Exploración Inicial

Antes de transformar los datos, es vital entender su estructura, tipos de datos y posibles anomalías.

```python
df.head(5)       # Primeras 5 filas
df.tail(3)       # Útimas 3 filas
df.shape         # Dimensiones (filas, columnas)
df.info()        # Tipos de datos y valores no nulos
df.describe()    # Resumen estadístico de variables numéricas
df.columns       # Nombres de las columnas
df.dtypes        # Tipos de datos por columna
```

---

## 5. Selección, Indexación y Filtrado

### Selección de columnas y filas
```python
# Seleccionar una columna (retorna una Serie)
edades = df['Edad']

# Seleccionar múltiples columnas (retorna un DataFrame)
sub_df = df[['Nombre', 'Ciudad']]

# Selección por etiqueta (loc) y posición numérica (iloc)
df.loc[0]            # Fila con etiqueta 0
df.iloc[0:2]         # Primeras dos filas por posición
df.loc[0, 'Nombre']  # Celda específica por etiqueta
```

### Filtrado condicional
```python
# Filtrar mayores de 30 años
mayores_30 = df[df['Edad'] > 30]

# Filtrar con múltiples condiciones (AND: &, OR: |)
filtro_multiple = df[(df['Edad'] > 25) & (df['Ciudad'] == 'Madrid')]
```

---

## 6. Limpieza y Transformación de Datos

### Manejo de Valores Nulos
```python
df.isnull().sum()            # Contar valores nulos por columna
df_clean = df.dropna()       # Eliminar filas con cualquier valor nulo
df_filled = df.fillna(0)     # Rellenar nulos con un valor constante
df['Edad'] = df['Edad'].fillna(df['Edad'].median()) # Rellenar con la mediana
```

### Duplicados y Tipos de Datos
```python
df.duplicated().sum()        # Contar filas duplicadas
df = df.drop_duplicates()    # Eliminar duplicados

# Cambiar tipo de dato
df['Edad'] = df['Edad'].astype(int)
```

### Creación y Modificación de Columnas
```python
# Crear una nueva columna basada en operaciones
df['Edad_en_10_anos'] = df['Edad'] + 10

# Aplicar funciones personalizadas (Lambda o funciones externas)
df['Nombre_Mayus'] = df['Nombre'].apply(lambda x: x.upper())
```

---

## 7. Agrupaciones y Agregaciones (GroupBy)

Permite dividir los datos en grupos basados en criterios y aplicar funciones estadísticas.

```python
# Agrupar por ciudad y calcular la edad promedio
promedio_edad = df.groupby('Ciudad')['Edad'].mean()

# Múltiples funciones de agregación
resumen = df.groupby('Ciudad')['Edad'].agg(['min', 'max', 'mean', 'count'])
```

---

## 8. Combinación de DataFrames

### Concatenación (Unir vertical u horizontalmente)
```python
# Apilar DataFrames (filas)
df_total = pd.concat([df1, df2], axis=0, ignore_index=True)

# Unir columnas
df_lados = pd.concat([df_izq, df_der], axis=1)
```

### Cruces (Merge / Joins)
```python
# Cruce tipo SQL (Inner Join)
df_merged = pd.merge(df_empleados, df_departamentos, on='id_depto', how='inner')
# Opciones de 'how': 'inner', 'outer', 'left', 'right'
```

---

## 9. Buenas Prácticas y Consejos de Rendimiento

1. **Evita bucles `for` explícitos**: Recorrer filas con `for` es extremadamente lento en Pandas. Utiliza operaciones vectorizadas o métodos como `.apply()`.
2. **Usa tipos de datos eficientes**: Si una columna categórica tiene pocos valores únicos, conviértela a tipo `category` para ahorrar memoria:
   ```python
   df['Ciudad'] = df['Ciudad'].astype('category')
   ```
3. **Carga masiva por bloques**: Si trabajas con archivos CSV gigantescos que no caben en memoria RAM, usa el parámetro `chunksize`:
   ```python
   for chunk in pd.read_csv('archivo_gigante.csv', chunksize=10000):
       # Procesar cada chunk
       pass
   ```
