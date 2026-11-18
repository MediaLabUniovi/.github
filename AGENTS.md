# MediaLabUniovi — Reglas Oficiales para Agentes de IA

Este documento establece las directrices operativas que **todo agente de IA** (Antigravity, Copilot Workspace, Claude Code, OpenAI, etc.) debe seguir de forma estricta al inicializar, refactorizar, programar o documentar repositorios dentro de la organización **MediaLabUniovi**.

Para la especificación completa, arquitectura de referencia y plantillas de README, consulta [NORMAS_PROYECTOS_AGENTES.md](./NORMAS_PROYECTOS_AGENTES.md).

---

## 1. Etapa 0: Clasificación Obligatoria de Tipología

Antes de crear cualquier carpeta o archivo, el agente **debe clasificar el proyecto**:
- **Tipo A (Sistemas Embebidos / IoT):** Arquitectura dual obligatoria (`hardware/` y `software/`), cada uno con su propio `README.md`.
- **Tipo B (Software Puro):** Aplicaciones web, backend, librerías, scripts (`src/`, `tests/`, etc.). **Prohibido crear `hardware/`**.
- **Tipo C (Hardware Puro):** Esquemáticos, PCBs, CAD/CAM (`schematics/`, `gerber/`, `datasheets/`). **Prohibido crear `software/`**.

*Queda prohibido crear carpetas vacías o parásitas.*

---

## 2. Estilo Markdown y Paleta de Emoticonos Funcionales

1. **Encabezados Limpios (Slugs Universales):**
   - Todos los títulos (`#`, `##`, `###`) **deben ser texto plano sin emoticonos** (ej. `## Estructura del Repositorio`, `## Compilacion y Carga`) para evitar la rotura de anclajes en GitHub.
2. **Paleta Mínima y Sobria:**
   - Se restringe el uso de emoticonos exclusivamente a estos 5 marcadores funcionales en árboles y enlaces:
     - `📁` : Directorios y módulos (`[📁 Hardware](hardware/)`).
     - `🛠️` : Herramientas y comandos de compilación.
     - `🔌` : Conexiones y pinout.
     - `⚠️` : Avisos de seguridad o bloqueos pendientes.
     - `📌` : Seguimiento de incidencias (GitHub Issues).
   - **Prohibido:** Usar emoticonos en texto explicativo, tablas, nombres de variables o comandos.

---

## 3. Protocolo Estricto de Seguridad y Secretos

1. **Aislamiento:** NUNCA colocar credenciales, tokens o contraseñas en el código principal (`main.cpp`, `config.json`, etc.). Deben estar en archivos dedicados (`credentials.h`, `.env`, `secrets.ini`).
2. **Desindexación de Git:** Si un archivo con secretos fue trackeado previamente:
   ```bash
   git rm --cached <ruta-archivo-sensible>
   ```
   Añadir la ruta a `.gitignore`.
3. **Plantilla segura obligatoria:** Crear siempre un archivo gemelo de ejemplo (ej. `credentials.example.h` o `.env.example`) con placeholders ficticios (`"YOUR_API_KEY"`).

---

## 4. Gestión de Incidencias (GitHub Issues)

1. **Verificación previa:**
   ```bash
   gh auth status
   ```
   - Si `gh` está autenticado: crear issues vía CLI con `--body-file temp_issue.md`.
   - Si `gh` no está disponible o el proyecto es puramente local: registrar tareas en `TODO.md`.
2. **Cierre automatizado en commits:** Usar palabras clave estándar en commits: `fix(scope): descripcion (Closes #X)`.

---

## 5. Control de Versiones

- Formato obligatorio: **Conventional Commits** (`feat:`, `fix:`, `docs:`, `chore(hw):`, `refactor:`).
- Nunca hardcodear configuraciones dependientes de máquina local (ej. puertos serie fijos como `COM6`; usar `<PUERTO_SERIE>`).
- Todo repositorio debe contar con archivo `LICENSE` formal en la raíz.
