# Ecuación de calor en dos dimensiones

> **Estado: documentación en actualización.** Python y C++ están publicados. Faltan pruebas comparativas completas, mediciones de rendimiento y la integración de MkDocs y scripts.

**Proyecto final de Física Computacional — FS-0432 | Universidad de Costa Rica**

Este proyecto estudia la evolución de la temperatura en una placa mediante la solución numérica de la ecuación de calor bidimensional. Se utiliza el método de diferencias finitas FTCS y se analizan dos escenarios térmicos mediante mapas de colores. La primera implementación se realizó en Python. Posteriormente, el 13 de noviembre de 2026, Eytan incorporó la versión C++ con directivas OpenMP, el ejecutable `heat.exe` y los archivos CSV de los dos escenarios. La presencia de paralelismo en el código está verificada; su aceleración todavía no se ha medido en un entorno documentado.

## Integrantes

| Nombre completo | Carné | Aporte / responsabilidad |
|---|---|---|
| Fernando Rodríguez Blanco | C26548 | Implementación en Python |
| Orlando Ismael Benavides Renault | C11025 | Documentación, README, reporte y bitácora |
| Samuel Rojas Baldi | C36852 | Configuración YAML (`.yml`), scripts y soporte de documentación web |
| Eytan Sequeira Mora | C07458 | Implementación en C++ y publicación de `heat.cpp`, `heat.exe` y dos CSV |

Los nombres, carnés y responsabilidades se registran según la información proporcionada por el grupo. Los cuatro carnés están registrados. Las responsabilidades son asignaciones de trabajo, no certificaciones de tareas terminadas. Los commits de la cuenta `aurodriguezb-sys` documentan cambios del repositorio, pero no se ha confirmado su correspondencia con un integrante.

**Profesor:** Marlon Brenes (autor del enunciado suministrado).

## Problema y objetivos

Resolver $\partial_t u=\kappa(\partial_{xx}u+\partial_{yy}u)$ en una región rectangular, estudiar el efecto de las condiciones iniciales y de frontera, visualizar la solución, implementar el método en Python y C++ y evaluar la aceleración en memoria compartida.

## Documentación

- [Inicio](docs/index_falta.md)
- [Fundamento físico y método numérico](docs/explanation.md)
- [Tutorial de ejecución](docs/tutorials_falta.md)
- [Referencia de código](docs/reference_falta.md)
- [Bitácora del proyecto](docs/bitacora_falta.md)
- [Resultados y validación](docs/resultados_falta.md)
- [Paralelismo y optimización](docs/paralelismo_falta.md)
- [Reporte académico](docs/reporte_falta.md)

## Ejecutar

Desde la raíz del repositorio original, con Python y las dependencias instaladas:

```bash
python -m pip install numpy matplotlib
python heat.py
```

El programa Python muestra dos figuras de Matplotlib; cierre la primera para continuar. El programa original no guarda automáticamente los PNG. El repositorio también contiene `heat.cpp`, que se puede compilar con GCC y OpenMP:

```bash
g++ -O3 -std=c++17 -fopenmp heat.cpp -o heat
./heat
```

En Windows se puede usar `-o heat.exe` y ejecutar `heat.exe` o `.\heat.exe`, según la terminal. La ejecución C++ escribe `resultados_punto.csv` y `resultados_gradiente.csv` en el directorio de trabajo. Para el script complementario de validación propuesto, consultar el [tutorial](docs/tutorials_falta.md): todavía no forma parte del repositorio público.

## Estado

| Hito | Avance |
|---|---|
| Método numérico y Python | Implementados |
| Condiciones iniciales distintas | Parcial: punto caliente e interior uniforme con fronteras asimétricas |
| Visualización temporal | Cuatro instantáneas por caso |
| C++ | `heat.cpp` y `heat.exe` publicados por Eytan el 13 de noviembre |
| Paralelismo | OpenMP presente en `heat.cpp`; falta medir aceleración y eficiencia |
| Documentación | Markdown en revisión; pendientes MkDocs y scripts |

Fuente: [repositorio del grupo](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional), rama `main`, revisado hasta el commit [`50693a1`](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/commit/50693a1ccc0b00f6b8b4b81f16bafd447de81546), de 13 de noviembre de 2026. Actualización documental: 16 de noviembre de 2026. Revisar los datos personales antes de publicar.

## Integración de esta entrega

Esta actualización modifica únicamente los archivos Markdown de documentación de Orlando. No sustituye ni altera `heat.py`, `heat.cpp`, `heat.exe`, los CSV ni las gráficas. Los archivos `_falta.md` conservan ese sufijo porque existen pendientes documentados. Cuando se completen las pruebas y la configuración, renombrarlos y actualizar los enlaces de navegación; `docs/index.md` es el nombre habitual de la portada en MkDocs. Mantener el `README.md` original hasta que el grupo apruebe el reemplazo.
