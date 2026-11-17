# Referencia del código

> **Estado: Python y C++ documentados; scripts externos pendientes.** La referencia incorpora `heat.cpp` y sus salidas CSV, pero falta validar su interfaz mediante pruebas automatizadas y añadir los scripts de Samuel.

Fuentes: [heat.py](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/blob/main/heat.py) y [heat.cpp](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/blob/main/heat.cpp). La incorporación de C++ corresponde al commit [`50693a1`](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/commit/50693a1ccc0b00f6b8b4b81f16bafd447de81546).

## `heat_ftcs_2D(Th, Tc, Tm, kappa, L, M, h, t_end, initial='punto', T_hot=100.0)`

| Parámetro | Significado |
|---|---|
| `Th`, `Tc` | Temperatura del borde caliente/frío en `gradiente` |
| `Tm` | Temperatura base y de bordes restantes |
| `kappa` | Difusividad térmica |
| `L` | Lado de la placa cuadrada |
| `M` | Número de intervalos por eje |
| `h` | Paso temporal |
| `t_end` | Tiempo final solicitado |
| `initial` | `punto` o `gradiente` |
| `T_hot` | Temperatura del nodo central, por defecto 100 |

**Retorno:** `T_res`, arreglo NumPy de forma `(M+1, M+1, 4)`, con índices espaciales `[j,i]` e índice temporal `k`. Los tiempos son fijos: 0.01, 1, 5, 15. `its=round(t_end/h)`; se guardan estados cuando el índice temporal coincide con `round(tiempo/h)`.

**Dependencias:** NumPy; Matplotlib para gráficas fuera de la función. En `punto`, la función usa las coordenadas globales `x`, `y`, no las calcula internamente. La importación de `heat.py` ejecuta ambos experimentos porque no tiene guardia `if __name__ == '__main__':`.

## Arreglos

| Objeto | Uso |
|---|---|
| `T` | Estado anterior y fronteras |
| `Tp` | Estado nuevo; conserva bordes |
| `T_res` | Instantáneas |
| `x,y,X,Y` | Coordenadas y malla de visualización |

## Limitaciones y mejoras propuestas

- Validar `M`, `L`, `h`, `kappa`, `initial` y estabilidad antes de iterar.
- Construir las coordenadas dentro de la función, recibir tiempos de salida y validar que sean alcanzables; si no, quedan instantáneas de ceros sin advertencia.
- Separar simulación de visualización y usar una guardia de ejecución para permitir importar funciones.
- Emplear intercambio de búferes en lugar de copiar toda la matriz cada paso; comparar resultados.
- Documentar unidades, tratamiento de esquinas y fronteras. Fijar escalas de color comparables para mostrar la evolución.
- Valorar clase de configuración o simulador si mejora modularidad; no afirmar que ya se usa OOP.

## Código complementario propuesto

`../scripts/validar_calor.py`: `inicializar(M, caso)` prepara los escenarios; `evolucionar(T, kappa, dx, dy, dt, pasos)` ejecuta FTCS vectorizado con verificación de estabilidad y fronteras fijas; `main()` genera CSV y ejecuta pruebas. Es una herramienta independiente de validación, no un reemplazo de `heat.py`.

## Función C++ `heat_ftcs_2D`

```cpp
std::vector<std::vector<std::vector<double>>> heat_ftcs_2D(
    double Th, double Tc, double Tm, double kappa, double L,
    int M, double h, double t_end, const std::string& initial,
    const std::vector<double>& x, std::vector<double>& y,
    double T_hot = 100.0);
```

| Parámetro | Significado y tipo |
|---|---|
| `Th`, `Tc`, `Tm` | Temperaturas de frontera y de referencia; `double`. |
| `kappa` | Difusividad térmica; `double`. |
| `L` | Lado de la placa cuadrada; `double`. |
| `M` | Intervalos espaciales por eje; `int`. |
| `h`, `t_end` | Incremento temporal y tiempo final solicitado; `double`. |
| `initial` | Escenario `punto` o `gradiente`; referencia constante a cadena. |
| `x`, `y` | Coordenadas de la malla; referencias a vectores de `double`. El código no modifica `y`, aunque su parámetro no está declarado `const`. |
| `T_hot` | Temperatura inicial del punto central; `double`, valor por defecto 100. |

**Retorno:** vector tridimensional `T_res[j][i][k]` de dimensiones `(M+1) × (M+1) × 4`, con cuatro instantáneas para los tiempos 0.01, 1, 5 y 15. Las matrices `T` y `Tp` se representan con vectores anidados. El método utiliza diferencias finitas FTCS y dos regiones paralelas OpenMP, una para actualizar los nodos interiores y otra para almacenar estados.

## Programa C++ `main()` y archivos generados

El programa establece los parámetros `Th=100`, `Tc=0`, `Tm=25`, `kappa=1`, `L=1`, `M=20`, `h=0.0001` y `t_end=15`, genera coordenadas uniformes y ejecuta ambos casos. Imprime en consola la temperatura central para cuatro instantes y guarda:

- `resultados_punto.csv`: columnas `Tiempo, j, i, Temperatura`.
- `resultados_gradiente.csv`: las mismas columnas para el segundo escenario.

Cada CSV tiene cuatro bloques de 441 nodos, uno por instante, además de la fila de cabecera. `heat.exe` es el ejecutable incluido en GitHub; no se ha comprobado de manera independiente su ejecución en otro entorno. El código no acepta parámetros por línea de comandos y, al igual que el Python original, no valida todavía la condición de estabilidad antes de iterar.

## Comparación y limitaciones pendientes

Las dos implementaciones usan la misma fórmula FTCS y los mismos parámetros nominales. En C++ se copia la matriz completa mediante `T = Tp` en cada paso; en Python se utiliza `T[:, :] = Tp[:, :]`. Queda por implementar una comparación automatizada nodo a nodo y medir la aceleración de OpenMP. También faltan validaciones de dimensiones, del valor de `initial`, de los tiempos solicitados y de la estabilidad numérica. Los scripts mencionados en propuestas previas, como `scripts/validar_calor.py`, no figuran todavía en el repositorio principal.
