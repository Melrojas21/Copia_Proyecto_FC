# Integrantes y estado del proyecto

> **Estado: en actualización.** Se documentan los aportes publicados y se separan de los encargos todavía pendientes.

**Universidad de Costa Rica — FS-0432 Física Computacional**

| Nombre completo | Carné | Aporte / responsabilidad |
|---|---|---|
| Fernando Rodríguez Blanco | C26548 | Implementación en Python |
| Orlando Ismael Benavides Renault | C11025 | Documentación, README, reporte y bitácora |
| Samuel Rojas Baldi | C36852 | Configuración YAML (`.yml`), scripts y recursos de MkDocs |
| Eytan Sequeira Mora | C07458 | Implementación en C++ y publicación de ejecutable y CSV |

Los nombres, carnés y responsabilidades se registran según la información proporcionada por el grupo. Los cuatro carnés están registrados. Las responsabilidades son asignaciones de trabajo, no certificaciones de tareas terminadas. Los commits de la cuenta `aurodriguezb-sys` documentan cambios del repositorio, pero no se ha confirmado su correspondencia con un integrante.

## Distribución de responsabilidades

| Integrante | Encargo comunicado | Evidencia y estado |
|---|---|---|
| Fernando Rodríguez Blanco | Python | `heat.py` y dos PNG existen en el repositorio revisado; confirmar cambios posteriores y validación del programa original. |
| Orlando Ismael Benavides Renault | Archivos Markdown, README, reporte y bitácora | Seis commits documentales publicados el 12 de noviembre; actualización de la documentación tras el aporte de Eytan. |
| Eytan Sequeira Mora | C++ | El commit [`50693a1`](https://github.com/aurodriguezb-sys/Proyecto-fisica-computacional/commit/50693a1ccc0b00f6b8b4b81f16bafd447de81546) incorpora `heat.cpp`, `heat.exe` y los dos CSV; código con OpenMP. |
| Samuel Rojas Baldi | YAML (`.yml`), scripts y recursos de documentación web | Responsabilidad asignada; `mkdocs.yml` y los scripts aún no constan en el repositorio. Aunque se habló de CSV como posible encargo, los dos CSV presentes se incorporaron en el commit de Eytan. |

## Estado del trabajo

Python y los mapas de temperatura están disponibles desde el 9 de noviembre. C++ y dos CSV están publicados desde el 13 de noviembre. El archivo `heat.cpp` contiene paralelismo OpenMP, pero todavía no existen en el repositorio mediciones suficientes para cuantificar la aceleración ni la eficiencia.

La documentación y el script independiente de validación descrito en el reporte se prepararon como aportes complementarios para revisión del grupo; dicho script no está integrado en el repositorio público. Véase el [historial](bitacora_falta.md) y el [reporte](reporte_falta.md).
