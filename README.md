# Laboratorio Interactivo de Prueba de Hipótesis

Herramienta visual e interactiva para explorar el contraste de hipótesis clásico sobre la media y la varianza
poblacional —con la media poblacional fijada discrecionalmente por quien la usa—, construida en HTML/JS puro,
sin dependencias de backend ni de librerías externas de graficación o de estadística.

Este laboratorio se construyó sobre la base del [Laboratorio de Intervalos de Confianza](../laboratorio-intervalos-de-confianza),
del cual conserva el motor de simulación, el sistema de diseño (tipografías, paleta y tema claro/oscuro) y la
mayor parte de los controles y paneles, reemplazando el bloque de intervalos y cobertura por el análisis de
contraste de hipótesis descripto más abajo.

## Demo

Abrí [`index.html`](./index.html) directamente en el navegador — no requiere servidor.

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
   rojo: se rechaza H₀).
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

## Implementación estadística

Todas las funciones están implementadas en JS puro, sin librerías externas de estadística:

- **Mulberry32 + transformación de Box-Muller** — generación de la población y de las muestras.
- **Función beta incompleta regularizada** (fracciones continuas) — valores críticos y valor p del contraste t.
- **Función gamma incompleta regularizada** (serie + fracción continua) — densidad, valores críticos y valor p
  del contraste χ².
- **Aproximación de Abramowitz & Stegun de la función error** — CDF normal estándar, usada únicamente para la
  potencia teórica de la matriz sistemática (aproximación con σ conocida).
- **Inversas numéricas** de t y χ² por búsqueda binaria — valores críticos para las regiones de rechazo.

## Dependencias externas

Ninguna. Los gráficos se dibujan directamente sobre `<canvas>` con la API 2D nativa del navegador; la única
carga externa es la tipografía (Google Fonts: Source Serif 4, Public Sans, IBM Plex Mono).

## Estructura del repositorio

```
.
├── index.html   # Aplicación completa (autocontenida)
└── README.md
```

## Nota conceptual

> s² **no** sigue una distribución χ². Lo que sigue una χ²(n−1) bajo H₀ es el estadístico de prueba
> T = (n−1)s²/σ₀². La relación exacta es s² ~ [σ₀²/(n−1)]·χ²(n−1). El gráfico muestra la distribución de T, no
> de s². A diferencia del contraste t —aproximadamente válido bajo el teorema central del límite aun con
> moderados apartamientos de la normalidad—, la validez del contraste χ² sobre la varianza se apoya de forma
> más estricta en la normalidad de la población subyacente.

## Referencias

- Stock, J. H., & Watson, M. W. (2020). *Introduction to econometrics* (4.ª ed. global). Pearson Education Limited.
- Wooldridge, J. M. (2025). *Introductory econometrics: A modern approach* (8.ª ed.). Cengage Learning.

## Licencia

MIT
