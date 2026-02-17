# System Definition - AI Agents

## 🎯 Arquitectura de Agentes del Sistema SaaS Multi-Tenant

Este documento define la arquitectura completa de agentes IA que integran el ciclo de desarrollo del proyecto SaaS Multi-Tenant.

---

## 📊 Mapa de Agentes

```
                    ┌─────────────────────────────────────┐
                    │        🎯 MANAGER / ORCHESTRATOR     │
                    │         (claude-opus-4-6)            │
                    │              #FF6B6B                 │
                    └─────────────────────────────────────┘
                                    │
        ┌───────────────────────────┼───────────────────────────┐
        │                           │                           │
        ▼                           ▼                           ▼
┌───────────────┐           ┌───────────────┐           ┌───────────────┐
│  📋 PLANNER   │           │  🗄️ DBADMIN   │           │  🧪 TESTING   │
│  (Sonnet-4.5) │           │  (Sonnet-4.5) │           │  (Sonnet-4.5) │
│    #4ECDC4    │           │    #FFA07A    │           │    #87CEEB    │
└───────────────┘           └───────────────┘           └───────────────┘
        │                           │                           │
        ▼                           ▼                           ▼
┌───────────────┐           ┌───────────────┐           ┌───────────────┐
│   📚 DOCS     │           │   🔌 API      │           │  🚀 DEPLOY    │
│  (Haiku-4.5)  │           │  (Sonnet-4.5) │           │  (Sonnet-4.5) │
│    #45B7D1    │           │    #98D8C8    │           │    #90EE90    │
└───────────────┘           └───────────────┘           └───────────────┘
        │                           │                           │
        ▼                           ▼                           ▼
┌───────────────┐           ┌───────────────┐           ┌───────────────┐
│  🎨 UX/UI     │           │  🔒 SECURITY  │           │  🌿 GITOPS    │
│  (Sonnet-4.5) │           │  (Opus-4-6)   │           │  (Sonnet-4.5) │
│    #DDA0DD    │           │    #FF6347    │           │    #F0E68C    │
└───────────────┘           └───────────────┘           └───────────────┘
        │                           │                           │
        ▼                           ▼                           ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      ⚡ PERFORMANCE (Sonnet-4.5)                         │
│                           #FFD700                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Agente Manager / Orquestador

**Archivo:** [00-manager.md](./00-manager.md)
**Modelo:** `claude-opus-4-6`
**Color:** `#FF6B6B` (Rojo Coral)
**Comando:** `/manager`

### Responsabilidades
- Coordinación de todos los subagentes
- Gestión de sprints y roadmap
- Toma de decisiones y resolución de conflictos
- Validación de entregas y calidad

### Comandos Clave
- `/manager task list` - Lista tareas activas
- `/manager sprint status` - Estado del sprint
- `/manager validate <feature>` - Valida feature completada

---

## 📋 Agente Planner - Planificación

**Archivo:** [01-planner.md](./01-planner.md)
**Modelo:** `claude-sonnet-4-5`
**Color:** `#4ECDC4` (Turquesa)
**Comando:** `/planner`

### Responsabilidades
- Planificación de sprints
- Desglose de tareas y estimaciones
- Identificación de dependencias y riesgos
- Mantenimiento del roadmap

### Comandos Clave
- `/planner sprint plan <number>` - Planifica sprint
- `/planner task breakdown <feature>` - Desglosa feature
- `/planner dependencies <task>` - Identifica dependencias

---

## 📚 Agente Docs - Documentación

**Archivo:** [02-docs.md](./02-docs.md)
**Modelo:** `claude-haiku-4-5` → `claude-sonnet-4-5` (revisión)
**Color:** `#45B7D1` (Azul Cielo)
**Comando:** `/docs`

### Responsabilidades
- Documentación técnica y de usuario
- API docs (OpenAPI/Swagger)
- Diagramas y guías
- Changelogs y ADRs

### Comandos Clave
- `/docs api generate` - Genera doc de API
- `/docs guide create <topic>` - Crea guía
- `/docs changelog <version>` - Genera changelog

---

## 🗄️ Agente DBAdmin - Base de Datos

**Archivo:** [03-dbadmin.md](./03-dbadmin.md)
**Modelo:** `claude-sonnet-4-5`
**Color:** `#FFA07A` (Salmón)
**Comando:** `/db`

### Responsabilidades
- Diseño de schemas Neon
- Políticas RLS
- Migraciones forwards/backwards
- Optimización de queries

### MCPs
- `neon-db`: Conexión directa a Neon PostgreSQL

### Comandos Clave
- `/db schema create <model>` - Crea schema
- `/db migrate up` - Ejecuta migraciones
- `/db rls policy <table>` - Crea política RLS

---

## 🔌 Agente API - API y Contratos

**Archivo:** [04-api.md](./04-api.md)
**Modelo:** `claude-sonnet-4-5`
**Color:** `#98D8C8` (Menta)
**Comando:** `/api`

### Responsabilidades
- Diseño de endpoints REST/GraphQL
- Contratos TypeScript
- Validación con Zod
- OpenAPI specification

### Comandos Clave
- `/api endpoint create <resource>` - Crea CRUD
- `/api contract generate` - Genera tipos
- `/api openapi update` - Actualiza OpenAPI

---

## 🎨 Agente UX/UI - Diseño y Accesibilidad

**Archivo:** [05-uxui.md](./05-uxui.md)
**Modelo:** `claude-sonnet-4-5`
**Color:** `#DDA0DD` (Púrpura)
**Comando:** `/ui`

### Responsabilidades
- Componentes React reutilizables
- Design System consistente
- WCAG 2.1 AA compliance
- Responsive design

### MCPs
- `4.5v-mcp`: Análisis de imágenes de diseño

### Comandos Clave
- `/ui component create <name>` - Crea componente
- `/ui a11y audit` - Audita accesibilidad
- `/ui flow design <feature>` - Diseña flujo

---

## 🔒 Agente Security - Seguridad

**Archivo:** [06-security.md](./06-security.md)
**Modelo:** `claude-opus-4-6` (auditoría) → `claude-sonnet-4-5` (implementación)
**Color:** `#FF6347` (Tomate)
**Comando:** `/security`

### Responsabilidades
- Autenticación y autorización
- RLS validations
- OWASP Top 10 compliance
- Auditoría y logs de seguridad

### Comandos Clave
- `/security audit` - Auditoría completa
- `/security rls verify` - Verifica RLS
- `/security scan dependencies` - Escanea vulnerabilidades

---

## ⚡ Agente Performance - Optimización

**Archivo:** [07-performance.md](./07-performance.md)
**Modelo:** `claude-sonnet-4-5`
**Color:** `#FFD700` (Dorado)
**Comando:** `/perf`

### Responsabilidades
- Optimización frontend/backend
- Core Web Vitals
- Performance budgets
- Load testing

### Comandos Clave
- `/perf analyze` - Análisis completo
- `/perf query optimize <query>` - Optimiza query
- `/perf vitals check` - Check Web Vitals

---

## 🧪 Agente Testing - Testing Sintético

**Archivo:** [08-testing.md](./08-testing.md)
**Modelo:** `claude-sonnet-4-5`
**Color:** `#87CEEB` (Azul Claro)
**Comando:** `/test`

### Responsabilidades
- Usuarios sintéticos realistas
- Tests E2E, unitarios, integración
- RLS testing con usuarios
- Reports de cobertura

### Comandos Clave
- `/test user create <profile>` - Crea usuario sintético
- `/test e2e run` - Ejecuta E2E
- `/test rls verify` - Verifica RLS

---

## 🚀 Agente Deploy - Despliegue Dokploy

**Archivo:** [09-deploy.md](./09-deploy.md)
**Modelo:** `claude-sonnet-4-5`
**Color:** `#90EE90` (Verde Claro)
**Comando:** `/deploy`

### Responsabilidades
- Configuración Dokploy
- CI/CD pipelines
- Health checks
- Rollback automático

### MCPs
- `dokploy`: Gestión de despliegues

### Comandos Clave
- `/deploy setup <project>` - Configura proyecto
- `/deploy prod` - Deploy a producción
- `/deploy status` - Estado de servicios

---

## 🌿 Agente GitOps - Branching y Repos

**Archivo:** [10-gitops.md](./10-gitops.md)
**Modelo:** `claude-haiku-4-5` → `claude-sonnet-4-5` (complejo)
**Color:** `#F0E68C` (Khaki)
**Comando:** `/git`

### Responsabilidades
- Gestión de ramas
- Pull requests y code review
- Conventional commits
- Versionamiento SemVer

### MCPs
- `git-operations`: Operaciones Git

### Comandos Clave
- `/git branch create <name>` - Crea rama
- `/git pr create <title>` - Crea PR
- `/git release create <version>` - Crea release

---

## 🔗 MCPs Disponibles

### Locales (Proyecto)
| MCP | Descripción | Capabilidades |
|-----|-------------|---------------|
| `neon-db` | Conexión Neon PostgreSQL | query, schema, migrate, backup |
| `dokploy` | Gestión Dokploy | deploy, logs, config, health |
| `stripe-events` | Webhooks Stripe | events, webhooks |

### Globales
| MCP | Descripción |
|-----|-------------|
| `file-system` | Operaciones de archivo |
| `git-operations` | Operaciones Git |
| `web-reader` | Lectura de documentación web |
| `4.5v-mcp` | Análisis de imágenes |

---

## 📊 Matriz de Comandos

| Comando | Agente | Modelo | Uso |
|---------|--------|--------|-----|
| `/manager` | Manager | Opus-4.6 | Coordinación general |
| `/planner` | Planner | Sonnet-4.5 | Planificación |
| `/docs` | Docs | Haiku-4.5 | Documentación |
| `/db` | DBAdmin | Sonnet-4.5 | Base de datos |
| `/api` | API | Sonnet-4.5 | API y contratos |
| `/ui` | UXUI | Sonnet-4.5 | Diseño UI/UX |
| `/security` | Security | Opus-4.6 | Seguridad |
| `/perf` | Performance | Sonnet-4.5 | Optimización |
| `/test` | Testing | Sonnet-4.5 | Testing |
| `/deploy` | Deploy | Sonnet-4.5 | Despliegue |
| `/git` | GitOps | Sonnet-4.5 | Git y repositorio |

---

## 🎨 Colores de Agentes (Identificación Visual)

| Color | Agente | Hex |
|-------|--------|-----|
| 🔴 Rojo Coral | Manager | #FF6B6B |
| 🟢 Turquesa | Planner | #4ECDC4 |
| 🔵 Azul Cielo | Docs | #45B7D1 |
| 🟠 Salmón | DBAdmin | #FFA07A |
| 🟢 Menta | API | #98D8C8 |
| 🟣 Púrpura | UXUI | #DDA0DD |
| 🔴 Tomate | Security | #FF6347 |
| 🟡 Dorado | Performance | #FFD700 |
| 🔵 Azul Claro | Testing | #87CEEB |
| 🟢 Verde Claro | Deploy | #90EE90 |
| 🟡 Khaki | GitOps | #F0E68C |

---

## 🔄 Flujo de Trabajo

```
1. Usuario solicita tarea
       ↓
2. 🎯 Manager analiza y distribuye
       ↓
3. Subagentes especializados ejecutan
       ↓
4. Manager valida y coordina
       ↓
5. Usuario recibe reporte final
```

---

## 📁 Estructura de Archivos

```
.claude/
├── agents/
│   ├── 00-manager.md          # Manager/Orquestador
│   ├── 01-planner.md          # Planificación
│   ├── 02-docs.md             # Documentación
│   ├── 03-dbadmin.md          # Base de datos
│   ├── 04-api.md              # API y contratos
│   ├── 05-uxui.md             # UX/UI y accesibilidad
│   ├── 06-security.md         # Seguridad
│   ├── 07-performance.md      # Performance
│   ├── 08-testing.md          # Testing sintético
│   ├── 09-deploy.md           # Despliegue Dokploy
│   ├── 10-gitops.md           # Git y repositorio
│   └── system_definition.md   # Este archivo
├── settings.json              # Configuración del proyecto
└── settings.local.json        # Configuración local
```

---

## 🚀 Inicio Rápido

### Invocar un Agente Específico

```bash
# Para planificar una tarea
/planner task breakdown "Implementar OAuth2"

# Para crear un schema de DB
/db schema create User

# Para generar documentación
/docs api generate

# Para ejecutar tests
/test e2e run
```

### Flujo Típico de Feature

```bash
# 1. Manager recibe requerimiento
/manager task add "Implementar OAuth2 Google login"

# 2. Planner desglosa la tarea
/planner task breakdown "OAuth2 Google login"

# 3. DBAdmin crea schema
/db schema create oauth_accounts

# 4. API crea endpoints
/api endpoint create oauth

# 5. Security valida implementación
/security audit

# 6. Testing crea tests
/test scenario create oauth_login

# 7. Deploy despliega
/deploy staging

# 8. Docs documenta
/docs guide create oauth_login

# 9. GitOps crea PR
/git pr create "feat: add OAuth2 Google login"
```

---

## 📝 Notas Importantes

1. **Siempre pasa por el Manager** para tareas complejas que requieren múltiples agentes
2. **Usa el comando correcto** para invocar al agente especializado
3. **Los colores ayudan a identificar** qué agente está actuando en cada momento
4. **Cada agente tiene sus limitaciones** bien definidas en su archivo correspondente
5. **El settings.json** contiene toda la configuración del proyecto y agentes
