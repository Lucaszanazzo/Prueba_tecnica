# Prueba Técnica: Ingeniería de Datos (SQL & PySpark)

Este repositorio contiene la resolución de una prueba práctica de Ingeniería de Datos enfocada en la limpieza, transformación, análisis y exportación de datos utilizando **SQL (SQLite)** y **Apache Spark (PySpark)**.

## 🔗 Acceso al Notebook

* **Google Colab:** [Abrir Notebook en Google Colab](https://colab.research.google.com/github/Lucaszanazzo/Prueba_tecnica/blob/Luquitas_resolve/Prueba_T%C3%A9cnica_DE_BP.ipynb#scrollTo=TKYVXg-2aBRb)

---

## 📋 Problemática General

En entornos reales de ingeniería de datos, la información proviene de fuentes heterogéneas y no estructuradas (portales web, archivos JSON multilínea o listas en memoria) con problemas de calidad como tildes, caracteres inconsistentes, tipos de datos incorrectos o nombres de entidades sin normalizar. El objetivo de esta prueba es construir flujos de procesamiento que limpien, integren y modelen estos datos para responder a requerimientos analíticos y de negocio.

---

## 🛠️ Estructura y Solución de los Ejercicios

### Sección 1: Análisis Relacional con SQL (SQLite)

1. **Tabla de Posiciones (Qatar 2022):**
   * *Problemática:* Datos crudos de partidos con resultados en texto separados por comas y nombres de equipos inconsistentes.
   * *Solución:* Limpieza mediante funciones de cadena (`TRIM`, `REPLACE`, `SUBSTR`, `INSTR`), unificación de partidos por equipo con `UNION ALL` y cálculo de puntos y diferencia de goles almacenados en una vista (`CREATE VIEW`).
2. **Clasificados y Cruces de Segunda Ronda:**
   * *Problemática:* Identificar a los dos mejores equipos por grupo bajo criterios de desempate y cruzarlos con el calendario de octavos de final.
   * *Solución:* Uso de funciones de ventana (`ROW_NUMBER() OVER`) particionadas por grupo y cruce dinámico (`LEFT JOIN` con `LIKE`) contra la tabla de sedes y horarios.
3. **Identificación de Equipos Élite:**
   * *Problemática:* Seleccionar automáticamente a las selecciones ubicadas en el quintil superior (Top 20%) según puntaje acumulado.
   * *Solución:* Cálculo de distribución porcentual mediante `PERCENT_RANK()` y filtrado del percentil `<= 0.20`.

### Sección 2: Procesamiento Distribuido con PySpark

4. **Ranking de Goleadores Históricos y Exportación JSON:**
   * *Problemática:* Consolidar listas en memoria de jugadores y países, identificar líderes por país, proponer un ranking por efectividad y generar un JSON anidado.
   * *Solución:* Creación de DataFrames con casteo de tipos (`cast("int")`), cruce y marcado mediante agregaciones (`groupBy`, `min`, `when`), cálculo de promedio de gol por partido (`round`) y empaquetado jerárquico (`collect_list`, `struct`) exportado a `.json`.
5. **Ubicación Óptima de Centros Educativos (Madrid):**
   * *Problemática:* Determinar qué centro educativo es el más céntrico dentro de cada entidad titular a partir de coordenadas espaciales en un JSON multilínea y evaluar formatos de almacenamiento.
   * *Solución:* Lectura multilínea (`option("multiline", "true")`), cálculo de centroides por grupo (`avg`), implementación de una **UDF** para distancia euclidiana, selección del centro más cercano con `Window` y `row_number()`, y exportación a **Parquet**, **JSON** y **CSV** (delimitado por `|`). Se documentó la elección de **Parquet** por su eficiencia columnar y compresión en Databricks.

---

## ⚙️ Tecnologías Utilizadas

* **Lenguajes:** Python 3.10, SQL
* **Frameworks y Librerías:** PySpark, SQLite3, Pandas, PyArrow
* **Entorno:** Jupyter Notebook / Google Colab / VS Code
