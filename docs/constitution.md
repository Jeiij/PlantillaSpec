# Constitución del Proyecto

Principios innegociables, verificables y de máxima jerarquía para este proyecto.

1. **Simplicidad del Stack:** Stack mínimo viable; no añadir librerías sin justificar por qué la solución nativa es insuficiente.
2. **La Spec Manda:** Todo desarrollo parte de una spec aprobada. No se escribe código sin un RF asociado. Si falta una decisión, se para y se pregunta.
3. **Separación de Lógica e Interfaz:** La lógica de negocio reside en funciones puras y desacopladas de la UI, con dependencias inyectadas (ej. fechas pasadas como parámetro).
4. **Política de Tests:** Ninguna tarea se da por terminada sin tests unitarios asociados ejecutados y pasando en verde (`node --test`).
5. **Protección de Datos:** Los datos del usuario son sagrados; no se transmiten a terceros ni se persisten sin consentimiento explícito.
6. **Idioma y Convenciones:** El código, variables y commits en inglés (o español según proyecto); interfaz de usuario y documentación en español.
