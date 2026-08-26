# Laboratorio Interactivo de Prueba de Hipótesis

Herramienta visual e interactiva para explorar el contraste de hipótesis clásico sobre la media y la varianza
poblacional —con la media poblacional fijada discrecionalmente por quien la usa—, construida en HTML/JS puro,
sin dependencias de backend ni de librerías externas de graficación o de estadística.

Este laboratorio se construyó sobre la base del [Laboratorio de Intervalos de Confianza](https://github.com/fcontiggiani/Laboratorio-de-Prueba-de-Hipotesis),
del cual conserva el motor de simulación, el sistema de diseño (tipografías, paleta y tema claro/oscuro) y la
mayor parte de los controles y paneles, reemplazando el bloque de intervalos y cobertura por el análisis de
contraste de hipótesis descripto más abajo.

## Demo

Abrí el [`demo`](https://fcontiggiani.github.io/Laboratorio-Interactivo-de-Prueba-de-Hipotesis/index.html) directamente en el navegador — no requiere servidor.

## ¿Qué hace?

El laboratorio:

1. **Genera una población** de N = 200.000 observaciones, con forma configurable (normal, uniforme, exponencial
   o bimodal) y **media poblacional μ fijada discrecionalmente** por quien lo usa: cada forma conserva su
   dispersión y asimetría características, pero se traslada para que su media efectiva coincida con el valor
   especificado.
2. **Extrae muestras aleatorias simples con reposición** de tamaño n ∈ {10, 100, 1.000, 10.000} del mismo
   proceso generativo.
3. **Ejecuta dos contrastes de hipótesis** por muestra, en paralelo, con el **tipo de contraste seleccionable**
   entre bilateral y unilateral (cola derecha o cola izquierda), aplicado simultáneamente a ambas pruebas:
   - **Contraste t sobre la media** — H₀: μ = μ₀ frente a H₁: μ ≠ μ₀ (bilateral), H₁: μ > μ₀ (cola derecha) o
     H₁: μ < μ₀ (cola izquierda) — estadístico t = (x̄ − μ₀)/(s/√n) ~ t(n−1) bajo H₀.
   - **Contraste χ² sobre la varianza** — H₀: σ² = σ₀² frente a H₁: σ² ≠ σ₀² (bilateral), H₁: σ² > σ₀² (cola
     derecha) o H₁: σ² < σ₀² (cola izquierda) — estadístico T = (n−1)s²/σ₀² ~ χ²(n−1) bajo H₀.
4. **Visualiza**, para cada contraste, la distribución teórica bajo H₀, la región de rechazo correspondiente al
   tipo de contraste y al nivel de significancia α elegidos (una región bilateral repartida en ambas colas, o
   una única región concentrada en la cola indicada por H₁), y —una vez ejecutadas las réplicas— la proyección
   de cada estadístico muestral sobre esa distribución, coloreada según la decisión (verde: no se rechaza H₀;
   rojo: se rechaza H₀). Tanto en el panel de la media como en el de la varianza, la distribución teórica se
   traza en dos versiones superpuestas: la distribución bajo H₀ (trazo principal, sólido, coincidente por
   construcción con los límites de la región de rechazo) y, como referencia, la derivada de los datos
   poblacionales efectivos (trazo punteado). En el panel de la media, la primera es una t de Student con
   g.l. = n−1 centrada en μ₀, y la segunda la aproximación N(μ, σ²/n) del teorema central del límite. En el
   panel de la varianza, la primera es una χ²(n−1) sin escalar, y la segunda esa misma χ²(n−1) escalada por el
   factor σ²/σ₀² (pues T = (n−1)s²/σ₀² = c·χ²(n−1) con c = σ²/σ₀², dado (n−1)s²/σ² ~ χ²(n−1) bajo normalidad
   poblacional). En ambos paneles, las dos curvas convergen visualmente cuando H₀ es verdadera (μ₀ = μ, o
   σ₀² = σ²) o cuando n es grande, y se separan claramente cuando H₀ es falsa o n es pequeño.
5. **Reporta tasas de rechazo empíricas** por tamaño de muestra: si μ₀ (o σ₀²) coincide con el valor poblacional
   efectivo, la tasa observada bajo repetición del muestreo estima el error de tipo I y debería aproximarse a α;
   si no coincide, estima la potencia del contraste frente a esa alternativa específica.
6. **Compara sistemáticamente**, en una matriz n × α, la potencia teórica del contraste t (aproximación normal
   con σ poblacional conocida), y superpone las distribuciones muestrales de X̄ de los cuatro tamaños de muestra
   sobre un soporte común mediante estimación de densidad por núcleos (KDE).
7. **Detalla las réplicas individuales**, cuando el número de réplicas R se fija en 10: se despliegan una tabla
   con el resultado de ambos contrastes —media, varianza muestral, estadístico t, valor p y dictamen sobre H₀
   para la media; estadístico χ², valor p y dictamen sobre H₀ para la varianza— en cada una de las 10 muestras
   simuladas, y un panel de detalle con el histograma de la muestra seleccionada (al hacer clic sobre la fila
   correspondiente), señalando la media muestral y μ₀ sobre la distribución empírica de las observaciones
   individuales.

## Parámetros configurables

| Parámetro | Descripción | Default |
|-----------|-------------|---------|
| Forma poblacional | Normal, uniforme, exponencial o bimodal | Normal |
| μ | Media poblacional (discrecional) | 50 |
| Semilla | Semilla del generador pseudoaleatorio (mulberry32) | 20260826 |
| n | Tamaño de cada muestra | 100 |
| μ₀ | Media bajo H₀ (contraste t) | 50 |
| σ₀² | Varianza bajo H₀ (contraste χ²) | 100 |
| Tipo de contraste | Bilateral (≠), cola derecha (>) o cola izquierda (<); común a los dos contrastes | Bilateral |
| α | Nivel de significancia | 0,05 |
| R | Número de réplicas independientes para la simulación (con R = 10 se habilita la tabla de detalle muestra a muestra) | 500 |

## Experimentos sugeridos

- **H₀ verdadera (error de tipo I)**: dejá μ₀ = μ (y σ₀² = σ² efectiva, visible en el panel «Población»). La
  tasa de rechazo empírica debería rondar α.
- **H₀ falsa en la media (potencia)**: fijá μ, luego cambiá μ₀ a μ ± 1σ/√n aproximadamente. Observá cómo la tasa
  de rechazo empírica —y la potencia teórica de la matriz sistemática— se aleja de α.
- **Efecto del tamaño de muestra sobre la potencia**: con μ₀ ≠ μ fijo, aumentá n → la distribución muestral se
  angosta, la región de no rechazo se reduce en términos absolutos y la tasa de rechazo (potencia) sube.
- **Control del nivel α**: reducí α a 0,01 → las regiones de rechazo se achican y caen menos réplicas en ellas,
  a costa de una menor potencia frente a alternativas cercanas.
- **Sensibilidad del contraste χ² a la no normalidad**: repetí el experimento de error de tipo I sobre σ² con
  la población exponencial o bimodal en lugar de la normal, y compará la tasa de rechazo empírica resultante
  con α.
- **Bilateral vs. unilateral**: con μ₀ apenas por debajo de μ, compará la tasa de rechazo del contraste
  bilateral con la del contraste de cola derecha (H₁: μ > μ₀) al mismo α: el unilateral, al concentrar toda el
  área de rechazo en la cola relevante, detecta esa alternativa con mayor potencia. Si en cambio μ₀ está por
  encima de μ, repetí el ejercicio con cola izquierda (H₁: μ < μ₀).
- **Inspección muestra a muestra**: fijá R = 10 y ejecutá «Ejecutar R réplicas» (o «Comparar los 4 tamaños»)
  para desplegar la tabla de detalle; haciendo clic en cada fila se observa cómo, a igualdad de μ y μ₀, la
  variabilidad muestral produce estadísticos t y valores p distintos —y, ocasionalmente, dictámenes distintos—
  entre réplicas generadas por el mismo proceso poblacional.
- **Distribución bajo H₀ vs. distribución con los datos poblacionales (media)**: con n pequeño (por ejemplo,
  n = 10) y μ₀ desplazada respecto de μ, observá cómo la curva sólida (t, g.l. = n−1, centrada en μ₀) y la curva
  punteada (aprox. normal, centrada en la μ efectiva) se separan claramente, y cómo los límites de la región de
  rechazo coinciden exactamente con las colas de la curva sólida —no con las de la punteada—, dado que la
  región de rechazo se construye bajo el supuesto de que H₀ es verdadera. Aumentá n y notá cómo ambas curvas
  convergen progresivamente (t<sub>n−1</sub> → N(0,1) estandarizada) cuando μ₀ = μ.
- **Distribución bajo H₀ vs. distribución con los datos poblacionales (varianza)**: fijá σ₀² bien por encima o
  por debajo de la σ² efectiva (visible en el panel «Población») y observá cómo la curva punteada —la χ²(n−1)
  escalada por σ²/σ₀²— se desplaza respecto de la curva sólida (χ²(n−1) sin escalar, bajo H₀), mientras que la
  región de rechazo sigue demarcada por esta última. Con σ₀² muy por debajo de σ², casi todas las réplicas caen
  en la cola derecha de rechazo; con σ₀² muy por encima, en la izquierda —ilustrando de forma directa por qué
  la potencia del contraste χ² depende de esa distancia relativa (σ²/σ₀²), no de su diferencia absoluta.

## Implementación estadística

Todas las funciones están implementadas en JS puro, sin librerías externas de estadística:

- **Mulberry32 + transformación de Box-Muller** — generación de la población y de las muestras.
- **Densidad t de Student** (a partir de la función gamma vía `gammln`) — curva de la distribución muestral
  teórica bajo H₀ en el panel de la media, con g.l. = n−1.
- **Función beta incompleta regularizada** (fracciones continuas) — valores críticos y valor p del contraste t.
- **Función gamma incompleta regularizada** (serie + fracción continua) — densidad, valores críticos y valor p
  del contraste χ².
- **Aproximación de Abramowitz & Stegun de la función error** — CDF normal estándar, usada únicamente para la
  potencia teórica de la matriz sistemática (aproximación con σ conocida).
- **Inversas numéricas** de t y χ² por búsqueda binaria — valores críticos para las regiones de rechazo.

## Dependencias externas

Ninguna. Los gráficos se dibujan directamente sobre `<canvas>` con la API 2D nativa del navegador; la única
carga externa es la tipografía (Google Fonts: Source Serif 4, Public Sans, IBM Plex Mono).


## Nota conceptual

> s² **no** sigue una distribución χ². Lo que sigue una χ²(n−1) bajo H₀ es el estadístico de prueba
> T = (n−1)s²/σ₀². La relación exacta es s² ~ [σ₀²/(n−1)]·χ²(n−1). El gráfico muestra la distribución de T, no
> de s². A diferencia del contraste t —aproximadamente válido bajo el teorema central del límite aun con
> moderados apartamientos de la normalidad—, la validez del contraste χ² sobre la varianza se apoya de forma
> más estricta en la normalidad de la población subyacente. Cuando σ₀² ≠ σ² (H₀ falsa), T ya no es χ²(n−1): es
> (σ²/σ₀²)·χ²(n−1), una χ² escalada —la curva punteada del panel muestra precisamente esta distribución.

## Referencias

- Stock, J. H., & Watson, M. W. (2020). *Introduction to econometrics* (4.ª ed. global). Pearson Education Limited.
- Wooldridge, J. M. (2025). *Introductory econometrics: A modern approach* (8.ª ed.). Cengage Learning.

## Licencia

MIT
