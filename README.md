# Particles — Análisis de la función S(A_p, x)

Colección de notebooks de **Jupyter/Python** para definir, documentar y visualizar
la función de distribución de señal de partículas $S(A_p, x)$ en distintos
regímenes del parámetro $\beta$.

---

## Tabla de contenidos

1. [Descripción del proyecto](#descripción-del-proyecto)
2. [Fundamento físico-matemático](#fundamento-físico-matemático)
3. [Estructura del repositorio](#estructura-del-repositorio)
4. [Requisitos](#requisitos)
5. [Instalación](#instalación)
6. [Uso](#uso)
7. [Descripción detallada de cada notebook](#descripción-detallada-de-cada-notebook)
8. [Parámetros del modelo](#parámetros-del-modelo)
9. [Singularidades y dominio válido](#singularidades-y-dominio-válido)

---

## Descripción del proyecto

Este repositorio implementa y visualiza la función $S(A_p, x)$, que modela la
**distribución de señal (o flujo) de partículas** en función de:

- $x$ — variable cinemática (fracción de momento longitudinal, $0 < x < 1$).
- $A_p$ — parámetro de amplitud del proceso.

Los notebooks cubren:

- Evaluación numérica de $S$ en el régimen $\beta \ll 1$.
- Gráficas 2D de $S(x)$ con división por segmentos para esquivar singularidades.
- Superficie 3D de $S(A_p, x)$ sobre una malla bidimensional.

---

## Fundamento físico-matemático

### Función completa S(A_p, x)

$$S(A_p,\,x) \;=\;
\frac{\sqrt{\pi}}{2a}\,\Omega\,\bigl(A_p(1-x)-x\bigr)\,
\exp\!\left(-\frac{(1-x)^2}{\beta^2}\,
\frac{1+\dfrac{\beta}{2}\,\dfrac{A_p(1-x)-x}{(1-x)^3}}
{\dfrac{1+d}{1-x}+\dfrac{4n}{1-4x}}\right)
\left[x^2\!\left(\frac{1+d}{1-x}+\frac{4n}{1-4x}\right)\right]^{-1}$$

### Forma simplificada S(x)

Para estudiar la estructura de polos sin el amortiguamiento exponencial:

$$S(x) \;=\;
\frac{\dfrac{1+d}{1-x} + \dfrac{4n}{1-4x}}
{1 + \dfrac{\beta}{2(1-x)^2}\!\left(A_p - \dfrac{x}{1-x}\right)}$$

### Régimen β ≪ 1

El parámetro $\beta = 0.1$ selecciona el régimen de campo débil/suave, en el que
la corrección al exponente es pequeña y la distribución está dominada por el
factor de amplitud $A_p(1-x)-x$.

---

## Estructura del repositorio

```
particles/
├── Codigo1.ipynb    # Gráfico 2D por segmentos + Superficie 3D (notebook combinado)
├── Codigo2.ipynb    # Definición de S(Ap, x) y gráfico 2D
├── Codigo3.ipynb    # Copia de referencia de Codigo2.ipynb
├── Untitled.ipynb   # Gráfico 2D de S(x) por segmentos (forma simplificada)
├── belencilla.dat   # Datos de referencia
├── marco.txt        # Notas auxiliares
└── README.md        # Este archivo
```

---

## Requisitos

| Paquete | Versión mínima | Uso |
|---------|---------------|-----|
| Python  | 3.9+          | Lenguaje base |
| NumPy   | 1.21+         | Operaciones vectoriales |
| Matplotlib | 3.4+       | Visualización 2D y 3D |
| JupyterLab / Notebook | cualquiera | Ejecución interactiva |

---

## Instalación

```bash
# 1. Clonar el repositorio
git clone https://github.com/MikeCxC99/particles.git
cd particles

# 2. (Recomendado) Crear un entorno virtual
python -m venv .venv
source .venv/bin/activate        # Linux / macOS
# .venv\Scripts\activate         # Windows

# 3. Instalar dependencias
pip install numpy matplotlib jupyterlab
```

---

## Uso

```bash
# Lanzar JupyterLab
jupyter lab

# O la interfaz clásica de Jupyter Notebook
jupyter notebook
```

Abre cualquiera de los notebooks en el orden que prefieras.  El orden recomendado
para comprensión progresiva es:

1. **`Untitled.ipynb`** — visualización más simple de $S(x)$ por segmentos.
2. **`Codigo2.ipynb`** — función completa $S(A_p, x)$ y gráfico 2D.
3. **`Codigo1.ipynb`** — notebook combinado con gráfico 2D y superficie 3D.
4. **`Codigo3.ipynb`** — copia de `Codigo2.ipynb` para exploración independiente.

---

## Descripción detallada de cada notebook

### `Codigo1.ipynb` — Gráfico 2D + Superficie 3D

El notebook más completo.  Contiene **dos bloques** en celdas separadas:

| Bloque | Descripción |
|:------:|:------------|
| **1** | Gráfico 2D de $S(x)$ con `Ap = 0.8` fijo, usando tres segmentos para rodear las singularidades en $x=0.25$ y $x \approx 0.711$. |
| **2** | Superficie 3D de $S(A_p, x)$ sobre una malla NumPy, con proyecciones `contourf` en las paredes del gráfico. |

**Función clave:** `S(Ap, x)` — forma completa con factor exponencial.

---

### `Codigo2.ipynb` — Definición canónica de S(A_p, x)

Define la función $S(A_p, x)$ con docstring completo y la evalúa variando $x$
de 0.1 a 0.99 para un conjunto de valores de $A_p$ en paralelo.

**Función clave:** `S(Ap, x)` — forma completa.  
**Salida:** gráfico 2D de $x$ vs $S$.

---

### `Codigo3.ipynb` — Copia de referencia

Notebook idéntico a `Codigo2.ipynb`.  Útil para experimentar con parámetros
distintos sin alterar la versión canónica.

---

### `Untitled.ipynb` — S(x) por segmentos (forma simplificada)

Visualiza la **forma simplificada** $S(x)$ (cociente polinomial con corrección
de primer orden en $\beta$) dividiendo el dominio en tres segmentos:

| Segmento | Rango | Descripción |
|:--------:|:-----:|:------------|
| `xa` | $[0.10,\; 0.25]$ | Antes de la singularidad $x=0.25$ |
| `xb` | $[0.251,\; 0.711]$ | Entre las dos singularidades |
| `xc` | $[0.712,\; 0.99]$ | Después de la singularidad paramétrica |

Las líneas verticales punteadas marcan las singularidades en el gráfico.

**Función clave:** `S_segmento(x)` — forma simplificada con parámetros globales.

---

## Parámetros del modelo

| Variable | Símbolo | Valor por defecto | Descripción |
|----------|:-------:|:-----------------:|-------------|
| `a`      | $a$        | 1   | Constante de normalización |
| `O`      | $\Omega$   | 1   | Factor multiplicativo global |
| `d`      | $d$        | 10  | Parámetro de estructura |
| `n`      | $n$        | 0.5 | Índice de la distribución |
| `B`      | $\beta$    | 0.1 | Parámetro del régimen ($\beta \ll 1$) |
| `Ap`     | $A_p$      | 0.8 | Parámetro de amplitud (variable en gráficas 3D) |

---

## Singularidades y dominio válido

La función $S(A_p, x)$ presenta singularidades que deben evitarse al elegir
el dominio de evaluación:

| Punto singular | Causa matemática | Estrategia |
|:--------------:|:----------------:|:----------:|
| $x = 0.25$ | $1-4x = 0$ → divergencia en denominador | Segmentar dominio |
| $x \approx 0.711$ | El denominador de la corrección $\beta$ se anula | Segmentar dominio |
| $x = 1$ | $1-x = 0$ → todos los factores divergen | Usar $x_{\max} = 0.99$ |

El rango seguro recomendado es $x \in [0.1,\; 0.99]$ con exclusión de los
puntos singulares.
