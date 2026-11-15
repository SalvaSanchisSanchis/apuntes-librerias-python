# 📚 Cheatsheet: Numpy, Pandas, Matplotlib Essentials

## 🔢 **Numpy**  
### 📦 Arrays  
- `np.array()`  
- `np.zeros(shape)`  
- `np.ones(shape)`  

### 🧮 Operaciones matemáticas  
- `np.sum()`  
- `np.mean()`  
- `np.std()`  

### 🎲 Aleatoriedad  
- `np.random.rand()`  
- `np.random.randn()`  

### 🔍 Indexación  
- `arr[0,1]`  
- `arr[:,1:3]`  

### 📐 Álgebra lineal  
- `np.dot(a, b)`  
- `np.linalg.inv(matrix)`  

---

## 📊 **Pandas**  
### 📋 DataFrames  
- `pd.DataFrame()`  
- `df.head(n)`  

### 📁 Lectura/Escritura  
- `pd.read, csv('file.csv')`  
- `df.to_csv('file.csv', index=False)`  

### 🔄 Manipulación  
- `df.groupby('column')`  
- `df.merge(df2, on='key')`  
- `df.fillna(value)`  

### 📈 Estadísticas  
- `df.describe()`  
- `df.corr()`  

---

## 📈 **Matplotlib**  
### 📈 Gráficos  
- `plt.plot(x, y)`  
- `plt.bar(x, height)`  
- `plt.scatter(x, y)`  

### 🎨 Personalización  
- `plt.title('Title')`  
- `plt.xlabel('X-axis')`  
- `plt.ylabel('Y-axis')`  
- `plt.legend()`  

### 📌 Estilo  
- `plt.style.use('ggplot')`  

### 🧾 Subplots  
- `plt.subplots(nrows=1, ncols=2)`  

### 📁 Guardar  
- `plt.savefig('plot.png')`  

### 🖼️ Mostrar  
- `plt.show()`  

---

### ✅ Nota:  
Este documento está listo para convertirse a PDF usando herramientas como:  
- **Pandoc**: `pandoc cheatsheet.md -o cheatsheet.pdf`  
- **Google Docs**: Copia y pega el contenido y exporta a PDF.  
- **Convertidores en línea**: [Smallpdf](https://smallpdf.com/), [PDFmyURL](https://pdfmyurl.com/).
