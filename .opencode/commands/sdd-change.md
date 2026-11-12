---
description: SDD · Gestión de cambios de requisitos: primero spec, luego código (uso: /sdd-change NNN-nombre nuevo requisito)
agent: plan
---
Nuevo requisito para la spec specs/$1/: $ARGUMENTS (la carpeta/código es $1, y el nuevo requisito es el texto posterior).
NO toques código. Usa la skill sdd.
1. Actualiza specs/$1/spec.md: nuevo RF (o cambio de uno existente) en EARS,
   sus casos límite y lo que queda fuera de alcance.
2. Indica qué partes de plan.md y tasks.md habría que cambiar después.
3. Muéstrame el diff de la spec y espera mi aprobación.
