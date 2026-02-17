# Git Strategy

## Estrategia de Branches y Workflow

## Branch Strategy

```
main (producción)
│
├── develop (integración)
│   ├── sprint/0-foundation
│   ├── sprint/1-auth
│   ├── sprint/2-users-tenants
│   ├── sprint/3-stripe
│   ├── sprint/4-dashboard
│   ├── sprint/5-core-release
│   ├── sprint/6-ecommerce
│   ├── sprint/7-services
│   ├── sprint/8-realestate
│   ├── sprint/9-restaurant
│   ├── sprint/10-enterprise
│   │
│   ├── feature/* (features específicas)
│   ├── bugfix/* (fixes de bugs)
│   └── hotfix/* (urgentes en producción)
```

## Workflow por Sprint

```
┌─────────────────────────────────────────────────────────────────┐
│                     WORKFLOW POR SPRINT                          │
└─────────────────────────────────────────────────────────────────┘

1. CREAR RAMA DEL SPRINT
   git checkout -b sprint/X-nombre-descriptivo

2. DESARROLLO
   ├─ Commit y push en la rama del sprint
   ├─ Las tasks se marcan cuando el Agente de Planificación confirma
   └─ El Agente de Testing valida con usuarios/datos sintéticos

3. PUSH A DEVELOP (cuando el sprint está completo)
   git checkout develop
   git merge sprint/X-nombre-descriptivo
   git push origin develop

4. TESTING EN DEVELOP
   ├─ Sub-Agente de Testing ejecuta pruebas
   ├─ Se usan usuarios/datos sintéticos (docs/users/synthetic_users.json)
   └─ Validación de funcionalidad

5. CREAR RELEASE Y TAG
   gh release create vX.Y.Z \
     --title "vX.Y.Z - Sprint X: Nombre" \
     --notes "Descripción del release..."
   git tag -a vX.Y.Z -m "vX.Y.Z - Sprint X completado"
   git push origin vX.Y.Z

6. MERGE A MAIN
   git checkout main
   git merge develop
   git push origin main

7. LIMPIEZA
   git branch -d sprint/X-nombre-descriptivo
```

## Convención de Commits

```
<tipo>(<alcance>): <descripción>

[opcional: cuerpo]

[opcional: footer]
```

### Tipos

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `feat` | Nueva feature | `feat(auth): agregar magic link` |
| `fix` | Bug fix | `fix(tenant): resolver RLS filter` |
| `docs` | Documentación | `docs(readme): actualizar instrucciones` |
| `style` | Formato/estilo | `style(lint): corregir indentación` |
| `refactor` | Refactorización | `refactor(database): optimizar queries` |
| `test` | Tests | `test(stripe): mocks de webhooks` |
| `chore` | Mantenimiento | `chore(deps): actualizar dependencias` |

### Alcances Comunes

- `auth` - Autenticación y autorización
- `tenant` - Gestión de tenants
- `user` - Gestión de usuarios
- `stripe` - Pagos y suscripciones
- `database` - Esquema y queries
- `api` - Endpoints y middleware
- `dashboard` - UI de dashboard
- `ecommerce` - Módulo eCommerce
- `services` - Módulo Servicios
- `realestate` - Módulo Inmobiliario
- `restaurant` - Módulo Restaurante
- `enterprise` - Features enterprise

## Pull Request Template

```markdown
## Descripción
<!-- Breve descripción del cambio -->

## Tipo de Cambio
- [ ] Feature (nueva funcionalidad)
- [ ] Bug fix (corrección)
- [ ] Refactor (reestructuración)
- [ ] Docs (documentación)
- [ ] Tests (pruebas)

## Sprint Relacionado
Sprint X: vX.Y.Z

## Checklist
- [ ] Código sigue las convenciones de estilo
- [ ] Tests agregados/actualizados
- [ ] Documentación actualizada
- [ ] Todos los checks del CI pasan
- [ ] Agente de Planificación confirmó las tasks

## Testing
<!-- Cómo se probó este cambio -->

## Capturas (si aplica)
<!-- Agregar capturas para cambios visuales -->
```

## Rules principales

1. **MAIN** es inmutable excepto por merges desde develop aprobados
2. **DEVELOP** es la rama de integración y testing
3. **SPRINT branches** se crean desde develop y mergean de vuelta a develop
4. **FEATURE branches** (opcionales) para tareas aisladas dentro de un sprint
5. **HOTFIX branches** se crean desde main y mergean tanto a main como develop
6. **TAGS** se crean SEMPRE desde develop, antes del merge a main
7. **RELEASES** se crean junto con los tags

## Coordenadas de Agents

| Agent | Responsabilidad |
|-------|-----------------|
| **Agente de Planificación** | Confirma que cada task está completada antes de marcar el check |
| **Sub-Agente de Testing** | Ejecuta pruebas en develop con datos sintéticos |
| **Agente de Release** | Crea releases y tags cuando el sprint pasa testing |

## Comandos Útiles

```bash
# Crear nueva rama de sprint
git checkout -b sprint/X-nombre develop

# Ver estado de los checks del sprint
# (los checks están en docs/planning/project_roadmap.md)

# Crear release y tag
gh release create vX.Y.Z --notes-file RELEASE_NOTES.md
git tag -a vX.Y.Z -m "Release vX.Y.Z"
git push origin vX.Y.Z

# Merge a main (solo después de release)
git checkout main && git merge develop --no-ff
```
