# AI Usage Log — GitHub MCP Integration (ADA-08)

Entradas correspondientes a la herramienta de IA usada en este ADA: **Claude Sonnet 5 / Claude Code**. Ajustar fechas si el trabajo se retomó en días distintos.

## Entry 1

**Tool:** Claude Sonnet 5

**Date:** 04/11/2026

**Stage:** Auditoria Inicial

**Prompt:**

    Haz una auditoria rapida de usando el documento de @ada-05-spec-driven-feature/REPOSITORY_READINESS_AUDIT.md  , donde cada dimension se mide en una escala de 0 a 5, para determinar los puntos por dimension el score de 0-5 se divide entre 5 y se multiplica por el peso de la dimension.

    Lo que se evalua de cada dimension esta en el paso 7 del documento @ADA-08_AgentReady_Repository.pdf :
        1. Entry Point & Navigation - El agente puede descubrir propósito, estructura, archivos relevantes, comandos, 
        tests y fuentes de verdad sin depender de 
        instrucciones manuales.  
        2. Agent Instructions -  AGENTS.md define reglas del proyecto, source of truth, comandos, límites y Definition of Done. 
        3. Build / Run / Test - Comandos reproducibles para instalar, ejecutar, probar y validar. 
        4. Architecture Boundaries - Componentes, responsabilidades y  dependencias están explícitas; el agente sabe dónde NO modificar. 
        5. Requirements & Traceability - Requirements/spec/tasks se conectan por IDs o referencias y evitan duplicación/conflictos. 
        6. Automation & Guardrails - Scripts, lint, hooks o CI convierten reglas importantes en verificaciones automáticas y reutilizables por humanos y agentes. 
        7. Testability & Evidence - Hay pruebas relevantes, criterios verificables y una ruta clara para demostrar que el cambio funciona. 
        8. Safety & Permissions - Secretos, archivos sensibles, operaciones destructivas y acciones externas tienen límites explícitos. 

    La escala de puntuacion tambien se especifica en @ADA-08_AgentReady_Repository.pdf en el paso 8:
    - 0 Ausente. El agente debe inferirlo. 
    - 1 Existe de forma parcial, obsoleta o ambigua. 
    - 2 Útil solo con conocimiento previo del proyecto. 
    - 3 Suficiente para un humano, pero genera fricción para un  agente. 
    - 4 Claro, verificable y normalmente suficiente para trabajo agentic. 
    - 5 Explícito, reproducible, seguro y validado por 
    automatización/evidencia.
    
    Llena el @ada-05-spec-driven-feature/REPOSITORY_READINESS_AUDIT.md  sin demasiados tecnisismos

**AI contribution:** El agente hizo la auditoria inicial preliminar, dando argumentos de las decisiones preliminares y dejando los puntos que se consideraron, asi como los puntos de friccion principales. En el documento se destaco que ya se cubria de buena manera las dimensiones pero quedaban expuestas a desincronizacion de cambios por la falta de automatizacion, por loq ue la "calificacion" fue 70/100.

**Student decision:** Se acepto la auditorio despues de una revision manual a varios de los puntos, se hicieron cambios a la redaccion y se cambio la puntuacion de Automation & Guardrails de 2/5 a 1/5 debido a que solo existe una configuracion minima de pytest y comandos, pero no hay lints, integracion continua, hooks ni mucho menos algun tipo de automatizacion.

**Impact:** `REPOSITORY_READINESS_AUDIT.md` creado.

---

## Entry 2

**Tool:** Claude Sonnet 5

**Date:** 04/11/2026

**Stage:** Ejecucion de Before

**Prompt:**

    Implement the change described in @ada-05-spec-driven-feature/TASK_UNDER_TEST.md  
 
    Before making any changes: 
    - inspect the repository, 
    - identify the relevant project context, 
    - determine how the project is built, tested, and validated, 
    - respect the existing architecture and repository conventions. 
    
    When finished: 
    - run the appropriate validation, 
    - report the files modified, 
    - report the commands executed, 
    - provide evidence that the requested change works. 
    
    Do not ask for additional repository context unless it is necessary to proceed safely.

**AI contribution:** El agente implemento el cambio de `TASK_UNDER_TEST.md` que fue implementar un filtro para emails en el buscador, modificó `traceability.md` y `TASKS.md` en la parte de la documentacion y en la parte de codigo modifico `cli.py`, `search_service.py`, `session.py` y varios tests:

- `conftest.py`
- `test_clip.py`
- `test_latency.py`
- `test_models.py`
- `test_repository.py`
- `test_search_service.py`
- `test_session.py`

**Student decision:** Se acpetaron dichos cambios al sistema, posteriormente se subieron estos cambios a la rama de evidence/before-run (que es una derivada de la rama de baseline ada08-baseline) en el repositorio del ada05-spec-driven-feature (que es donde el proyecto "raiz" esta), con el fin de guardar los cambios hechos en el BEFORE. Se tomaron en cuenta varias de las dificultados que tuvo el agente al implementar como el no saber exactamente donde estaba "parado", lo que hizo que usara comandos extraños y el hecho de que el repo local esta en una carpeta anidada que le parece provocar problemas al agente.

**Impact:** Se creo la rama evidence/before-run en el repositorio del ada05 con los cambios antes mencionados.

---


## Entry 3

**Tool:** Claude Sonnet 5

**Date:** 04/11/2026

**Stage:** Discoverability / implicit knowledge

**Prompt:**

    No hubo un prompt como tal ya que lo que se añadio a `IMPLICIT_KNOWLEDGE.md` se puso en base a las acciones que tomo el agente en la entrada 2 (ejecucion del before).

**AI contribution:** El agente si bien realizo bien la implementacion y esta funciona, hubo ciertos problemas que resaltaron por sus acciones, por ejemplo:
- No sabe bien donde inicia ya que esta en la carpeta ada-05-spec-driven-feature y el repo esta anidada en otra carpeta del mismo nombre (cosa que se puede solucionar yu se anoto como conocimiento implicito pero igual depende de donde se abra el proyecto).
- Parece tomar como referencia primero `pyproject.toml` y no `README.md` para ejecutar las pruebas, lo que hace que use un comando diferente.
- El agente busca referencias a los FR, AC, SR y VC junto con "domain" y "dominio" en todo el sistema debido a que ningun documento ha documentado el tasks (`TASKS.md` no se actualizo en el prompt de la implementacion por su falta de automatizacion que se habia reportado en la auditoria).

**Student decision:** Se registraron los conocimientos implicitos o "inventados" por el agente junto con el como se describio y como deberia "arreglarse".

**Impact:** `IMPLICIT_KNOWLEDGE.md creado`.

---

## Entry 4

**Tool:** Claude Sonnet 5

**Date:** 05/11/2026

**Stage:** Diseño de AGENTS.md

**Prompt:**

     A @ada-05-spec-driven-feature/AGENTS.md se le añadieron los subtemas:
    - Read First
    - Sources of Truth
    - Repository Map
    - Standard Commands
    - Change Rules
    - Stop Conditions

    Project Rules, validation y Definition of Done ya estaban desde antes.

    Por un lado segun @ada-05-spec-driven-feature/IMPLICIT_KNOWLEDGE.md hay 3 conocimientos implicitos con los que los agentes han tenido problemas. IK-01 explica que el agente no utiliza los comandos de pytest que @ada-05-spec-driven-feature/README.md en su seccion 4 indica  por lo que @ada-05-spec-driven-feature/AGENTS.md deberia "apuntar" a dicha seccion de @ada-05-spec-driven-feature/README.md . IK-02 explica por su parte que el agente trabaja desde una carpeta externa al repositorio , ya que el repositorio esta anidado (ada-05-spec-driven-feature/ada-05-spec-driven-feature/) ya que fue clonado dentro de euna carpeta con el mismo nombre. Y se recomienda una nota a nivel root o en @ada-05-spec-driven-feature/AGENTS.md para no perderse en la naveegacion. 

    Considera este ultimo debido a que en teoria puede solucionarse con abrir vs code en la carpeta correcta. Ademas pon los comandos adecuados en la seccion de Standard commands en @ada-05-spec-driven-feature/AGENTS.md. Haz otras modificaciones necesarias sin tecnisismos

**AI contribution:** Modifico `AGENTS.md` con el fin de incluir la informacion extra que se usaba en el ejemplo del pdf del ADA08 y ademas, de revisar que se tomaran en cuenta los 3 conocimientos implicitos de `IMPLICIT_KNOWLEDGE.md` para el AGENTS.MD. Las secciones de:
- Read First
- Sources of Truth
- Repository Map
- Standard Commands
- Change Rules
- Stop Conditions

Se añadieron a `AGENTS.md`

**Student decision:** Se acpetaron los cambios hechos por el agente y se añadieron cambios extras a la seccion de "Validation" para que considerara los tests que existian.

**Impact:** `AGENTS.md` modificado.

---

## Entry 5

**Tool:** Claude Sonnet 5

**Date:** 15/11/2026

**Stage:** CI

**Prompt:**

    Segun el ADA08 - paso 14 se debe crear un único comando que represente el conjunto mínimo de verificaciones que un agente debe ejecutar antes de 
    declarar DONE. Puede llamar a otros scripts existentes. Adjunto un ejemplo:

    #!/usr/bin/env bash 
    set -euo pipefail 
    
    echo "[1/3] Running tests" 
    pytest -q 
    
    echo "[2/3] Running lint" 
    ruff check .   # example only 
    
    echo "[3/3] Repository checks" 
    python scripts/check_repo_contract.py 
    
    echo "VERIFY_REPO: PASS"

    Pero tambien tiene un subpaso 14.1- CI como contrato ejecutable compartido:

    El mismo comando de verificación que utiliza el agente localmente debe poder ejecutarse de manera independiente. Si el 
    repositorio está alojado en GitHub, configura GitHub Actions; en otra plataforma, utiliza el mecanismo de CI equivalente. 
    No dupliques la lógica de validación dentro del pipeline: CI debe invocar el mismo `scripts/verify_repo.sh`.
    # .github/workflows/verify.yml 
    name: Repository Verification 
    
    on: 
    pull_request: 
    push: 
    
    jobs: 
    verify: 
        runs-on: ubuntu-latest 
        steps: 
        - uses: actions/checkout@v4 
        - uses: actions/setup-python@v5 
            with: 
            python-version: "3.12" 
        - run: pip install -r requirements.txt 
        - run: ./scripts/verify_repo.sh



**AI contribution:**  El agente implemento el `verify.sh` y el `verify.yml`, eso como tal no fue lo complicado pero lo mas importante fue que explico que no hya que hacer configuraciones con respecto a Github actions ya que viene por defecto y que no pasa naa que el comando este en bash ya que el job de GitHub Actions corre en ubuntu-latest, así que el comando que usa CI tiene que poder ejecutarse en Linux. 

**Student decision:** Se recibieron los archivos y se analizaron las explicaciones del agente. En su momento se habia generado el `verify.sh` solo con pytests pero posteriormente se configuro ruff y el `check_repo_contract.py` para incluirlos. El agente solo verifica el estado de las verificacionees, las correciones quedan a cargo del humano, como en el caso de que ruff notifique errores, que se tuvo que hacer por el uso de mayusculas en List y otros errores (ya sea con ruff check --fix o manualmente). 

**Impact:** `verify.sh` y `verify.yml` creados

---

## Entry 6

**Tool:** Claude Sonnet 5

**Date:** 15/11/2026

**Stage:** Guardrail

**Prompt:**

    Analiza el archivo check_repo_contract.py y añadele quee valide que valide que exista el directorio de results/ y que documentacion "core" del proyecto (REQUIREMENTS, SPEC, README, ARCHITECTURE, TASKS, docs/REPO_MAP ) no haga referencias a rutas inexistentes.

**AI contribution:**  El agente modifico la estructura generica de `check_repo_contract.py` para añadir la validacion de rutas y lo limito a la documentacion Core de proyecto.

**Student decision:** Se eligieron la validaciones de rutas debido a los problemas que pueden ocurrir al tener que actualizar la documentacion del proyecto por cambios en el mismo, en muchas ocasiones se cambian rutas de archivos o incluso archivos sse eliminan y sus funciones terminan en otros, por lo que las rutas quedan rotas y el agente puede perderse. Por otro lado se seleccionaron los siguientes documentos como core:
- README.md
- REQUIREMENTS.md
- SPEC.md
- ARCHITECTURE.md
- TASKS.md
- docs/REPO_MAP.md

Ya que son documentacion "pilar" del proyecto al especificarlo y describirlo o documentando lo que se hizo en el.

**Impact:** `check_repo_contract.py` con validaciones de rutas en docs core y de carpeta de results en las validaciones.

---

## Entry 7

**Tool:** Claude Sonnet 5

**Date:** 15/11/2026

**Stage:** Ejecucion After

**Prompt:**

    Implement the change described in @ada-05-spec-driven-feature/TASK_UNDER_TEST.md  
    
    Before making any changes: 
    - inspect the repository, 
    - identify the relevant project context, 
    - determine how the project is built, tested, and validated, 
    - respect the existing architecture and repository conventions. 
    
    When finished: 
    - run the appropriate validation, 
    - report the files modified, 
    - report the commands executed, 
    - provide evidence that the requested change works. 

**AI contribution:**  El agente implemento el cambio del filtro de dominio en el proyecto (que ya estsba limpio de los cambios anteriores y con la documentacion nueva y modificada).Se modificaron los siguiente archivos:
- `search_service.py`
- `cli.py`
- `test_search_service.py`
- `test_cli.py`


**Student decision:** Se analizó la implementacion, donde lo primero que se noto fue el menor alcance de la misma que en el BEFORE, ya que el agente solo modifico 4 documentos. Posteriormente se aceptaron los cambios para la auditoria final. Se tomo en consideracion que no se modificara `TASKS.md` y `Traceability.md` lo que significa que el agente se restringio a un punto en que no registra "historial".


**Impact:**  `search_service.py`, `cli.py`, `test_search_service.py` y `test_cli.py` modificados

## Entry 8

**Tool:** Claude Sonnet 5

**Date:** 15/11/2026

**Stage:** Evaluacion final

**Prompt:** Se utilizo la misma prompt que se uso para crear `REPOSITORY_READINESS_AUDIT` pero ahora indicando que los cambios debian ser para `readiness_improvement_report`

**AI contribution:** El agente en un inicio realizo la auditoria y el reporte, marcando un 88/100 mostrando mejorias significativas como en Automatation y Guardrails (que paso de 1 a 4) pero hubo varios aspectos que se vieron afectados por que el lint (ruff) agregaba errores, que no se habian resuelto en esta version de ada08-baseline. Igualmente el agente recomendo incluir en el proyect.toml el ruff para permitir la disponibilidad del ruff a cualquiera que tenga el repo, en vez de estar instalandolo localmente.

**Student decision:**  Se uso `ruff check --fix` para arreglar la mayoria de errores que detectaba ruff y un error que quedo se soluciono manualmente, despues se hizo la auditoria otra ve donde Agents & Instruction, Automation & Guardrails subieron de 4 a 5, lo que dejo la calificacion en 93/100. 

**Impact:**  `readiness_improvement_report.md` craedo con una calificacion de 93/100