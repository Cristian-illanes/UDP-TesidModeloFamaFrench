# Forecasting, Riesgo y Asset Allocation mediante el Modelo de Seis Factores de Fama-French

**Tesis de Magíster — Universidad Diego Portales**  
**Autor:** VARIOS AUTORES  
**Programa:** Magíster en Finanzas  

---

## Resumen Ejecutivo

Esta investigación propone y evalúa un **framework cuantitativo integrado** para la construcción y gestión de portafolios de inversión, articulando tres dimensiones fundamentales: forecasting de retornos esperados, modelamiento condicional del riesgo y optimización avanzada de portafolios.

Como núcleo metodológico se emplea el **modelo de seis factores de Fama-French (FF6)** — que incorpora los factores de mercado, tamaño, valor, rentabilidad, inversión y momentum — para generar estimaciones disciplinadas de retornos esperados y estructuras de covarianza. Sobre estas estimaciones se construyen portafolios óptimos bajo múltiples criterios: mínima varianza global (GMVP), media-varianza (MV), máximo Sharpe y el enfoque Bayesiano de **Black-Litterman**.

La gestión del riesgo se aborda mediante **modelos condicionales de volatilidad** (GARCH, GJR-GARCH) aplicados sobre los residuos del modelo factorial, permitiendo estimaciones dinámicas del **Value-at-Risk (VaR) y Expected Shortfall (ES)**. El desempeño de todos los portafolios es evaluado **fuera de la muestra** bajo métricas de retorno ajustado por riesgo, con comparación sistemática contra benchmarks clásicos.

Finalmente, se presenta una **aplicación exploratoria al mercado accionario chileno**, examinando la transferibilidad del framework a un mercado emergente y su relevancia para la industria local de Asset Management.

---

## Estructura de la Tesis

```
Capítulo 1 — Introducción y Marco Conceptual
├── Motivación y relevancia
├── Objetivos generales y específicos
├── Hipótesis de investigación
└── Revisión de literatura

Capítulo 2 — Datos y Modelo Factorial
├── Descripción del universo de activos
├── Factores Fama-French: construcción y fuentes
├── Regresiones de series de tiempo
├── Estimación de betas y alphas
└── Matriz de covarianza factorial vs. muestral

Capítulo 3 — Forecasting y Riesgo Condicional
├── Modelos GARCH y GJR-GARCH sobre residuos FF6
├── VaR paramétrico condicional
├── Expected Shortfall (ES/CVaR)
├── Comparación VaR histórico vs. VaR condicional
└── Backtesting estadístico (Kupiec, Christoffersen)

Capítulo 4 — Optimización de Portafolios
├── Global Minimum Variance Portfolio (GMVP)
├── Media-Varianza (MV) — Frontera eficiente
├── Máximo Sharpe Ratio
├── Black-Litterman con views extraídas de FF6
├── Evaluación out-of-sample (rolling window)
└── Métricas: Sharpe, Sortino, Drawdown, Turnover

Capítulo 5 — Aplicación al Mercado Chileno
├── Construcción de factores FF locales (IPSA/S&P CLX)
├── Regresiones FF6 para acciones chilenas
├── Optimización y gestión de riesgo local
└── Comparación USA vs. Chile

Capítulo 6 — Conclusiones
├── Hallazgos principales
├── Implicancias para la industria de Asset Management
├── Limitaciones del estudio
└── Trabajo futuro: extensiones del framework
```

---

## Metodología

### Modelo de Seis Factores de Fama-French

$$R_i - R_f = \alpha_i + \beta_1 \cdot Mkt + \beta_2 \cdot SMB + \beta_3 \cdot HML + \beta_4 \cdot RMW + \beta_5 \cdot CMA + \beta_6 \cdot WML + \varepsilon_i$$

| Factor | Descripción |
|--------|-------------|
| **Mkt-RF** | Prima de riesgo del mercado |
| **SMB** | Small Minus Big — prima de tamaño |
| **HML** | High Minus Low — prima de valor |
| **RMW** | Robust Minus Weak — prima de rentabilidad |
| **CMA** | Conservative Minus Aggressive — prima de inversión |
| **WML** | Winners Minus Losers — prima de momentum |

### Criterios de Optimización

| Estrategia | Objetivo |
|------------|----------|
| **GMVP** | Minimizar varianza del portafolio |
| **MV** | Maximizar retorno para un nivel de riesgo dado |
| **Max Sharpe** | Maximizar retorno ajustado por riesgo |
| **Black-Litterman** | Combinar equilibrio de mercado con views factoriales |

### Métricas de Evaluación

- Sharpe Ratio, Sortino Ratio
- Value-at-Risk (VaR) al 95% y 99%
- Expected Shortfall (ES/CVaR)
- Maximum Drawdown
- Portfolio Turnover

---

## Estructura del Repositorio

```
UDP-TesidModeloFamaFrench/
│
├── README.md                    # Este archivo
│
├── notebooks/
│   ├── 01_datos_estadisticas.ipynb      # Descarga y exploración de datos
│   ├── 02_modelo_ff6_regresiones.ipynb  # Estimación del modelo factorial
│   ├── 03_covarianza_factorial.ipynb    # Construcción de la matriz de covarianza
│   ├── 04_garch_var_es.ipynb            # Modelos de volatilidad y riesgo
│   ├── 05_optimizacion_portafolios.ipynb # GMVP, MV, Max Sharpe
│   ├── 06_black_litterman.ipynb         # Modelo Black-Litterman con views FF6
│   ├── 07_backtesting.ipynb             # Evaluación out-of-sample
│   └── 08_aplicacion_chile.ipynb        # Aplicación mercado chileno
│
├── src/
│   ├── data/                    # Scripts de descarga de datos
│   ├── models/                  # Clases y funciones del modelo FF6
│   ├── optimization/            # Funciones de optimización (CVXPY)
│   └── risk/                    # Modelos GARCH, VaR, ES
│
├── data/
│   ├── raw/                     # Datos originales (no modificados)
│   └── processed/               # Datos procesados
│
├── results/
│   ├── figures/                 # Gráficos y visualizaciones
│   └── tables/                  # Tablas de resultados
│
├── docs/
│   ├── bibliografia.md          # Referencias bibliográficas
│   └── plan_estudio.md          # Plan de estudio y avance
│
└── requirements.txt             # Dependencias de Python
```

---

## Stack Tecnológico

| Librería | Uso |
|----------|-----|
| `pandas` | Manipulación de datos |
| `numpy` | Álgebra lineal y matrices |
| `pandas_datareader` | Descarga de factores FF6 |
| `yfinance` | Descarga de precios de acciones |
| `statsmodels` | Regresiones OLS y series de tiempo |
| `arch` | Modelos GARCH y GJR-GARCH |
| `cvxpy` | Optimización convexa (GMVP, MV, Max Sharpe) |
| `scipy` | Optimización y estadística |
| `matplotlib` / `seaborn` | Visualizaciones |
| `jupyter` | Entorno de desarrollo interactivo |

### Instalación

```bash
git clone https://github.com/Cristian-illanes/UDP-TesidModeloFamaFrench.git
cd UDP-TesidModeloFamaFrench
pip install -r requirements.txt
```

---

## Plan de Desarrollo (24 semanas)

| Fase | Semanas | Contenido |
|------|---------|-----------|
| **1 — Fundamentos y datos** | 1–3 | Setup, descarga FF6, regresiones, covarianza factorial |
| **2 — Optimización** | 4–7 | GMVP, MV, Max Sharpe, backtesting out-of-sample |
| **3 — Riesgo condicional** | 8–11 | GARCH, VaR, ES, backtesting estadístico |
| **4 — Black-Litterman** | 12–15 | BL clásico, views FF6, portafolio BL integrado |
| **5 — Mercado chileno** | 16–19 | Factores locales, aplicación IPSA, comparación USA |
| **6 — Escritura y entrega** | 20–24 | Redacción final, revisión, repositorio documentado |

---

## Referencias Principales

### Papers Fundacionales
- Fama, E. F., & French, K. R. (2015). A five-factor asset pricing model. *Journal of Financial Economics*, 116(1), 1–22.
- Fama, E. F., & French, K. R. (2018). Choosing factors. *Journal of Financial Economics*, 128(2), 234–252.
- Black, F., & Litterman, R. (1992). Global portfolio optimization. *Financial Analysts Journal*, 48(5), 28–43.
- Markowitz, H. (1952). Portfolio selection. *Journal of Finance*, 7(1), 77–91.
- Engle, R. F. (1982). Autoregressive conditional heteroscedasticity. *Econometrica*, 50(4), 987–1007.
- Bollerslev, T. (1986). Generalized autoregressive conditional heteroscedasticity. *Journal of Econometrics*, 31(3), 307–327.
- DeMiguel, V., Garlappi, L., & Uppal, R. (2009). Optimal versus naive diversification. *Review of Financial Studies*, 22(5), 1915–1953.

### Libros de Referencia
- Cajas, D. (2025). *Advanced Portfolio Optimization*. Springer.
- MOSEK ApS (2024). *MOSEK Portfolio Optimization Cookbook* (v1.6.0).
- Danielsson, J. (2011). *Financial Risk Forecasting*. Wiley.
- Ang, A. (2014). *Asset Management: A Systematic Approach to Factor Investing*. Oxford University Press.
- Grinold, R., & Kahn, R. (2000). *Active Portfolio Management*. McGraw-Hill.

---

## Estado del Proyecto

- [x] Definición del tema y estructura
- [x] Revisión bibliográfica inicial
- [ ] Descarga y exploración de datos
- [ ] Estimación modelo FF6
- [ ] Optimización de portafolios
- [ ] Modelos de riesgo condicional
- [ ] Black-Litterman
- [ ] Aplicación mercado chileno
- [ ] Redacción final

---

*Universidad Diego Portales — Magíster en Finanzas*
