# Contexto — Sesión 01 de Trabajo

**Fecha:** 13 de abril de 2026  
**Participantes:** Cristian Illanes + Claude (IA)  
**Propósito:** Registro de decisiones, preguntas y respuestas relevantes para el desarrollo de la tesis.

---

## 1. Configuración inicial del repositorio

Se creó el repositorio `UDP-TesidModeloFamaFrench` y se subió el README con la estructura de la tesis, los autores y el plan de desarrollo.

**Autores definidos:** Álvaro Gatica, Fernanda Rojas, Luis Pizarro y Cristian Illanes  
**Convención de idioma:** Todo el proyecto — código, comentarios, documentación y nombres de variables — está escrito en **español**.

---

## 2. Primer notebook creado

Se generó `notebooks/01_datos_estadisticas.ipynb` con comentarios línea a línea en español, tabla explicativa de cada librería y contexto económico en las celdas markdown.

**Librerías principales:**

| Librería | Rol |
|---|---|
| `pandas` | Manipulación de datos tabulares |
| `numpy` | Álgebra lineal y operaciones numéricas |
| `pandas_datareader` | Descarga de factores FF6 desde la librería de Kenneth French |
| `yfinance` | Descarga de precios históricos desde Yahoo Finance |
| `statsmodels` | Regresiones OLS de series de tiempo |
| `arch` | Modelos GARCH y GJR-GARCH |
| `cvxpy` | Optimización convexa (GMVP, MV, Max Sharpe) |
| `scipy` | Estadística y distribuciones de probabilidad |
| `matplotlib` / `seaborn` | Visualizaciones |

---

## 3. ¿De dónde vienen los retornos?

**Pregunta:** ¿De dónde vienen los retornos que usa el modelo?

**Respuesta:** Los retornos se calculan a partir de los **precios de cierre ajustados** que descarga `yfinance` desde Yahoo Finance.

La fórmula es:

```
Retorno_t = (Precio_t - Precio_{t-1}) / Precio_{t-1}
```

El precio **ajustado** incorpora retroactivamente dividendos y splits, lo que permite calcular retornos totales reales sin sesgos por eventos corporativos.

Se calculan en dos frecuencias:
- **Diarios:** para los modelos GARCH (notebook 04)
- **Mensuales:** para las regresiones FF6 (alineados con la frecuencia de los factores de la librería de French)

---

## 4. Ejemplo de datos reales: 2000–2015

Se descargaron y verificaron los datos del período 200001–201501. Muestra de los factores FF6:

| Fecha | Mkt-RF | SMB | HML | RMW | CMA | RF | WML |
|---|---|---|---|---|---|---|---|
| 2000-01 | -0.0474 | +0.0421 | -0.0112 | -0.0615 | +0.0456 | 0.0041 | +0.0186 |
| 2000-02 | +0.0245 | +0.1846 | -0.0977 | -0.1895 | -0.0113 | 0.0043 | +0.1802 |
| 2000-03 | +0.0521 | -0.1554 | +0.0850 | +0.1165 | -0.0120 | 0.0047 | -0.0685 |

Los valores son **retornos decimales mensuales** (ej. `-0.0474` = caída de 4.74% en enero 2000).

Los datos se guardaron en Google Drive (carpeta `1. Datos`, cuenta `cristian.illanes@mail.udp.cl`):

| Archivo | Contenido | Dimensiones |
|---|---|---|
| `factores_ff6_mensuales_200001_201501.csv` | 7 factores FF6 + RF | 125 meses × 7 cols |
| `retornos_mensuales_200001_201501.csv` | Retornos mensuales de 16 activos | 125 meses × 16 cols |
| `retornos_diarios_200001_201501.csv` | Retornos diarios de 16 activos | 2.630 días × 16 cols |

---

## 5. Decisión pendiente: universo de activos

**Pregunta:** ¿Las empresas seleccionadas (AAPL, MSFT, JPM, etc.) tienen sustento en su elección?

**Respuesta:** **No.** Son una selección por defecto usada para ilustrar el código. Tienen dos problemas académicos graves:

1. **Sesgo de supervivencia:** se eligieron empresas que hoy son exitosas, sobreestimando retornos históricos.
2. **No representan bien los factores:** SMB y HML necesitan variedad real en tamaño y múltiplos de valoración.

### Opciones académicamente válidas

| Opción | Universo | Sustento |
|---|---|---|
| **A — Portafolios FF (recomendada)** | 25 portafolios 5×5 (Size × B/M) de Kenneth French | Estándar en la literatura FF, elimina sesgo de selección |
| **B — S&P 500 completo** | ~500 acciones del índice | Amplio y líquido |
| **C — Top 100 por capitalización** | Las 100 mayores por período | Manejable con criterio explícito |

> **Pendiente:** definir el universo final de activos antes del notebook 02.

---

## 6. Enfoque para el mercado chileno

**Pregunta:** ¿Existirá algún enfoque para el mercado chileno o es poco razonable?

**Respuesta:**

Es totalmente razonable y tiene precedente académico, pero hay que ser honesto sobre las limitaciones del mercado chileno.

---

### Lo que hace viable el enfoque

Existe literatura previa sobre modelos de factores en mercados latinoamericanos. No estás entrando en territorio inexplorado — hay papers sobre FF en Chile, Brasil, México y el MILA que puedes citar como antecedente.

El CMF publica datos financieros de todas las empresas que cotizan en bolsa (estados financieros, capitalización bursátil, ratios) que son necesarios para construir los factores HML, RMW y CMA.

---

### Las limitaciones reales

| Problema | Impacto |
|---|---|
| **Universo pequeño** | El IPSA tiene 30 acciones, la bolsa completa ~200 con baja liquidez. Los portafolios de factores quedan muy delgados |
| **Liquidez** | Muchas acciones chilenas tienen días sin transacciones — los retornos calculados son artificiales |
| **Historial corto** | Datos confiables desde ~1995–2000. FF necesita ciclos completos (crisis, recuperación) |
| **Concentración sectorial** | La bolsa chilena está dominada por retail, utilities y recursos naturales — poca diversidad para capturar todos los factores |
| **Factor momentum** | WML en Chile tiene evidencia débil comparado con EE.UU. |

---

### Los enfoques posibles (de menor a mayor complejidad)

**Opción 1 — Aplicar factores de EE.UU. a acciones chilenas**

Usas los factores FF6 de la librería de French y los regresas contra retornos de acciones chilenas. Esto responde la pregunta: *¿explican los factores globales los retornos locales?*
- Simple de implementar
- Sustento en la literatura de factores globales vs. locales

**Opción 2 — Construir factores FF locales (MILA)**

En lugar de usar solo Chile (~30 stocks), amplías el universo al MILA (Chile + Perú + Colombia + México, ~300 stocks). Esto da suficiente masa para construir portafolios SMB, HML, etc. con metodología French.
- Más robusto estadísticamente
- Responde una pregunta más relevante para la industria regional

**Opción 3 — Factores FF puramente chilenos**

Construyes SMB, HML, RMW, CMA y WML solo con acciones de la Bolsa de Santiago. Factible, pero los portafolios de factores tendrán 5–10 acciones cada uno.
- Alta varianza en los estimadores
- El aporte académico está en documentar qué tan bien funcionan en un mercado pequeño

---

### Recomendación concreta para la tesis

El enfoque más publicable y con mejor sustento sería:

> **Opción 1 como baseline + Opción 3 como análisis local**
>
> 1. Primero estimas si los factores globales de FF6 explican retornos chilenos → test de integración de mercados
> 2. Luego construyes factores locales con las acciones de la bolsa → test de primas locales
> 3. Comparas ambos: ¿cuál explica mejor? ¿hay un alpha residual?
>
> Esa comparación es en sí misma una contribución al conocimiento, especialmente para un mercado emergente como Chile.

---

### Fuentes de datos para Chile

| Fuente | Contenido | Acceso |
|---|---|---|
| Yahoo Finance (`.SN`) | Precios históricos IPSA | Gratuito vía yfinance |
| CMF (cmf.cl) | Estados financieros, capitalización bursátil | Gratuito, descarga manual |
| Bolsa de Santiago | Precios históricos completos | Convenio UDP |
| Economatica | Base estándar en academia latinoamericana | Probablemente vía UDP |

---

## Decisiones y pendientes al cierre de la sesión

- [x] Repositorio creado y configurado
- [x] README con estructura, autores y plan de 7 meses
- [x] Notebook 01 creado con comentarios detallados
- [x] Datos del período 200001–201501 guardados en Drive
- [ ] **Pendiente:** definir universo final de activos (portafolios FF vs. S&P 500 vs. top 100)
- [ ] **Pendiente:** explorar disponibilidad de acciones chilenas en Yahoo Finance (`.SN`)
- [ ] **Pendiente:** decidir enfoque para el mercado chileno (global vs. local vs. MILA)
- [ ] **Pendiente:** crear notebook 02 (regresiones FF6)
