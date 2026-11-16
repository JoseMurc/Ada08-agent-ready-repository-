# Readiness Improvement Report

## AFTER

| Dimension | Score 0-5 | Weighted | Evidence | Main Gap |
|---|---:|---:|---|---|
| Entry Point & Navigation | 5 | 15/15 | docs/REPO_MAP.md ahora lista los puntos de entrada, las pruebas y una guía de cambio paso a paso. AGENTS.md trae un orden fijo de lectura (Read First) y check_repo_contract.py confirma que esos archivos siguen existiendo. | Ninguno relevante; la navegación quedó cubierta y verificada. |
| Agent Instructions | 5 | 15/15 | AGENTS.md incluye fuentes de verdad, comandos estándar, reglas de cambio, condiciones de alto y una Definition of Done concreta, todo en un solo archivo. El paso de validación que pide ejecutar (verify_repo.sh) ya corre limpio. | Ninguno relevante; la instrucción es clara y hoy está respaldada por un gate que pasa de verdad. |
| Build / Run / Test | 4 | 12/15 | Los comandos de instalación, ejecución y prueba siguen siendo exactos y reproducibles, y ahora hay un solo script (verify_repo.sh) que los agrupa junto con el lint, ya sin errores. | ruff no está declarado como dependencia del proyecto en pyproject.toml; solo lo instalan el entorno local o el workflow de CI a mano, así que el paso de lint no es reproducible desde un entorno limpio usando solo pyproject.toml. |
| Architecture Boundaries | 5 | 10/10 | docs/REPO_MAP.md agrega una sección explícita de "Do Not Edit Directly" y AGENTS.md suma condiciones de alto para cambios de arquitectura, reforzando lo que ya explicaba ARCHITECTURE.md. | Ninguno relevante; los límites quedaron explícitos y reforzados. |
| Requirements & Traceability | 4 | 8/10 | La matriz de trazabilidad sigue conectando cada requisito con sus reglas, tareas, código y pruebas mediante IDs consistentes. | La matriz y TASKS.md se siguen actualizando a mano; nada avisa si un requisito nuevo se queda sin registrar. |
| Automation & Guardrails | 5 | 10/10 | El flujo de CI (.github/workflows/verify.yml) corre pruebas, lint y el chequeo de contrato del repositorio en cada push o pull request, y ahora pasa limpio de punta a punta (72 pruebas, lint sin errores, contrato en PASS). | Ninguno relevante; el gate ya no es solo una promesa, corre y pasa. |
| Testability & Evidence | 5 | 15/15 | Hay 72 pruebas pasando para todas las capas, una prueba de latencia con resultados medidos, y ahora el CI las ejecuta automáticamente en cada cambio en vez de depender de que alguien las corra a mano. | Ninguno relevante; la evidencia automática de pruebas quedó cubierta. |
| Safety & Permissions | 4 | 8/10 | AGENTS.md ahora reúne reglas más claras: no tocar secretos ni archivos generados, no modificar el archivo de datos fuera de su tarea, y condiciones de alto ante una acción destructiva. | Esas reglas siguen siendo solo texto; ningún chequeo automático bloquea un cambio que las rompa, como sí ocurre con la navegación o las pruebas. |

TOTAL AFTER: 93 / 100

Mejora respecto al BEFORE (70/100): +23 puntos, impulsada sobre todo por la navegación, los límites de arquitectura, la evidencia de pruebas y, tras corregir los errores de lint, por un gate de automatización que ya corre limpio de punta a punta.

Top 3 remaining friction points:
1. ruff no está declarado como dependencia del proyecto en pyproject.toml, así que el paso de lint del script de verificación no es reproducible desde un entorno limpio sin depender de cómo lo instale cada persona o el workflow de CI.
2. Las reglas de seguridad (no tocar el archivo de datos fuera de su tarea, no tocar secretos o archivos generados) siguen siendo solo texto en AGENTS.md, sin ningún chequeo automático que las haga cumplir.
3. La trazabilidad de requisitos sigue dependiendo de que alguien actualice la matriz y TASKS.md a mano cada vez que se agrega un requisito nuevo.
