# Guía de Referencia y Operaciones en Pandas

Una guía técnica orientada a la manipulación, limpieza, filtrado y agregación de datos tabulares utilizando la librería **Pandas**.

## 📑 Índice

1. [Carga de Datos y Exploración Inicial](#1-carga-de-datos-y-exploración-inicial)
2. [Selección Avanzada (`loc`, `iloc`, `at`)](#2-selección-avanzada-loc-iloc-at)
3. [Filtrado Condicional y Máscaras Booleanas](#3-filtrado-condicional-y-máscaras-booleanas)
4. [Manipulación de Columnas y Tipos de Datos](#4-manipulación-de-columnas-y-tipos-de-datos)
5. [Manejo de Valores Nulos y Duplicados](#5-manejo-de-valores-nulos-y-duplicados)
6. [Ordenación y Estadísticos Descriptivos](#6-ordenación-y-estadísticos-descriptivos)
7. [Agrupaciones y Agregaciones (`groupby` y `agg`)](#7-agrupaciones-y-agregaciones-groupby-y-agg)
8. [Combinación y Unión de Tablas (`merge` y `concat`)](#8-combinación-y-unión-de-tablas-merge-y-concat)
9. [Lectura y Escritura de Ficheros (CSV y Excel)](#9-lectura-y-escritura-de-ficheros-csv-y-excel)

---

## 1. Carga de Datos y Exploración Inicial

La carga eficiente de ficheros estructurados es el primer paso en cualquier pipeline analítico.

```python
import pandas as pd

# Cargar un archivo CSV estándar
df_usuarios = pd.read_csv('usuarios.csv')

# Cargar un archivo separado por tabuladores (TSV) especificando el índice
df_productos = pd.read_csv('productos.csv', sep='\t', index_col='Codigo')

# Exploración estructural básica
print(df_usuarios.head(5))       # Primeras filas
print(df_usuarios.tail(3))       # Últimas filas
print(df_usuarios.shape)         # Dimensiones (filas, columnas)
print(df_usuarios.columns)       # Nombres de columnas
print(df_usuarios.index)         # Tipo de índice
print(df_usuarios.dtypes)        # Tipos de datos por columna
df_usuarios.info()               # Resumen completo con nulos y memoria

# Métricas de tamaño
total_filas = len(df_usuarios)
dimensiones = df_usuarios.shape
total_elementos = df_usuarios.size
valores_unicos = df_usuarios['Categoria'].nunique()
```

---

## 2. Selección Avanzada (`loc`, `iloc`, `at`)

El acceso preciso a celdas, filas y columnas se realiza mediante indexación explícita o posicional.

```python
# 1. Selección por etiquetas (.loc)
subset_etiqueta = df_usuarios.loc[0:10, ['Nombre', 'Edad', 'Correo']]

# 2. Selección por posición numérica (.iloc) - el límite superior no es inclusivo
subset_pos = df_usuarios.iloc[100:111, 0:4]

# 3. Acceso rápido a valores escalares (.at y .loc)
edad_usuario = df_usuarios.at[0, 'Edad']
edad_usuario_loc = df_usuarios.loc[0, 'Edad']

# 4. Modificación puntual de datos
df_usuarios.at[0, 'Edad'] = 30
```

---

## 3. Filtrado Condicional y Máscaras Booleanas

Las máscaras permiten aislar subconjuntos de datos aplicando expresiones lógicas vectorizadas.

```python
# Filtrado simple con operadores lógicos
mascara_edad = df_usuarios['Edad'] > 50
df_mayores = df_usuarios[mascara_edad]

# Filtrado con múltiples condiciones (AND, OR)
mascara_compleja = (df_usuarios['Edad'] > 25) & (df_usuarios['Activo'] == True)
df_filtrado = df_usuarios[mascara_compleja]

# Uso de métodos integrados (.isin() y .between())
df_ciudades = df_usuarios[df_usuarios['Ciudad'].isin(['Madrid', 'Barcelona', 'Valencia'])]
df_precio = df_usuarios[df_usuarios['Precio'].between(10, 100)]

# Filtrado dinámico mediante el método .query()
df_query = df_usuarios.query("Edad > 30 and Activo == True")

# Conteo de elementos filtrados
num_coincidentes = len(df_usuarios[mascara_edad])
```

---

## 4. Manipulación de Columnas y Tipos de Datos

La creación, transformación y gestión de columnas determina la calidad de la estructura de datos.

```python
# Creación de nuevas columnas mediante operaciones aritméticas
df_usuarios['Gasto_Total'] = df_usuarios['Precio'] + df_usuarios['Impuestos']

# Inserción de columnas en una posición específica con valores booleanos o condicionales
es_adulto = (df_usuarios['Edad'] >= 18).fillna(False)
df_usuarios.insert(0, 'EsAdulto', es_adulto)

# Renombrar columnas
df_usuarios = df_usuarios.rename(columns={'Edad': 'Anios', 'Correo': 'Email'})

# Eliminación de columnas
df_usuarios = df_usuarios.drop(columns=['Temporal1', 'Temporal2'])

# Conversión de tipos de datos (Cast)
df_usuarios['ID_Cliente'] = df_usuarios['ID_Cliente'].astype(str)

# Precaución con copias vs referencias en memoria
df_copia = df_usuarios.copy()       # Modificaciones no afectan al original
df_referencia = df_usuarios        # Modificaciones afectan directamente al original
```

---

## 5. Manejo de Valores Nulos y Duplicados

Identificar y tratar registros incompletos o redundantes asegura la integridad analítica.

```python
# Detección de nulos
conteo_nulos = df_usuarios.isnull().sum()

# Eliminación de filas con nulos en subconjuntos específicos
df_limpio = df_usuarios.dropna(subset=['Email', 'Telefono'])

# Imputación de nulos (rellenado con constantes, medias o propagación)
df_usuarios['Salario'] = df_usuarios['Salario'].fillna(df_usuarios['Salario'].mean())
df_usuarios['Estado'] = df_usuarios['Estado'].ffill()  # Propagar valor anterior

# Gestión de registros duplicados
num_duplicados = df_usuarios.duplicated().sum()
df_sin_duplicados = df_usuarios.drop_duplicates()

# Concatenación con eliminación de duplicados mediante índices limpios
df_concatenado = pd.concat([df_parte1, df_parte2], ignore_index=True)
```

---

## 6. Ordenación y Estadísticos Descriptivos

Obtener resúmenes estadísticos globales y ordenar la información de forma estructurada.

```python
# Ordenar valores por una o más columnas
df_ordenado = df_usuarios.sort_values(by=['Edad', 'Salario'], ascending=[False, True])

# Ordenar por el índice del DataFrame
df_por_indice = df_usuarios.set_index('Codigo').sort_index()

# Resumen estadístico para variables numéricas y categóricas
stats_num = df_usuarios.describe()
stats_cat = df_usuarios.describe(exclude='number')

# Frecuencias absolutas y relativas (normalizadas)
conteo_categorias = df_usuarios['Categoria'].value_counts()
proporcion_categorias = df_usuarios['Categoria'].value_counts(normalize=True)

# Correlación lineal entre variables numéricas
correlacion = df_usuarios['Edad'].corr(df_usuarios['Salario'])
```

---

## 7. Agrupaciones y Agregaciones (`groupby` y `agg`)

El patrón Split-Apply-Combine permite resumir métricas aplicando funciones estadísticas por categorías.

```python
# Agrupación simple con múltiples funciones de agregación
agrupacion_basica = df_usuarios.groupby('Departamento')['Salario'].agg(['mean', 'count', 'std'])

# Agrupación por múltiples claves
agrupacion_multiple = df_usuarios.groupby(['Departamento', 'Puesto'])['Salario'].mean().sort_values(ascending=False)

# Uso de diccionarios para aplicar funciones específicas por columna
diccionario_agregacion = {
    'Edad': 'mean',
    'Salario': 'max',
    'ID_Cliente': 'count'
}
df_resumen = df_usuarios.groupby('Departamento').agg(diccionario_agregacion).reset_index()

# Renombrado avanzado en agregaciones complejas
df_informe = df_usuarios.groupby(['Region', 'Segmento']).agg(
    salario_medio=('Salario', 'mean'),
    total_empleados=('ID_Cliente', 'count')
).reset_index()
```

---

## 8. Combinación y Unión de Tablas (`merge` y `concat`)

Integrar múltiples fuentes de datos relacionales mediante cruces de bases de datos.

```python
# 1. Cruces relacionales avanzados (Merge)
# Tipos de join: 'inner' (intersección), 'left' (todo el izquierdo), 'outer' (unión completa)
df_resultado_inner = pd.merge(df_empleados, df_departamentos, on='id_depto', how='inner')
df_resultado_left = pd.merge(df_empleados, df_departamentos, on='id_depto', how='left')

# 2. Apilado vertical u horizontal (Concat)
df_apilado_filas = pd.concat([df_bloque1, df_bloque2], axis=0, ignore_index=True)
df_unido_columnas = pd.concat([df_izq, df_der], axis=1)
```

---

## 9. Lectura y Escritura de Ficheros (CSV y Excel)

Persistencia de DataFrames transformados a formatos de almacenamiento estándar.

```python
# Carga de hojas de cálculo de Excel (requiere openpyxl)
df_excel = pd.read_excel('datos_financieros.xlsx', sheet_name='Balance')

# Exportar un DataFrame a CSV (con o sin índice)
df_usuarios.to_csv('usuarios_procesados.csv', index=False)

# Exportar manteniendo el índice personalizado
df_usuarios.set_index('ID_Cliente').to_csv('usuarios_con_indice.csv', index=True)

# Validación de lectura/escritura mediante verificación de igualdad
df_releido = pd.read_csv('usuarios_con_indice.csv', index_col=0)
son_iguales = df_releido.equals(df_usuarios.set_index('ID_Cliente'))
```
