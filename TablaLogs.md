Confirmado: no hay ningún filtro booleano implementado. Las únicas menciones de "status"/"filter" en el código son el status de resultado (`OK`/`EMPTY_QUERY`/`NO_RESULTS`) y comentarios sobre que el repository no filtra. Además, REQUIREMENTS.md explícitamente dice (A-07) que los estados de cliente **no están modelados** — así que un filtro booleano por "activo/inactivo" no tiene campo de datos que lo soporte hoy.

Ahora te muestro los dos entregables que pediste.

## 1 Mapeo AI_USAGE_LOG.md (paso 23) ↔ pasos del ADA

El PDF pide mínimo 5 entradas; da 7 categorías temáticas. Cada una corresponde a una fase concreta del flujo del ADA:

| Entrada AI_USAGE_LOG | Paso(s) del ADA de donde sale | Qué registrar ahí |
|---|---|---|
| **Auditoría inicial** | Paso 7 (Repository Readiness Audit – BEFORE) + Paso 9 (Ejecución BEFORE) | Qué detectó el agente al explorar el repo sin pistas, qué asumió mal, y qué validaste tú del reporte `results/before-agent-run.md`. |
| **Discoverability / implicit knowledge** | Paso 9.1 (Identifica el conocimiento implícito → `results/implicit-knowledge.md`) | Qué información (ej. comando de test, carpetas prohibidas) tuvo que inferir el agente y cómo decidiste hacerla explícita o ejecutable. |
| **Diseño de AGENTS.md** | Paso 11 (Mejora 1 – AGENTS.md) y Paso 12 (tool-specific instructions) | Qué sugerencias del agente para `AGENTS.md`/`CLAUDE.md` aceptaste o rechazaste, y por qué. |
| **Guardrail** | Paso 15 (Mejora 5 – Rules, hooks y guardrails) + Paso 16 (`check_repo_contract.py`) | Qué regla crítica (de las detectadas en 9.1) convertiste en script/hook/CI en lugar de dejarla como texto. |
| **CI** | Paso 14.1 (CI como contrato ejecutable compartido) | Qué verificaciones terminaron compartidas entre humano, agente y pipeline (`verify_repo.sh` invocado también por `.github/workflows/verify.yml`), y por qué esas y no otras. |
| **Ejecución AFTER** | Paso 17 (Ejecuta nuevamente la misma tarea – AFTER) | Qué cambió realmente en el comportamiento del agente (menos preguntas, menos supuestos incorrectos, mejor evidencia) comparando `before-agent-run.md` vs `after-agent-run.md`. |
| **Evaluación final** | Paso 18 (Audit AFTER) + Paso 19 (Análisis de impacto / `readiness-improvement-report.md`) | Qué limitación del repo sigue requiriendo intervención humana pese a las mejoras (los "stop conditions" que no se pudieron automatizar). |

Nota: las entradas no son 1-a-1 con "pasos numerados" del documento sino con las *fases* del protocolo BEFORE→mejoras→AFTER. El PDF mismo (sección 23) ya da esta agrupación temática; lo que yo añadí es el amarre a la sección exacta del PDF que genera la evidencia de cada entrada.

## 2 Opciones para la tarea del paso 6.2 (TASK_UNDER_TEST.md)

Confirmado que ningún filtro existe aún, así que "agregar un filtro booleano" sigue siendo una tarea válida y no trivial (toca `search_service.py`, posiblemente `repository.py`/`models.py` y tests — no es texto/rename/doc). Pero antes de que decidas, aquí están las opciones evaluadas contra los criterios del PDF (debe requerir comprensión de contexto + cambio de código + validación verificable, y **no** debe redefinir requisitos existentes en SPEC/REQUIREMENTS):