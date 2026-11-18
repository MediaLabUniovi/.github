# Guía de Estructura y Formateo de Proyectos para Agentes de IA

Este documento define el estándar oficial de arquitectura de repositorios, documentación, control de versiones y seguridad que cualquier agente de IA debe aplicar al inicializar, desarrollar, refactorizar o documentar proyectos técnicos.

---

## 1. Etapa 0: Clasificación Obligatoria de Tipología del Proyecto

Antes de crear cualquier carpeta o archivo, el agente **DEBE clasificar el proyecto** en una de las tres tipologías admitidas. Queda estrictamente prohibido crear carpetas vacías o parásitas:

- **Tipo A: Sistemas Embebidos / IoT (Hardware + Software):**
  Aplica la arquitectura dual modular (`hardware/` y `software/`). Cada submódulo es autónomo con su propio `README.md`.
- **Tipo B: Software Puro (Software-Only):**
  Aplicaciones web, móviles, librerías, scripts o utilidades de consola. Estructura estándar (`src/`, `tests/`, etc.). **Queda estrictamente prohibido crear la carpeta `hardware/`**.
- **Tipo C: Hardware Puro (Hardware-Only):**
  Diseños electrónicos, PCBs o sistemas mecánicos sin firmware (`schematics/`, `gerber/`, `datasheets/`). **Queda estrictamente prohibido crear la carpeta `software/`**.

---

## 2. Principios de Estilo y Formato

1. **Encabezados Limpios para Interoperabilidad de Anclajes (Slugs):**
   - Los títulos de nivel Markdown (`#`, `##`, `###`) **deben escribirse siempre en texto limpio sin emoticonos** (ejemplo: `## Estructura del Repositorio`, `## Compilacion y Carga`).
   - *Justificación técnica:* Evita la rotura de enlaces internos y tablas de contenidos generadas automáticamente entre diferentes renderizadores (GitHub cmark, GitLab, VS Code).
2. **Paleta Mínima y Sobria de Emoticonos Funcionales:**
   - Se elimina cualquier emoticono redundante o informal (como grúas `🏗️`, archivadores `🗂️` o carpetas abiertas `📂`).
   - Se restringe el uso exclusivamente a esta paleta funcional de 5 símbolos, utilizables únicamente en árboles de texto o enlaces a carpetas:
     - `📁` : Exclusivo para directorios y submódulos (`[📁 Hardware](hardware/)`, `[📁 Software](software/)`).
     - `🛠️` : Para comandos de compilación y herramientas.
     - `🔌` : Para conexiones físicas y asignación de pines (Pinout).
     - `⚠️` : Para avisos de seguridad, bloqueos o tareas pendientes.
     - `📌` : Para seguimiento de incidencias (GitHub Issues).
   - **Estrictamente prohibidos:** En párrafos explicativos, tablas de datos, nombres de variables, comandos o viñetas internas.
3. **Navegabilidad con Enlaces Relativos:** Todos los enlaces entre módulos deben ser relativos y verificados (ej. `[📁 Hardware](hardware/)`, `[`hardware/README.md`](hardware/README.md)`).

---

## 3. Protocolo de Seguridad Anti-Fuga de Secretos

En proyectos con credenciales, tokens de API o claves de red (Wi-Fi, LoRaWAN, BLE, Firebase):

1. **Aislamiento Obligatorio en Archivo Dedicado:**
   - **NUNCA** incrustar claves en el código fuente principal (ej. `main.cpp`, `App.kt`).
   - Las credenciales deben residir siempre en archivos independientes de configuración (ej. `credentials.h`, `.env`, `secrets.ini`).
2. **Desvinculación del Índice de Git:**
   - Si el archivo de configuración con credenciales reales ya fue trackeado previamente por Git, añadirlo a `.gitignore` no lo protege. El agente **debe ejecutar obligatoriamente:**
     ```bash
     git rm --cached <ruta-del-archivo-sensible>
     ```
   - Añadir la ruta correspondiente dentro del archivo `.gitignore`.
3. **Generación Automática de Plantilla Segura:**
   - El agente debe generar de inmediato un archivo plantilla (ej. `credentials.example.h` o `.env.example`) con la estructura exacta pero con valores ficticios (`"YOUR_WIFI_PASSWORD"`).
4. **Aviso Explícito por Consola:**
   - El agente debe informar en el chat al usuario: *"Se han aislado las credenciales en [archivo], se ha desindexado del control de versiones y se ha provisto una plantilla [archivo.example] para despliegue."*

---

## 4. Gestión Automatizada de GitHub Issues y Fallbacks

### 4.1. Verificación Previa del Entorno
Antes de intentar crear un issue, el agente debe verificar si el cliente CLI está autenticado y si existe un repositorio remoto:
```bash
gh auth status
```

- **Si `gh` está autenticado y existe remoto en GitHub:** Proceder mediante la CLI de GitHub.
- **Si `gh` no está disponible o el proyecto es puramente local:** Registrar la tarea de forma estructurada en un archivo local `TODO.md` en la raíz del proyecto.

### 4.2. Creación Segura de Issues ante Bloqueos o Tareas Pendientes
Si surge una limitación técnica, periférico no operativo o función pendiente:
1. El agente debe redactar la descripción en un archivo temporal para evitar roturas por caracteres especiales o comillas en la shell:
   ```bash
   gh issue create --title "Contexto técnico directo" --body-file temp_issue.md
   ```
2. Registrar el issue en la sección `## Seguimiento del Proyecto (GitHub Issues)` del `README.md` de la raíz.

### 4.3. Resolución y Cierre Automático
- Cuando el agente solucione el problema, debe incluir en el mensaje de commit la palabra clave estándar: `fix(modulo): descripcion (Closes #X)`.
- Al realizar el push a la rama por defecto, GitHub cerrará automáticamente el issue vinculado, evitando redundancia de comandos de cierre manual.

---

## 5. Control de Versiones: Conventional Commits

Todo commit generado por el agente debe cumplir la especificación estándar de Conventional Commits (`@commitlint`):

- `feat(scope):` Nueva funcionalidad (ej. `feat(audio): implementar buffer I2S para ES8311`).
- `fix(scope):` Corrección de fallos (ej. `fix(lora): corregir timeout en ciclo deep sleep (Closes #4)`).
- `docs:` Modificaciones en documentación o READMEs.
- `chore(hw):` Actualizaciones en esquemáticos, pinout o lista de materiales (BOM).
- `refactor:` Mejoras internas sin alteración de funcionalidad observable.

---

## 6. Estructura de Directorios Estándar (Ejemplo Tipo A: Embedded/IoT)

```text
nombre-del-proyecto/
├── README.md            # Presentación general, visión, arquitectura y enlaces a módulos
├── LICENSE              # Licencia formal de código abierto (MIT, Apache-2.0, CERN-OHL)
├── .gitignore           # Exclusiones de Git y protección de archivos con secretos
├── hardware/            # Esquemáticos, datasheets y asignación de pines
│   ├── README.md        # Documentación de hardware y tabla de pinout
│   ├── schematics/      # Esquemáticos electrónicos oficiales (PDF, etc.)
│   └── datasheets/      # Hojas de datos de componentes críticos en PDF
└── software/            # Firmware, código fuente, compilación y pruebas
    ├── README.md        # Documentación técnica del firmware, arquitectura y tareas
    ├── platformio.ini   # O CMakeLists.txt / Makefile / build.gradle con versiones fijadas
    ├── include/         # Cabeceras y plantillas seguras (credentials.example.h)
    └── src/             # Código fuente estructurado (C/C++, Kotlin, etc.)
```

---

## 7. Plantilla del README Raíz (`/README.md`)

```markdown
# Nombre-Proyecto: Subtítulo Descriptivo Breve

Descripción concisa de una o dos frases resumiendo qué es el sistema, en qué hardware/plataforma se basa y qué problema resuelve.

---

## Visión del Proyecto

Párrafo explicativo detallando el propósito del proyecto, la experiencia de usuario o funcionalidad esperada, y los requerimientos clave (bajo consumo, protocolos directos, interacción física, etc.).

---

## Estructura del Repositorio

El repositorio está organizado en módulos independientes:

\`\`\`text
Nombre-Proyecto/
├── README.md            # Presentación general del proyecto
├── hardware/            # Esquemáticos, datasheets de componentes y asignación de pines
└── software/            # Código fuente, entorno de compilación y controladores
\`\`\`

### [📁 Hardware](hardware/)
Contiene la documentación técnica de la placa y componentes físicos:
- **Esquemático oficial en PDF** del circuito electrónico.
- **Hojas de datos (Datasheets)** de los componentes clave.
- Tabla detallada de pines y asignaciones de periféricos en [`hardware/README.md`](hardware/README.md).

### [📁 Software](software/)
Contiene el código fuente completo y el entorno de compilación:
- **Firmware / Lógica principal:** Implementación en [Lenguaje] bajo [Entorno/Framework].
- **Protocolos y periféricos:** Descripción de comunicaciones y algoritmos críticos.
- Guía de compilación, flasheo y tareas pendientes en [`software/README.md`](software/README.md).

---

## Seguimiento del Proyecto (GitHub Issues)

1. [**Issue #X: Título descriptivo de tarea o bug abierto**](https://github.com/usuario/repo/issues/X)
2. [**Issue #Y: Título descriptivo de mejora en desarrollo**](https://github.com/usuario/repo/issues/Y)

---

## Licencia

Distribuido bajo licencia [MIT / Apache-2.0 / CERN-OHL]. Consulte el archivo `LICENSE` para más detalles.
```

---

## 8. Plantilla de Hardware (`/hardware/README.md`)

```markdown
# Documentación Hardware: [Nombre de Placa o Módulo]

Documentación técnica, esquemáticos y hojas de características (datasheets) de los componentes electrónicos.

- **Web oficial / Wiki:** [Enlace a documentación del fabricante]
- **Repositorio de referencia:** [Enlace a esquemáticos o repo base]

---

## Archivos Incluidos en este Directorio

### 1. Esquemáticos
- [esquematico-v1.0.pdf](schematics/esquematico-v1.0.pdf): Esquemático electrónico completo oficial de la placa.

### 2. Datasheets de Componentes
- [componente-principal.pdf](datasheets/componente-principal.pdf): Descripción técnica del microcontrolador o SoC principal.
- [sensor-secundario.pdf](datasheets/sensor-secundario.pdf): Controlador auxiliar o sensor (interfaz I2C/SPI).

---

## Asignación de Pines (Pinout)

### [Periférico 1 - Interfaz SPI / I2C / UART]
| Señal | Pin MCU | Función |
| :--- | :--- | :--- |
| `LINEA_CS` | GPIO X | Chip Select |
| `LINEA_SCK` | GPIO Y | Clock |
| `LINEA_DATA` | GPIO Z | Datos |

### [Periférico 2 - Alimentación y Control]
| Señal | Pin MCU | Función |
| :--- | :--- | :--- |
| `PWR_EN` | GPIO A | Habilitación de regulador / amplificador |
| `INT_ALERT` | GPIO B | Interrupción por flanco de bajada |
```

---

## 9. Plantilla de Software (`/software/README.md`)

```markdown
# Documentación Técnica del Software ([Framework / MCU])

Firmware y módulos de control desarrollados en [C++ / Rust / Kotlin] bajo [PlatformIO / ESP-IDF / Android SDK].

---

## Arquitectura y Funcionamiento del Firmware

### 1. [Subsistema Crítico 1: Comunicaciones / Red]
- Puntos técnicos clave con formato de viñeta.
- Formato de tramas binarias, canales, frecuencias o direcciones de red.
- Mecanismos de seguridad o descarte de paquetes erróneos.

### 2. [Subsistema Crítico 2: Gestión de Potencia / Sensores / UI]
- Máquinas de estados, tiempos de muestreo y filtros aplicados.
- Estrategias de bajo consumo (AOD, Deep Sleep, redibujado parcial).

---

## Organización de Archivos en `software/`

\`\`\`text
software/
├── platformio.ini         # Configuración del entorno, librerías y flags fijados
├── README.md              # Documentación técnica y estado
└── src/
    ├── main.cpp           # Inicialización y bucle principal
    ├── driver_a.h         # Controlador de periférico A
    └── driver_b.h         # Controlador de periférico B
\`\`\`

---

## Compilación y Carga

Instrucciones exactas de terminal para compilar y flashear el dispositivo:

\`\`\`bash
cd software

# Compilar el proyecto
pio run

# Flashear en el dispositivo conectado (indicar puerto detectado)
# En Windows: COMx | En Linux: /dev/ttyUSB0
pio run -t upload --upload-port <PUERTO_SERIE>
\`\`\`

---

## Estado de Desarrollo y Tareas Pendientes (GitHub Issues)

### 1. [Nombre de la Tarea Técnica] — [Issue #X](https://github.com/usuario/repo/issues/X)
- **Situación actual:** Descripción concisa del comportamiento actual o bloqueo detectado.
- **Implementación existente:** Qué partes ya están programadas y verificadas.
- **Siguientes pasos:** Pasos concretos para su resolución.
```

---

## 10. Checklist de Verificación Final para el Agente

Antes de dar por concluida la sesión o creación de un proyecto, el agente debe verificar:

- [ ] ¿Se clasificó correctamente el proyecto (Embedded, Software-Only o Hardware-Only) sin crear carpetas parásitas vacías?
- [ ] ¿Los encabezados Markdown (`#`, `##`) están en texto limpio sin emoticonos para asegurar anclajes `#slug` universales?
- [ ] ¿Se utiliza únicamente la paleta mínima permitida (`📁`, `🛠️`, `🔌`, `⚠️`, `📌`) en árboles o enlaces y NUNCA en el cuerpo del texto?
- [ ] ¿Se aislaron las credenciales en archivos dedicados, asegurando que están en `.gitignore` y desindexadas con `git rm --cached` si correspondía?
- [ ] ¿Se generaron las plantillas `*.example` correspondientes para cualquier archivo de configuración sensible?
- [ ] ¿Se verificó `gh auth status` antes de invocar la CLI de GitHub, usando `TODO.md` como fallback si no está autenticado?
- [ ] ¿Los commits cumplen con Conventional Commits (`feat:`, `fix:`, `chore(hw):`) y enlazan issues con `Closes #X`?
- [ ] ¿Las instrucciones de flasheo utilizan variables neutras (`<PUERTO_SERIE>`) sin hardcodear puertos exclusivos de una máquina?
- [ ] ¿Existe un archivo `LICENSE` formal en la raíz del repositorio?
