# Chuleta de Matplotlib

## Índice
1. [Importación y Creación de Figuras](#1-importación-y-creación-de-figuras)
2. [Gráficos de Líneas](#2-gráficos-de-líneas)
3. [Diagramas de Dispersión](#3-diagramas-de-dispersión)
4. [Gráficos de Barras](#4-gráficos-de-barras)
5. [Histogramas](#5-histogramas)
6. [Diagramas de Caja (Boxplot)](#6-diagramas-de-caja-boxplot)
7. [Diagramas Circulares](#7-diagramas-circulares)
8. [Personalización de Gráficos](#8-personalización-de-gráficos)
9. [Múltiples Gráficos y Subplots](#9-múltiples-gráficos-y-subplots)
10. [Integración con Pandas](#10-integración-con-pandas)
11. [Guardar Figuras](#11-guardar-figuras)

---

## 1. Importación y Creación de Figuras

### Importación estándar
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```

### Crear una figura y sus ejes
```python
fig, ax = plt.subplots(figsize=(8, 4))
```
- `fig`: objeto que representa la figura completa.
- `ax`: objeto que representa los ejes donde dibujamos.
- `figsize=(ancho, alto)`: tamaño de la figura en pulgadas.

### Mostrar el gráfico
```python
plt.show()                 # Mostrar el gráfico
```

### Crear una figura directamente
```python
fig = plt.figure(figsize=(15, 5))
```

---

## 2. Gráficos de Líneas

### Crear un gráfico de líneas
```python
x = np.array([1, 2, 3, 4, 5])
y = np.array([12, 14, 11, 17, 19])

fig, ax = plt.subplots(figsize=(8, 4))

ax.plot(x, y)
```

### Personalizar una línea
```python
ax.plot(
    x, y,
    color="tab:red",       # Color de la línea
    marker="o",            # Marcador de los puntos
    linestyle="--",        # Estilo de línea
    linewidth=2,           # Grosor de línea
    label="Temperaturas"  # Nombre para la leyenda
)
```

### Estilos de línea
```python
linestyle="-"
linestyle="--"
linestyle=":"
linestyle="-."
```

### Añadir información al gráfico
```python
ax.set_title("Temperaturas durante la semana")
ax.set_xlabel("Día")
ax.set_ylabel("Temperatura (°C)")
ax.grid(True)
ax.legend()
```

### Dibujar varias líneas
```python
x = np.linspace(0, 10, 100)

fig, ax = plt.subplots(figsize=(10, 5))

ax.plot(x, np.sin(x), color="tab:blue",
        linestyle="-", linewidth=2, label="sin(x)")

ax.plot(x, np.cos(x), color="tab:red",
        linestyle="--", linewidth=1.5, label="cos(x)")

ax.plot(x, np.sin(x) * np.exp(-x / 5),
        color="tab:green", linestyle=":", linewidth=2.5,
        label="Senoidal amortiguada")

ax.legend()
plt.show()
```

### Utilizar directamente plt
```python
plt.plot(x, y)
plt.title("Mi gráfico")
plt.xlabel("Eje X")
plt.ylabel("Eje Y")
plt.grid(True)
plt.show()
```

---

## 3. Diagramas de Dispersión

### Crear un diagrama de dispersión
```python
fig, ax = plt.subplots(figsize=(8, 5))

ax.scatter(
    df_coches["kms"],       # Eje X
    df_coches["precio"],    # Eje Y
    color="tab:blue",       # Color de los puntos
    alpha=0.7,              # Transparencia
    s=60                    # Tamaño de los puntos
)
```

### Personalizar el gráfico
```python
ax.set_title("Relación entre kilometraje y precio")
ax.set_xlabel("Kilometraje (km)")
ax.set_ylabel("Precio (€)")
ax.grid(True)
```

### Colorear puntos según una variable
```python
fig, ax = plt.subplots(figsize=(8, 5))

scatter = ax.scatter(
    df_coches["kms"],
    df_coches["precio"],
    c=df_coches["precio"],  # Variable que determina el color
    cmap="viridis",         # Mapa de colores
    alpha=0.7,
    s=80
)

fig.colorbar(scatter, ax=ax, label="Precio (€)")
```

### Guardar el gráfico
```python
fig.savefig("dispersion_coches.png")
```

---

## 4. Gráficos de Barras

### Barras verticales
```python
fig, ax = plt.subplots(figsize=(14, 6))

ax.bar(
    range(len(df_tecno)),
    df_tecno["Employees"],
    color="tab:blue"
)

ax.set_xticks(range(len(df_tecno)))
ax.set_xticklabels(df_tecno["Company"], rotation=90)

ax.set_title("Empleados de cada empresa")
ax.set_ylabel("Empleados")

plt.show()
```

### Barras horizontales
```python
df_ordenado = df_tecno.sort_values(
    by="Revenue",
    ascending=True
)

fig, ax = plt.subplots(figsize=(10, 8))

ax.barh(
    df_ordenado["Company"],
    df_ordenado["Revenue"],
    color="tab:orange"
)

ax.set_title("Ingresos por empresa")
ax.set_xlabel("Ingresos")
ax.set_ylabel("Empresa")

plt.show()
```

### Barras agrupadas
```python
top6 = df_tecno.nlargest(6, "Employees")

x = np.arange(len(top6))
ancho = 0.35

fig, ax = plt.subplots(figsize=(12, 6))

ax.bar(
    x - ancho / 2,
    top6["Employees"] / 1000,
    ancho,
    label="Empleados (miles)"
)

ax.bar(
    x + ancho / 2,
    top6["Revenue"] / 1000,
    ancho,
    label="Ingresos (miles de millones)"
)

ax.set_xticks(x)
ax.set_xticklabels(top6["Company"], rotation=90)

ax.set_title("Barras agrupadas")
ax.legend()

plt.show()
```

### Barras apiladas
```python
fig, ax = plt.subplots(figsize=(10, 6))

ax.bar(
    top6["Company"],
    top6["Employees"] / 1000,
    label="Empleados"
)

ax.bar(
    top6["Company"],
    top6["Revenue"] / 1000,
    bottom=top6["Employees"] / 1000,
    label="Ingresos"
)

ax.set_title("Barras apiladas")
ax.set_xlabel("Empresa")
ax.set_ylabel("Valor")

ax.set_xticklabels(top6["Company"], rotation=90)
ax.legend()

plt.show()
```

**Diferencia importante:**
- `bar()`: barras verticales.
- `barh()`: barras horizontales.
- Barras agrupadas: se utiliza `x - ancho/2` y `x + ancho/2`.
- Barras apiladas: se utiliza `bottom=`.

---

## 5. Histogramas

### Crear un histograma
```python
edades = df_titanic["Age"].dropna()

fig, ax = plt.subplots(figsize=(8, 5))

ax.hist(
    edades,
    bins=20,
    color="tab:blue",
    edgecolor="black"
)

ax.set_title("Histograma de edades")
ax.set_xlabel("Edad")
ax.set_ylabel("Frecuencia")
ax.grid(True)

plt.show()
```

### Cambiar el número de intervalos
```python
ax.hist(edades, bins=5)
ax.hist(edades, bins=20)
ax.hist(edades, bins=40)
```

### Añadir una línea vertical
```python
media_edades = edades.mean()

ax.axvline(
    media_edades,
    color="red",
    linestyle="--",
    label=f"Media: {media_edades:.2f} años"
)

ax.legend()
```

---

## 6. Diagramas de Caja (Boxplot)

### Crear un diagrama de caja
```python
especies = ["Adelie", "Chinstrap", "Gentoo"]

masas = [
    df_penguins.loc[
        df_penguins["species"] == esp,
        "body_mass_g"
    ].values
    for esp in especies
]

fig, ax = plt.subplots(figsize=(8, 5))

ax.boxplot(
    masas,
    tick_labels=["Adelie", "Chinstrap", "Gentoo"]
)

ax.set_title("Masa corporal por especie")
ax.set_ylabel("Masa corporal (g)")

plt.show()
```

### Boxplot por categorías
```python
tarifas = [
    df_titanic.loc[df_titanic["Pclass"] == c, "Fare"].values
    for c in (1, 2, 3)
]

fig, ax = plt.subplots(figsize=(8, 5))

ax.boxplot(
    tarifas,
    tick_labels=["1ª", "2ª", "3ª"]
)

ax.set_title("Tarifa por clase")
ax.set_ylabel("Tarifa (£)")
ax.set_ylim(0, 200)

plt.show()
```

### Personalizar el color de las cajas
```python
ax.boxplot(
    tarifas,
    tick_labels=["1ª", "2ª", "3ª"],
    showfliers=True,
    patch_artist=True,
    boxprops=dict(
        facecolor="lightblue",
        edgecolor="black"
    )
)
```

**Parámetros importantes:**
- `tick_labels`: nombres de las categorías.
- `showfliers=True`: mostrar valores atípicos.
- `patch_artist=True`: permitir colorear las cajas.
- `boxprops`: propiedades visuales de las cajas.
- `set_ylim()`: limitar el eje Y.

---

## 7. Diagramas Circulares

### Crear un diagrama circular
```python
supervivencia = df_titanic["Survived"].value_counts()

datos = [
    supervivencia[0],
    supervivencia[1]
]

etiquetas = ["No sobrevivió", "Sobrevivió"]

fig, ax = plt.subplots(figsize=(8, 6))

ax.pie(
    datos,
    labels=etiquetas,
    startangle=90,
    autopct="%1.1f%%",
    shadow=True
)

ax.set_title("Supervivientes del Titanic")

plt.show()
```

### Personalizar el diagrama circular
```python
ax.pie(
    datos,
    labels=etiquetas,
    startangle=90,
    autopct="%1.1f%%",
    shadow=True,
    explode=(0, 0.1),
    colors=["tab:gray", "tab:green"]
)
```

### Añadir una leyenda
```python
ax.legend(labels=etiquetas)
```

### Diagrama circular con pandas
```python
especies = df_penguins["species"].value_counts()

fig, ax = plt.subplots(figsize=(8, 6))

ax.pie(
    especies,
    labels=especies.index,
    startangle=90,
    autopct="%1.1f%%"
)

ax.set_title("Pingüinos por especie")
ax.legend(labels=especies.index)

plt.show()
```

**Parámetros importantes:**
- `labels`: etiquetas de los sectores.
- `startangle`: ángulo de inicio.
- `autopct`: mostrar porcentajes.
- `shadow`: añadir sombra.
- `explode`: separar sectores.
- `colors`: asignar colores.

---

## 8. Personalización de Gráficos

### Título y etiquetas
```python
ax.set_title("Título del gráfico")
ax.set_xlabel("Eje X")
ax.set_ylabel("Eje Y")
```

### Límites de los ejes
```python
ax.set_xlim(0, 10)
ax.set_ylim(-1.2, 1.2)
```

### Cambiar las marcas de los ejes
```python
ax.set_xticks(np.arange(0, 11, 2))

ax.set_xticklabels(
    ["0", "2", "4", "6", "8", "10"],
    rotation=45
)
```

### Leyenda
```python
ax.legend()
ax.legend(loc="lower left")
```

Posiciones habituales:
```python
loc="upper right"
loc="upper left"
loc="lower right"
loc="lower left"
loc="best"
```

### Rejilla
```python
ax.grid(True)
ax.grid(axis="x")
ax.grid(axis="y")
ax.grid(axis="both")
```

### Colores
```python
color="tab:blue"
color="tab:red"
color="tab:green"
color="tab:orange"
color="lightblue"
color="black"
```

### Transparencia
```python
alpha=0.7
```

### Grosor de línea
```python
linewidth=2
```

### Estilo de línea
```python
linestyle="-"
linestyle="--"
linestyle=":"
linestyle="-."
```

### Marcadores
```python
marker="o"
```

---

## 9. Múltiples Gráficos y Subplots

### Crear varios gráficos en una figura
```python
fig, axes = plt.subplots(
    2, 2,
    figsize=(12, 8),
    sharey=True
)
```

- `2, 2`: dos filas y dos columnas.
- `figsize`: tamaño total.
- `sharey=True`: compartir el eje Y.

### Acceder a cada subplot
```python
axes[0, 0]    # Arriba izquierda
axes[0, 1]    # Arriba derecha
axes[1, 0]    # Abajo izquierda
axes[1, 1]    # Abajo derecha
```

### Dibujar diferentes gráficos
```python
axes[0, 0].plot(x, y)
axes[0, 1].scatter(x, y)
axes[1, 0].bar(x, y)
axes[1, 1].hist(y, bins=20)
```

### Añadir títulos y etiquetas
```python
axes[0, 0].set_title("Gráfico de líneas")
axes[0, 1].set_title("Dispersión")
axes[1, 0].set_title("Barras")
axes[1, 1].set_title("Histograma")

axes[0, 0].set_ylabel("Precio (€)")
axes[0, 1].set_xlabel("Kilómetros")
```

### Ajustar los márgenes
```python
fig.tight_layout()
```

### Crear varios subplots en una fila
```python
fig, (ax1, ax2, ax3) = plt.subplots(
    1, 3,
    figsize=(15, 5)
)

ax1.hist(edades, bins=5)
ax2.hist(edades, bins=20)
ax3.hist(edades, bins=40)

fig.tight_layout()
plt.show()
```

### Crear ejes con add_subplot()
```python
fig = plt.figure(figsize=(15, 5))

ax1 = fig.add_subplot(1, 3, 1)
ax2 = fig.add_subplot(1, 3, 2)
ax3 = fig.add_subplot(1, 3, 3)
```

La estructura es:
```python
fig.add_subplot(filas, columnas, posicion)
```

---

## 10. Integración con Pandas

Matplotlib permite dibujar directamente utilizando los métodos de gráficos de pandas.

### Cargar un CSV
```python
df_coches = pd.read_csv("coches.csv")

df_tecno = pd.read_csv("tecno.csv", sep="\t")

df_canarias = pd.read_csv(
    "democanarias.csv",
    index_col="Island"
)
```

### Gráfico de barras verticales
```python
df_canarias.plot(
    kind="bar",
    y="Population",
    ax=ax1,
    title="Población por isla",
    legend=False
)
```

### Gráfico de barras horizontales
```python
df_canarias.plot(
    kind="barh",
    y="Area",
    ax=ax2,
    title="Superficie por isla",
    legend=False
)
```

### Gráfico circular
```python
df_canarias.plot(
    kind="pie",
    y="Population",
    ax=ax3,
    title="Población por isla",
    autopct="%1.1f%%"
)
```

### Crear intervalos con pandas
```python
df_coches["tramo"] = pd.cut(
    df_coches["kms"],
    bins=5
)

precio_medio = df_coches.groupby(
    "tramo",
    observed=True
)["precio"].mean()
```

### Ordenar un DataFrame
```python
df_ordenado = df_coches.sort_values(
    by="precio",
    ascending=True
)
```

### Seleccionar los mayores valores
```python
top6 = df_tecno.nlargest(6, "Employees")
```

---

## 11. Guardar Figuras

### Guardar en PNG
```python
fig.savefig("grafico.png")
```

### Guardar en PNG con resolución
```python
fig.savefig("canarias.png", dpi=150)
```

### Guardar en PDF
```python
fig.savefig("canarias.pdf")
```

### Guardar una figura con varios gráficos
```python
fig.tight_layout()
fig.savefig("panel_coches.png")
```

### Comprobar que se ha guardado
```python
import os

os.path.exists("grafico.png")
```

---

## Resumen rápido de los comandos más importantes

| Comando | Función |
|---|---|
| `plt.subplots()` | Crear figura y ejes |
| `plt.figure()` | Crear figura |
| `ax.plot()` | Gráfico de líneas |
| `ax.scatter()` | Diagrama de dispersión |
| `ax.bar()` | Barras verticales |
| `ax.barh()` | Barras horizontales |
| `ax.hist()` | Histograma |
| `ax.boxplot()` | Diagrama de caja |
| `ax.pie()` | Diagrama circular |
| `ax.axvline()` | Línea vertical |
| `ax.set_title()` | Título |
| `ax.set_xlabel()` | Etiqueta eje X |
| `ax.set_ylabel()` | Etiqueta eje Y |
| `ax.set_xticks()` | Marcas del eje X |
| `ax.set_xticklabels()` | Texto de las marcas X |
| `ax.set_xlim()` | Límites eje X |
| `ax.set_ylim()` | Límites eje Y |
| `ax.grid()` | Rejilla |
| `ax.legend()` | Leyenda |
| `fig.colorbar()` | Barra de colores |
| `fig.tight_layout()` | Ajustar márgenes |
| `fig.add_subplot()` | Añadir ejes |
| `fig.savefig()` | Guardar figura |
| `plt.show()` | Mostrar gráfico |
