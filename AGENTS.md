# AGENTS.md — [Nombre del proyecto]
[Una o dos frases: qué es, para quién y cuál es su objetivo.]

## Stack y estructura
- **Tecnologías y versiones:** (ej. Node.js LTS, TypeScript / JavaScript, Framework o Vanilla, CSS).
- **Estructura relevante:**
  - `specs/`: Especificaciones funcionales, planes técnicos y listas de tareas.
  - `docs/`: Documentación general y `constitution.md`.
  - `src/` o raíz: Código fuente del proyecto.
  - `tests/`: Tests unitarios y de integración.

## Comandos
- **Desarrollo / Ejecutar:** `npm run dev` (o comando correspondiente)
- **Tests unitarios:** `node --test` (o `npm test`)
- **Lint / Verificación:** `npm run lint` / `npm run check`

## Convenciones
- **Código y comentarios:** Nombres de variables, funciones y archivos en inglés (o español); textos de interfaz y especificaciones en español.
- **Arquitectura de código:** Separar lógica de negocio de la interfaz. La lógica debe residir en funciones puras testeables sin dependencias de DOM ni navegador (inyectar parámetros como fechas `hoy`).
- **Archivo de referencia:** `[indicar archivo modelo de estilo si aplica]`

## Reglas de dominio / trampas conocidas
- [Detallar peculiaridades del negocio o trampas que el agente no pueda deducir leyendo el código].

## Forma de trabajar (SDD)
- Metodología estricta de **Spec-Driven Development**:
  1. No tocar código sin una spec y plan aprobados (`specs/NNN-nombre/`).
  2. Implementar paso a paso mediante `/sdd-implement NNN-nombre Tn`.
  3. Tests primero (en rojo), código hasta verde, verificar y parar tras cada tarea.
  4. Mantener `MEMORY.md` actualizado tras cada hito o sesión.

## Límites
- ✅ **Siempre:** seguir la spec aprobada, escribir tests unitarios, usar funciones puras para lógica.
- ⚠️ **Pregunta antes:** añadir dependencias externas, crear archivos fuera del plan aprobado, cambiar esquemas de datos o contratos de API.
- 🚫 **Nunca:** escribir código que no responda a un RF de la spec, modificar archivos ajenos a la tarea en curso, hardcodear secretos o credenciales.

## Verificación
- Ejecutar suite de pruebas: `node --test` (debe pasar al 100% en verde).
- Si hay interfaz web: verificar visualmente y con MCP Chrome DevTools (incluyendo viewport móvil 375px).
