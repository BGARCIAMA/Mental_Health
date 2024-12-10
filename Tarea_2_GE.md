# Tarea Opción 2
# Taller de Geoespaciales

Yuneri Pérez Arellano 199813

Plantea **matemáticamente** la solución del siguiente problema:

1. Tienes $n$ puntos en $\R^2$ que representan las coordenadas de latitud y longitud.
2. Para el punto $x$, ¿cómo puedes determinar todos los puntos al menos de $r$ metros de manera rápida y eficiente? Al menos un filtro inicial para minimizar el número de operaciones.
3. Restricciones:
   1. No utilices *for, while, lapply()* como paso inicial; ningún tipo de ciclo.
   2. No consideres el uso de cómputo en la nube o máquinas virtuales con más capacidad.

Razona este problema matemáticamente como Maestro en Ciencia de Datos.

**Solución:**

**Contexto:**

Se nos presentan $n$ puntos en $\R^2$ representando coordenadas geográficas (*latitud, longitud*). Nuestro objetivo es encontrar de manera eficiente todos los puntos dentro de un radio $r$ metros respecto a un punto $x$, considerando las restricciones de no usar ciclos explícitos (*for, while, etc.*).

**Modelo Matemático**

Dado un conjunto de puntos ${(x_i = lat_i,lon_i | i=1,2, \dots n)}$ y un punto objetivo $x_0=(lat_0,lon_0)$, la distancia entre dos puntos en coordenadas geográficas se calcula mediante la fórmula del *haversine*:

$$
[
d(x_0, x_i) = 2*R \arcsin \left( \sqrt{\sin^2\left(\frac{\text{lat}_i - \text{lat}_0}{2}\right) + \cos(\text{lat}_0) \cos(\text{lat}_i) \sin^2\left(\frac{\text{lon}_i - \text{lon}_0}{2}\right)} \right)
]
$$

Donde:
- $R$: Radio de la Tierra ( 6,371 km).
- $\text{lat}$ y $\text{lon}$: Coordenadas en radianes.

Para encontrar puntos dentro del radio $r$, debemos filtrar aquellos que cumplen:

$$
d(x_0, x_i) \leq r
$$

#### **Optimización del Cálculo**

Para grandes cantidades de puntos, calcular $d(x_0, x_i)$ para todos los puntos es ineficiente. Para reducir el número de cálculos:
1. **División en Celdas**:
   - Dividimos el espacio en una cuadrícula con celdas de tamaño aproximado a $r$ metros.
   - Cada celda es identificada por un par de índices $(c_x, c_y)$, calculados como:
  $$
   c_x = \left\lfloor \frac{\text{lon}}{\text{cell\_size}} \right\rfloor, \quad c_y = \left\lfloor \frac{\text{lat}}{\text{cell\_size}} \right\rfloor
  $$

   Donde $\text{cell\_size}$ es el tamaño de la celda en grados, aproximado como:
  $$
   \text{cell\_size} = \frac{r}{111}
  $$

  $$(1^\circ \text{ de latitud} \approx 111 \text{ km})$$



1. **Filtro Inicial**:
   - Para el punto objetivo $x_0$, determinamos su celda $(c_{x_0}, c_{y_0})$.
   - Filtramos los puntos dentro de las celdas vecinas $(c_x, c_y)$ que cumplen:
  $$
   c_x \in [c_{x_0} - 1, c_{x_0} + 1], \quad c_y \in [c_{y_0} - 1, c_{y_0} + 1]
  $$

2. **Cálculo Exacto**:
   - Aplicamos la fórmula del *haversine* solo a los puntos filtrados para determinar los que realmente están dentro del radio $r$.

#### **Ventajas del Enfoque**

- **Reducción de Complejidad**:
  - Sin el filtro inicial, el cálculo exacto requiere $O(n)$ operaciones.
  - Con el filtro por celdas, solo evaluamos una fracción de los puntos, reduciendo significativamente el número de cálculos.


