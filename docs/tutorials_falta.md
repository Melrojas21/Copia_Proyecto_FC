# Tutorial de uso

> **Estado: ejecución Python y C++ documentada; pruebas de entorno pendientes.** Se incluyen comandos basados en `heat.cpp`. Los scripts complementarios y la configuración YAML aún no figuran en el repositorio.

## 1. Obtener e instalar

```bash
git clone https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional.git
cd Proyecto-fisica-computacional
python -m pip install numpy matplotlib
python heat.py
```

El repositorio no contiene un archivo de dependencias para Python ni una interfaz de línea de comandos para configurar los experimentos. Se necesita un entorno gráfico para ver `plt.show()`; cerrar la primera ventana permite avanzar a la segunda. Para ejecutar solo Python no se requiere C++.

## 2. Parámetros del experimento original

En `heat.py`, la sección de parámetros fija `Th=100`, `Tc=0`, `Tm=25`, `kappa=1`, `L=1`, `M=20`, `h=0.0001`, `t_end=15`. Las coordenadas se construyen con `np.linspace` y `np.meshgrid`. Los tiempos de salida son `[0.01, 1, 5, 15]`. Si se cambia `M` o `L`, las coordenadas globales y la condición de estabilidad deben mantenerse coherentes. Si se cambia `h` o `t_end`, revisar los tiempos de guardado dentro y fuera de la función.

## 3. Visualización

Cada escenario genera una figura de cuatro mapas de colores (`contourf`, 20 niveles, mapa `hot`). Los PNG ya presentes en el repositorio son [punto caliente](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/blob/main/grafica_punto_caliente_central.png) y [gradiente](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/blob/main/grafica_gradiente.png). El código no tiene `savefig`; para guardar, añadir `fig1.savefig(...)` o `fig2.savefig(...)` antes de `show`.

## 4. Comprobaciones complementarias

Tras integrar el script complementario del paquete completo anterior en la raíz (no incluido en esta entrega de docs):

```bash
python -m pip install numpy
python scripts/validar_calor.py
```

El script escribe `resultados/resumen_ftcs.csv` y `resultados/convergencia.csv`. Reproduce los escenarios con una versión independiente vectorizada, compara contra un bucle escalar de la fórmula por 100 pasos y verifica un campo constante. No importa ni modifica el `heat.py` original. Véase [resultados](resultados_falta.md).

## 5. C++ y OpenMP

Desde el commit [`50693a1`](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/commit/50693a1ccc0b00f6b8b4b81f16bafd447de81546) existen `heat.cpp` y `heat.exe`. Para compilar el código fuente en un equipo que tenga GCC con soporte OpenMP, desde la raíz del repositorio:

**Linux:**

```bash
g++ -O3 -std=c++17 -fopenmp heat.cpp -o heat
./heat
```

**Windows con MinGW-w64 y PowerShell:**

```powershell
g++ -O3 -std=c++17 -fopenmp heat.cpp -o heat.exe
.\heat.exe
```

El programa utiliza los parámetros definidos en `main()`, imprime las temperaturas centrales para los tiempos 0.01, 1, 5 y 15 y escribe `resultados_punto.csv` y `resultados_gradiente.csv` en la carpeta desde la que se ejecuta. Cada archivo contiene las columnas `Tiempo, j, i, Temperatura`. Se debe conservar una copia de los CSV publicados antes de ejecutar si se desea compararlos con nuevas salidas.

Para explorar diferentes cantidades de hilos, establecer la variable de entorno `OMP_NUM_THREADS` antes de iniciar el ejecutable. Por ejemplo, en PowerShell:

```powershell
$env:OMP_NUM_THREADS = '4'
.\heat.exe
```

En Linux:

```bash
OMP_NUM_THREADS=4 ./heat
```

Estos son comandos de compilación y ejecución propuestos a partir del código revisado; todavía no se ha documentado una prueba de compilación reproducible ni un benchmark en el equipo del grupo. Consultar [paralelismo](paralelismo_falta.md).

## 6. Página de documentación (opcional)

En el material documental anterior se propuso `mkdocs.yml`; Samuel está a cargo de la configuración definitiva y el archivo aún no aparece en el repositorio revisado. La propuesta sirve para convertir los Markdown en un sitio navegable:

```bash
python -m pip install mkdocs pymdown-extensions
mkdocs serve
```

Abrir la dirección local indicada por MkDocs. `mkdocs build` genera el sitio en `site/`. Las fórmulas usan MathJax cargado desde CDN, por lo que requieren conexión para renderizarse. Esta entrega solo incluye archivos Markdown; Samuel se encargará de configurar los recursos del sitio. No se ha publicado un sitio remoto.
