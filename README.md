# dev-agents

Un equipo de desarrollo pequeño como subagentes de Claude Code. Cada agente tiene una responsabilidad, las herramientas que necesita y límites escritos de lo que **no** puede hacer. Hay una persona dueña que aprueba todo lo que llega a producción, cuesta dinero o sale hacia terceros.

*English version below.*

## Agentes

| Agente | Responsabilidad | Modelo | Puede escribir código | Nunca |
|---|---|---|---|---|
| [coordinator](agents/coordinator.md) | Divide objetivos en tareas, asigna agentes y reporta | Sonnet | No | Hacer merge, desplegar |
| [functional-analyst](agents/functional-analyst.md) | Specs, historias, reglas de negocio y criterios EARS | Sonnet | No (solo docs) | Decidir la solución técnica |
| [tech-lead](agents/tech-lead.md) | Planes técnicos, contratos y ADR | Opus | Docs y contratos | Imponer: propone y la persona decide |
| [code-auditor](agents/code-auditor.md) | Revisión de PR: corrección y mantenibilidad | Sonnet | No | Modificar código |
| [qa-engineer](agents/qa-engineer.md) | Pruebas contra los criterios y reportes de bugs | Sonnet | Solo pruebas | Probar en producción o con cuentas reales |
| [security-reviewer](agents/security-reviewer.md) | Modelo de amenazas, revisión de seguridad, bloqueo de releases | Sonnet | No | Aplicar parches por su cuenta |
| [backend-engineer](agents/backend-engineer.md) | APIs y lógica de servidor (JVM por defecto) | Sonnet | Sí | Tocar producción, cambiar contratos solo |
| [frontend-engineer](agents/frontend-engineer.md) | Interfaces web y landings | Sonnet | Sí | Cambiar contratos de API |
| [android-engineer](agents/android-engineer.md) | Apps Android en Kotlin, pruebas con adb | Sonnet | Sí | Publicar en Google Play |
| [ios-engineer](agents/ios-engineer.md) | Apps iOS en Swift, pruebas con xcodebuild | Sonnet | Sí | Subir a App Store |
| [data-engineer](agents/data-engineer.md) | Esquemas, migraciones e índices | Sonnet | Migraciones | Ejecutar SQL destructivo |
| [infrastructure-engineer](agents/infrastructure-engineer.md) | Infraestructura como código y CI/CD | Sonnet | IaC | Aplicar sin plan y aprobación |
| [ui-ux-designer](agents/ui-ux-designer.md) | Identidad visual y experiencia antes del desarrollo: dirección de arte, logo, paleta AA, tipografía, tokens y maquetas HTML | Sonnet | Maquetas y tokens, no código de producción | Publicar, cambiar la marca sin aprobación |
| [seo-geo-specialist](agents/seo-geo-specialist.md) | Intención de búsqueda, mapa de palabras clave, SEO local, contenido citable por IA, datos estructurados, auditoría y reporte mensual | Sonnet | Solo por PR | Comprar enlaces, inventar volúmenes de búsqueda |

## Flujo de una funcionalidad

```text
idea → functional-analyst (spec + criterios EARS) → aprobación
     → tech-lead (plan + ADR) + security-reviewer (amenazas) → aprobación
     → coordinator (tareas) → agentes de área (en worktrees separados)
     → code-auditor + security-reviewer + qa-engineer → la persona hace merge
```

## Flujo de un sitio o landing

```text
spec aprobada → seo-geo-specialist (intención + mapa de palabras clave por página)
     → ui-ux-designer (dirección de arte → aprobación → logo, paleta, tokens, maquetas;
                       anti-generic-design-review antes de entregar) → aprobación
     → copy (la persona o quien ella designe, con el mapa de palabras clave) → aprobación
     → frontend-engineer (implementa con los tokens) → code-auditor (anti-generic-design-review en PR de interfaz)
     → seo-geo-specialist (auditoría antes de publicar) → la persona publica
```

Los agentes usan las skills del plugin [dev-workflow](https://github.com/nvv-dev/dev-workflow) (TDD, depuración, verificación, revisión, EARS, ADR…). Los límites se refuerzan con los hooks de [agent-guardrails](https://github.com/nvv-dev/agent-guardrails); un perfil dice lo que el agente no debe hacer, un hook lo impide.

## Instalación

```text
/plugin marketplace add nvv-dev/claude-plugins
/plugin install dev-agents@nvvdev
/plugin install dev-workflow@nvvdev
/plugin install agent-guardrails@nvvdev
```

O copia los archivos de `agents/` que quieras a `.claude/agents/` en tu repo y ajústalos a tu stack.

## Cómo adaptarlos

- **Stack:** los agentes de área nombran un stack por defecto (por ejemplo JVM para backend). Edita el perfil con el de tu equipo.
- **Modelo:** Sonnet por defecto, Opus solo para el tech-lead. Cámbialo en el campo `model` según tu plan.
- **Herramientas:** el auditor y el revisor de seguridad no tienen `Write` ni `Edit` a propósito. No se los agregues.
- **Activación por oleadas:** no actives los catorce desde el primer día. Empieza con coordinator, code-auditor y el agente de tu stack principal, y agrega los demás cuando haya trabajo real para ellos.

## Licencia

MIT. Ver [CREDITS.md](CREDITS.md).

---

## English

A small software team as Claude Code subagents. Each agent has one responsibility, the tools it needs, and written limits on what it must **not** do. One human owner approves everything that reaches production, costs money or goes to third parties.

Install with `/plugin marketplace add nvv-dev/claude-plugins` and `/plugin install dev-agents@nvvdev` (together with `dev-workflow` and `agent-guardrails`), or copy the files in `agents/` into your repo's `.claude/agents/` and adapt the stack, model and tools. Reviewers (code-auditor, security-reviewer) intentionally have no write tools. The ui-ux-designer and seo-geo-specialist cover public sites: keyword map, then design, copy, implementation and a pre-publication SEO/GEO audit. Roll agents out in waves, only when there is real work for them. License: MIT.
