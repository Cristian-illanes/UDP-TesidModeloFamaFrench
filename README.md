# Forecasting, Riesgo y Asset Allocation mediante el Modelo de Seis Factores de Fama-French

**Tesis de Magíster — Universidad Diego Portales**  
**Autores:** Álvaro Gatica, Fernanda Rojas, Luis Pizarro y Cristian Illanes  
**Programa:** Magíster en Finanzas  

---

## Idioma del Proyecto

Todo el código, comentarios, documentación, nombres de variables y notebooks de este proyecto están escritos en **español**. Esto incluye comentarios en el código, títulos de gráficos, nombres de columnas y archivos exportados.

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

---

## Plan de Desarrollo (7 meses · 24 semanas)

> Con 7 meses el framework completo es viable: FF6 + GARCH + VaR/ES + GMVP/MV/Max Sharpe + Black-Litterman + aplicación Chile. Sin recortes.

**Carga estimada:** ~7 hrs/semana · ~168 hrs mínimo · ~240 hrs a 10 hrs/semana

---

### Fase 1 — Fundamentos teóricos y datos `semanas 1–3` · ~21 hrs

| Semana | Foco | Actividades |
|--------|------|-------------|
| 1 | Setup + datos | Lectura: MOSEK Cap 2, Markowitz (1952). Código: descarga factores FF6 + retornos S&P 500 con yfinance, exploración inicial |
| 2 | Modelo FF6 — teoría | Lectura: Cajas Cap 4 (4.2.1), Fama & French (2015, 2018), Asness et al. (2013) — momentum. Código: estadísticas descriptivas de factores FF6 |
| 3 | Regresiones FF6 + covarianza factorial | Lectura: Cajas Cap 3, Ledoit & Wolf (2004), MOSEK Cap 5. Código: regresiones por acción, betas, alphas, matriz de covarianza factorial vs. muestral. Escritura: borrador Cap 2 |

**Entregable:** base de datos limpia + betas FF6 + matriz de covarianza factorial

---

### Fase 2 — Optimización de portafolios `semanas 4–7` · ~28 hrs

| Semana | Foco | Actividades |
|--------|------|-------------|
| 4 | GMVP y Media-Varianza | Lectura: MOSEK Cap 4, Cajas Cap 8 (8.2.1–8.2.2). Código: GMVP y frontera eficiente con CVXPY, benchmark 1/N |
| 5 | Max Sharpe + restricciones reales | Lectura: Cajas Cap 8 (8.2.4), Cap 9, DeMiguel et al. (2009). Código: Max Sharpe ratio, restricciones long-only y peso máximo |
| 6 | Backtesting out-of-sample | Lectura: Cajas Cap 11, MOSEK App 13. Código: rolling window out-of-sample, métricas Sharpe, Sortino, Drawdown, Turnover |
| 7 | Buffer + escritura | Ajustes al código, revisión de resultados. Escritura: borrador Cap 4 |

**Entregable:** 3 portafolios optimizados + tabla comparativa out-of-sample completa

---

### Fase 3 — Gestión de riesgo condicional `semanas 8–11` · ~28 hrs

| Semana | Foco | Actividades |
|--------|------|-------------|
| 8 | GARCH sobre residuos FF6 | Lectura: Danielsson Cap 3, Engle (1982), Bollerslev (1986). Código: GARCH(1,1) y GJR-GARCH sobre residuos FF6, selección AIC/BIC |
| 9 | VaR y Expected Shortfall dinámicos | Lectura: Danielsson Caps 4–6, MOSEK Cap 8 (8.2), Jorion. Código: VaR paramétrico + ES condicional GARCH vs. VaR histórico |
| 10 | Backtesting de modelos de riesgo | Lectura: Danielsson Cap 9, Hansen & Lunde (2005). Código: pruebas Kupiec y Christoffersen, VaR/ES integrado a portafolios |
| 11 | Buffer + escritura | Ajustes, revisión de resultados. Escritura: borrador Cap 3 |

**Entregable:** VaR y ES dinámico por portafolio + backtesting estadístico validado

---

### Fase 4 — Black-Litterman con views FF6 `semanas 12–15` · ~28 hrs

| Semana | Foco | Actividades |
|--------|------|-------------|
| 12 | Teoría BL clásico | Lectura: Black & Litterman (1992), Cajas Cap 5 (5.1 completo). Código: BL clásico con equilibrio de mercado y views simples |
| 13 | Views extraídas de FF6 | Lectura: Cajas Cap 5 (5.2 Augmented BL, 5.3 BL Bayes). Código: views a partir de factores FF6 (alpha, momentum, value), calibración de confianza |
| 14 | BL + optimización integrada | Código: portafolio BL-FF6 optimizado, comparación con GMVP/MV/Max Sharpe, backtesting out-of-sample |
| 15 | Buffer + escritura | Ajustes al modelo BL. Escritura: sección BL dentro del Cap 4 |

**Entregable:** portafolio BL-FF6 con views factoriales + comparación completa de 4 estrategias

---

### Fase 5 — Aplicación mercado chileno `semanas 16–19` · ~28 hrs

| Semana | Foco | Actividades |
|--------|------|-------------|
| 16 | Datos chilenos + construcción de factores | Código: descarga acciones S&P CLX / IPSA, construcción de factores FF locales (SMB, HML, WML) |
| 17 | Aplicar framework al mercado chileno | Código: regresiones FF6 locales, optimización GMVP y Max Sharpe, VaR y ES para portafolios chilenos |
| 18 | Comparación USA vs. Chile | Código: tabla comparativa de desempeño, riesgo y primas factoriales. Escritura: borrador Cap 5 |
| 19 | Buffer fase Chile | Holgura para construcción de factores locales (parte más incierta del proyecto). Escritura: ajustes Cap 5 |

**Entregable:** framework aplicado a Chile + comparación USA vs. mercado emergente

---

### Fase 6 — Escritura e integración final `semanas 20–24` · ~35 hrs

| Semana | Foco | Actividades |
|--------|------|-------------|
| 20 | Cap 1 — Introducción completa | Escritura: introducción, motivación, objetivos, hipótesis. Lectura: Ang (2014), Grinold & Kahn |
| 21 | Revisión Caps 2, 3 y 4 | Escritura: pulir y conectar capítulos centrales, consistencia de notación. Código: dashboard final con resultados integrados |
| 22 | Cap 6 — Conclusiones | Escritura: conclusiones, limitaciones, implicancias para la industria, trabajo futuro |
| 23 | Revisión completa + bibliografía | Escritura: revisión integral, formato, bibliografía. Código: documentar y subir repositorio reproducible |
| 24 | Buffer final y entrega | Holgura para imprevistos o correcciones del director. **Entrega final:** documento + código + presentación |

**Entregable final:** tesis completa (6 capítulos) + código reproducible + presentación

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
