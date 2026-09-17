# Notas del Proyecto - Métodos Estadísticos

## 1. Información General del Proyecto
- **Proyecto:** Proyecto Final del Curso de Métodos Estadísticos.
- **Autores:** Camilo Padilla & Juan Campo.
- **Formato:** Libro web interactivo generado con `bookdown` (`gitbook`) publicado en **GitHub Pages** (directorio `docs/`).
- **URL de Despliegue:** [https://padillanerio.github.io/Proyecto_Final_Metodos_Estadisticos/](https://padillanerio.github.io/Proyecto_Final_Metodos_Estadisticos/)

---

## 2. Dataset: Concrete Compressive Strength
- **Fuente:** UCI Machine Learning Repository (Prof. I-Cheng Yeh, 2007).
- **Observaciones ($N$):** 1,030 mezclas experimentales de concreto.
- **Valores faltantes:** 0 (conjunto de datos completo).
- **Ruta de archivo local:** `concrete+compressive+strength/Concrete_Data.xls`

### Diccionario de Variables y Mapeo a `df_clean`:
| Variable Original | Nombre en `df_clean` | Rol | Unidades | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `Cement (component 1)` | `cemento` | Predictora ($X_1$) | $\text{kg/m}^3$ | Aglutinante principal |
| `Blast Furnace Slag (component 2)` | `escoria` | Predictora ($X_2$) | $\text{kg/m}^3$ | Escoria de alto horno |
| `Fly Ash (component 3)` | `ceniza` | Predictora ($X_3$) | $\text{kg/m}^3$ | Ceniza volante (subproducto puzolánico) |
| `Water (component 4)` | `agua` | Predictora ($X_4$) | $\text{kg/m}^3$ | Contenido de agua en la mezcla |
| `Superplasticizer (component 5)` | `superplast` | Predictora ($X_5$) | $\text{kg/m}^3$ | Aditivo reductor de agua |
| `Coarse Aggregate (component 6)` | `agregado_grueso` | Predictora ($X_6$) | $\text{kg/m}^3$ | Grava / piedra triturada |
| `Fine Aggregate (component 7)` | `agregado_fino` | Predictora ($X_7$) | $\text{kg/m}^3$ | Arena |
| `Age (day)` | `edad` | Predictora ($X_8$) | Días ($1 - 365$) | Tiempo de curado |
| `Concrete compressive strength` | `resistencia` | **Respuesta ($Y$)** | $\text{MPa}$ | Resistencia mecánica a la compresión |

---

## 3. Estadísticas Descriptivas y Comportamientos Clave
- **Resistencia (`resistencia`):** $\mu = 35.82\text{ MPa}$, $\sigma = 16.70$, $\text{Mediana} = 34.44$, $\text{Min} = 2.33$, $\text{Max} = 85.59$. Distribución simétrica acampanada.
- **Cemento (`cemento`):** $\mu = 281.17\text{ kg/m}^3$, $\sigma = 104.51$, Asimetría = $0.51$, Curtosis = $2.48$. Correlación directa más fuerte con resistencia ($r = +0.498$).
- **Agua (`agua`):** $\mu = 181.57\text{ kg/m}^3$, $\sigma = 21.36$, Asimetría = $0.07$, Curtosis = $3.12$. Distribución normal estándar, relación inversa ($r = -0.290$, Ley de Abrams).
- **Ceniza Volante (`ceniza`):** $\mu = 54.19\text{ kg/m}^3$, $\sigma = 63.99$. Bimodal inflada en cero ($55\%$ de mezclas control con $0\text{ kg/m}^3$). Correlación lineal simple aislada $r = -0.106$.
- **Tiempo de Curado (`edad`):** $\mu = 45.66\text{ días}$, $\sigma = 63.17$. Concentrado en $28\text{ días}$ y edades $\le 7\text{ días}$. Relación logarítmica ($\log(\text{edad})$ tiene $r = +0.550$ con resistencia).

---

## 4. Hallazgos Estadísticos para Modelación e Inferencia
1. **Interacción Ceniza $\times$ Edad:**
   - La ceniza volante es de reacción lenta (puzolánica). En un modelo múltiple con $\text{ceniza} \times \log(\text{edad})$, el término de interacción es estadísticamente significativo ($p = 0.0394$).
2. **Ingeniería de Características:**
   - `razon_agua_cementante = agua / (cemento + ceniza)` $\rightarrow$ Fuerte correlación negativa con la resistencia ($r \approx -0.50$).
   - `etapa_curado` $\rightarrow$ Factor: Temprano ($\le 7\text{d}$), Estándar ($14-28\text{d}$), Tardío ($>28\text{d}$).

---

## 5. Estructura de Capítulos del Documento
- `index.Rmd`: Presentación y metadatos del proyecto.
- `01-introduccion.Rmd`: Contexto, problemática, justificación, objetivo general y 5 objetivos específicos.
- `02-eda-concrete.Rmd`: Ficha técnica, diccionario, análisis univariado (densidad + boxplots), bivariado (dispersión) y multivariado (`ggpairs`, feature engineering).
- Capítulos siguientes: Inferencia / Intervalos de confianza, Pruebas de hipótesis / ANOVA, y Regresión Lineal Múltiple con diagnóstico de residuales.

---

## 6. Comandos de Compilación y Configuración Técnica
- **Rscript:** `C:\Program Files\R\R-4.6.1\bin\Rscript.exe`
- **Pandoc:** `C:\Program Files\RStudio\resources\app\bin\quarto\bin\tools`
- **Comando PowerShell para renderizar Bookdown:**
```powershell
$env:RSTUDIO_PANDOC="C:\Program Files\RStudio\resources\app\bin\quarto\bin\tools"
& "C:\Program Files\R\R-4.6.1\bin\Rscript.exe" -e "Sys.setenv(RSTUDIO_PANDOC='C:/Program Files/RStudio/resources/app/bin/quarto/bin/tools'); bookdown::render_book('index.Rmd', 'bookdown::gitbook')"
```
- **GitHub Pages:** Servido desde la carpeta `/docs` en la rama `main`. Requiere el archivo `docs/.nojekyll`.
