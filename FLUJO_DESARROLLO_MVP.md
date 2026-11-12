# Guía Maestra: Flujo de Desarrollo de MVPs con SDD (Spec-Driven Development)

Esta guía documenta el flujo de trabajo exacto, paso a paso, para construir un **Producto Mínimo Viable (MVP)** utilizando esta plantilla y un agente de inteligencia artificial (Opencode).

El objetivo del desarrollo guiado por especificaciones (SDD) es **eliminar el "vibe coding"** (pedir código a ciegas) y sustituirlo por un proceso ágil, medible y sin errores, donde la IA trabaja bajo límites estrictos de ingeniería de software.

---

## 🗺️ Mapa Visual del Flujo

```text
[0. Configurar Base] ──> AGENTS.md + MEMORY.md
          │
[1. Constitución]    ──> /sdd-constitution  ──> docs/constitution.md
          │
[2. Especificación]  ──> /sdd-spec          ──> specs/NNN-nombre/spec.md (EARS)
          │
[3. Clarificación]   ──> /sdd-clarify       ──> Auditoría de ambigüedades (QA)
          │
[4. Plan Técnico]    ──> /sdd-plan          ──> specs/NNN-nombre/plan.md (Arquitectura)
          │
[5. Tareas]          ──> /sdd-tasks         ──> specs/NNN-nombre/tasks.md (Micro-tareas)
          │
[6. Bucle TDD]       ──> /sdd-implement     ──> Test en rojo ──> Código en verde ──> Parar
          │                                           ↺ Repetir por cada tarea (T1, T2...)
[7. Validación]      ──> /sdd-validate      ──> node --test + Chrome DevTools (375px)
          │
[8. Si hay cambios]  ──> /sdd-change        ──> Spec primero, luego código
```

---

## FASE 0: Preparación de la Plantilla para un Nuevo MVP

Antes de abrir el chat con la IA o lanzar cualquier comando, debes personalizar los dos archivos de contexto principales:

### Paso 0.1: Inicializar el entorno
Copia o clona esta plantilla en la carpeta de tu nuevo proyecto:
```bash
git init
npm init -y   # O el inicializador del stack que prefieras (Python, Go, Vite, etc.)
```

### Paso 0.2: Lo PRIMERO que debes editar: `AGENTS.md`
Este archivo es el **manual de instrucciones permanente del agente**. Ábrelo en tu editor y completa cada sección:

1. **Nombre y descripción:** Qué resuelve tu MVP en 1-2 frases concisas.
2. **Stack y estructura:**
   - Define el lenguaje, runtime y herramientas (ej. Node.js LTS, TypeScript, Tailwind CSS).
   - Especifica dónde se alojará el código (ej. `src/`) y dónde los tests (ej. `tests/`).
3. **Comandos:**
   - Comando para arrancar en desarrollo: ej. `npm run dev`
   - Comando para ejecutar tests: ej. `node --test` o `npm test`
4. **Límites:**
   - **✅ Siempre:** Seguir la spec aprobada, escribir tests unitarios, usar funciones puras.
   - **⚠️ Pregunta antes:** Instalar paquetes externos de npm, cambiar modelos de datos.
   - **🚫 Nunca:** Inventar dependencias, escribir código sin test previo, subir secretos.

### Paso 0.3: Limpiar `MEMORY.md`
Cambia el encabezado con el nombre de tu proyecto:
```markdown
# MEMORY.md — [Nombre de tu MVP]
```
No añadas más contenido por ahora. Este archivo se mantendrá por debajo de 50 líneas resumiendo el estado del proyecto entre sesiones.

---

## FASE 1: Definir los Principios Innegociables (`/sdd-constitution`)

Para que el MVP no crezca descontroladamente ni se llene de dependencias innecesarias, se fijan 6 principios arquitectónicos.

### Ejecución
Escribe en el chat de Opencode:
```bash
/sdd-constitution "Queremos un MVP ligero enfocado en velocidad de carga y sin frameworks pesados"
```

### Qué sucede tras bambalinas
1. El agente lee `AGENTS.md`, `MEMORY.md` y tu instrucción.
2. Te propone en el chat 6 principios innegociables (simplicidad, trazabilidad spec-código, separación de lógica e interfaz, tests unitarios, datos locales e idioma).
3. **El agente NO modifica el archivo automáticamente**, se detiene y espera tu confirmación.

### Tu acción
Revisa los 6 principios. Si estás de acuerdo, escribe:
> *"Aprobado, guárdalo en docs/constitution.md"*.

---

## FASE 2: Especificar la Funcionalidad en EARS (`/sdd-spec`)

Un MVP se divide en especificaciones numeradas (`001-nombre`, `002-nombre`). Cada una cubre un flujo funcional completo.

### Ejecución
```bash
/sdd-spec 001-registro-usuarios "Permitir registro de usuario con email y contraseña con validaciones básicas"
```

### Qué sucede tras bambalinas (La Entrevista)
1. El agente no programa nada.
2. Te formulará preguntas de **UNA en UNA** (máximo 5) para eliminar asunciones. Por ejemplo:
   - *¿Qué caracteres mínimos requiere la contraseña?*
   - *¿Qué sucede si el email ya existe?*
   - *¿Se incluye recuperación de contraseña en esta versión inicial o queda fuera del MVP?*
3. Con tus respuestas, redacta `specs/001-registro-usuarios/spec.md`.

### Estándar EARS obligatorio en la spec
Cada Requisito Funcional (RF) quedará escrito sin ambigüedad:
- **Evento:** `RF-1: CUANDO el usuario pulsa 'Registrarse', EL SISTEMA valida el formato del email.`
- **Error:** `RF-2: SI el email no contiene '@', ENTONCES EL SISTEMA muestra 'Email no válido'.`
- **Estado:** `RF-3: MIENTRAS la petición esté en curso, EL SISTEMA deshabilita el botón de envío.`
- **Permanente:** `RF-4: EL SISTEMA almacena las contraseñas con hash seguro.`

---

## FASE 3: Auditoría de Calidad y Casos Límite (`/sdd-clarify`)

Antes de diseñar la solución técnica, ponemos al agente en rol de **QA profesional** para detectar lagunas.

### Ejecución
```bash
/sdd-clarify 001-registro-usuarios
```

### Qué sucede tras bambalinas
El agente analiza `spec.md` y genera una lista numerada con:
- Ambigüedades detectadas (requisitos no verificables).
- Contradicciones lógicas.
- Casos límite no cubiertos (ej. espacios en blanco en el email, fallo de red).
- Conflictos con `docs/constitution.md`.

### Tu acción
Responde a las dudas señaladas. El agente actualizará la spec e indicará:
```markdown
Estado: aprobada
```

---

## FASE 4: Planificación Técnica de Arquitectura (`/sdd-plan`)

Ahora definimos **CÓMO** se resuelve técnicamente la spec, sin escribir código aún.

### Ejecución
```bash
/sdd-plan 001-registro-usuarios
```

### Qué genera en `specs/001-registro-usuarios/plan.md`
1. **Estructura de archivos:** Qué ficheros se crearán o modificarán y la responsabilidad de cada uno.
2. **Lógica desacoplada (Funciones Puras):** Toda la lógica de negocio (validar email, calcular estados, filtrar datos) residirá en funciones puras sin dependencias de DOM ni librerías de terceros.
   - *Regla de oro:* Inyectar parámetros externos como dependencias (ej. pasar la fecha `hoy` como argumento para que el test sea determinista).
3. **Pseudocódigo:** Algoritmo principal paso a paso.
4. **Diseño de interfaz:** Qué componentes visuales intervienen y cómo interactúan.
5. **Estrategia de pruebas:** Lista de tests que se escribirán con el test runner nativo (`node --test`).

---

## FASE 5: Desglose en Tareas Atómicas (`/sdd-tasks`)

Convertimos el plan en una lista de trabajo secuencial y fácilmente gestionable.

### Ejecución
```bash
/sdd-tasks 001-registro-usuarios
```

### Qué genera en `specs/001-registro-usuarios/tasks.md`
Crea una lista de entre 3 y 10 tareas atómicas (máximo 20-30 minutos por tarea) en estricto orden de dependencia:
```markdown
- [ ] **T1. Crear módulo de validación de credenciales con funciones puras.** RF-1, RF-2
  - Hecho cuando: Los tests unitarios de validación de email y contraseña pasen al 100%.

- [ ] **T2. Implementar servicio de almacenamiento local de usuarios.** RF-4
  - Hecho cuando: Se pueda registrar y consultar un usuario en memoria/storage mediante tests.

- [ ] **T3. Crear formulario visual y conectar eventos de interfaz.** RF-3
  - Hecho cuando: El formulario muestre los errores en pantalla y bloquee el botón al enviar.
```

---

## FASE 6: Bucle de Implementación TDD (`/sdd-implement`)

Esta es la fase de codificación. Se ejecuta **tarea por tarea de forma disciplinada**:

### Ejecución de una tarea
```bash
/sdd-implement 001-registro-usuarios T1
```

### El ciclo obligatorio de la IA
1. **Rojo (Test primero):** Escribe el test unitario para T1 y lo ejecuta con `node --test`. Comprueba que **falla**.
2. **Verde (Código mínimo):** Escribe el código en `src/` estrictamente necesario para que el test pase.
3. **Verificación:** Ejecuta `node --test` y muestra el resultado en verde en la terminal.
4. **Marcado:** Marca `[x] T1` en `tasks.md`.
5. **PARADA OBLIGATORIA:** El agente **se detiene**. No continúa con T2.

### Cómo avanzar
Tú revisas el código generado y das la orden para la siguiente tarea:
```bash
/sdd-implement 001-registro-usuarios T2
```
Y así sucesivamente hasta completar todas las tareas.

---

## FASE 7: Validación Global del Módulo (`/sdd-validate`)

Cuando todas las tareas de `tasks.md` estén marcadas como hechas, comprobamos que el módulo cumple al 100% la especificación inicial.

### Ejecución
```bash
/sdd-validate 001-registro-usuarios
```

### Qué hace el agente
1. **Mapeo RF vs Tests:** Recorre `spec.md` requisito por requisito (RF-1, RF-2...) comprobando qué test cubre cada uno y ejecutando la suite completa.
2. **Auditoría visual con Chrome DevTools (MCP):**
   - Abre la interfaz en el navegador.
   - Inspecciona renderizado, clics y formularios.
   - Emula un viewport móvil de **375 px** para verificar diseño responsive.
3. **Veredicto:** Si todo pasa, cambia el estado de la spec a:
   ```markdown
   Estado: implementada
   ```
4. Actualiza `MEMORY.md` resumiendo el hito alcanzado.

---

## FASE 8: Gestión de Cambios o Pivote del MVP (`/sdd-change`)

Durante el desarrollo de un MVP es normal cambiar de opinión tras ver una pantalla. **Nunca le pidas a la IA que cambie código directamente.**

### Ejecución
```bash
/sdd-change 001-registro-usuarios "Añadir botón para mostrar/ocultar contraseña con icono de ojo"
```

### Proceso de cambio
1. El agente edita `spec.md` creando un nuevo RF en EARS.
2. Te muestra el `diff` (la diferencia de texto) y espera tu aprobación.
3. Una vez aprobado, actualiza `plan.md` y añade las nuevas tareas necesarias (ej. `T4`) a `tasks.md`.
4. Vuelves a la Fase 6 para implementar la nueva tarea con `/sdd-implement 001-registro-usuarios T4`.

---

## 🛠️ Comandos de Apoyo Rápido

### ¿Dónde me quedé? (`/sdd-status`)
Si cierras sesión o retomas el proyecto al día siguiente:
```bash
/sdd-status 001-registro-usuarios
```
*Te dirá exactamente qué tareas están hechas, cuáles faltan y qué comando debes escribir a continuación.*

### Tareas rápidas o fixes puntuales (`/feature`)
Si necesitas arreglar un bug menor o ajustar un estilo sin hacer todo el ciclo formal de SDD:
```bash
/feature "Centrar el contenedor principal en pantallas ultra-wide"
```
*El agente preparará un micro-plan respetando `AGENTS.md` y esperará tu aprobación antes de tocar código.*

---

## 💡 Las 5 Reglas de Oro para triunfar con este flujo

1. **La IA nunca programa sin spec:** Si no está en `spec.md`, no existe en el código.
2. **Una sola tarea por turno:** No ejecutes varias tareas a la vez; el valor de SDD reside en la pausa y verificación entre tareas.
3. **Lógica separada de la vista:** Toda regla de cálculo o validación debe poder probarse desde la terminal con `node --test` sin levantar navegador ni DOM.
4. **Fechas y tiempo siempre inyectados:** Nunca uses `new Date()` o `Date.now()` dentro de una función pura; pásalos como argumento (`hoy = new Date()`).
5. **Mantén `MEMORY.md` limpio:** No dejes que supere las 50 líneas. Resume decisiones y borra notas obsoletas tras cada hito.
