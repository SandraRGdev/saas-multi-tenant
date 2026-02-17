# Project Roadmap

## Roadmap del Proyecto SaaS Multi-Tenant

## Versionado SemVer

```
MAJOR.MINOR.PATCH
- MAJOR: Cambios breaking / Enterprise
- MINOR: Nuevas features / Verticales
- PATCH: Bug fixes / Mejoras
```

## Sprint Planning

---

### Sprint 0: v0.1.0 - Foundation
**Rama:** `sprint/0-foundation`

#### Tasks
- [ ] Crear estructura del monorepo
- [ ] Configurar ESLint, Prettier, TypeScript
- [ ] Configurar Neon database con RLS base
- [ ] Setup Dokploy para despliegue
- [ ] Configurar CI/CD base

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing con usuarios sintéticos OK
- [ ] Release v0.1.0 creada
- [ ] Tag v0.1.0 creado
- [ ] Merge a `main` completado

---

### Sprint 1: v0.2.0 - Auth Multi-Tenant
**Rama:** `sprint/1-auth`

#### Tasks
- [ ] Implementar autenticación (email/password + magic link)
- [ ] Crear modelo de Tenants con subdominio
- [ ] Implementar RLS por tenant
- [ ] Middleware de detección de tenant
- [ ] Roles base (Owner, Admin, Staff, Client)

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing con múltiples tenants OK
- [ ] Release v0.2.0 creada
- [ ] Tag v0.2.0 creado
- [ ] Merge a `main` completado

---

### Sprint 2: v0.3.0 - Users & Tenants Management
**Rama:** `sprint/2-users-tenants`

#### Tasks
- [ ] CRUD completo de usuarios por tenant
- [ ] Sistema de invitaciones por email
- [ ] Gestión de permisos por rol
- [ ] Auditoría de actividad (logs)
- [ ] Dashboard base para tenant admin

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing de gestión de usuarios OK
- [ ] Release v0.3.0 creada
- [ ] Tag v0.3.0 creado
- [ ] Merge a `main` completado

---

### Sprint 3: v0.4.0 - Stripe Integration
**Rama:** `sprint/3-stripe`

#### Tasks
- [ ] Integración Stripe SDK
- [ ] Modelo de planes (Free, Pro, Business, Enterprise)
- [ ] Webhooks de Stripe
- [ ] Gestión de suscripciones
- [ ] Límites de uso por plan

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing de pagos OK (modo test)
- [ ] Release v0.4.0 creada
- [ ] Tag v0.4.0 creado
- [ ] Merge a `main` completado

---

### Sprint 4: v0.5.0 - Base Dashboard & Analytics
**Rama:** `sprint/4-dashboard`

#### Tasks
- [ ] Dashboard principal con KPIs
- [ ] Métricas de uso por tenant
- [ ] Gráficos de actividad
- [ ] Event tracking
- [ ] Export de datos básico

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing de dashboards OK
- [ ] Release v0.5.0 creada
- [ ] Tag v0.5.0 creado
- [ ] Merge a `main` completado

---

### Sprint 5: v1.0.0 - Core SaaS Release
**Rama:** `sprint/5-core-release`

#### Tasks
- [ ] Unificar todos los módulos core
- [ ] Testing end-to-end completo
- [ ] Documentación de API core
- [ ] Optimización de performance
- [ ] Preparación para verticales

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing E2E OK
- [ ] Release v1.0.0 creada
- [ ] Tag v1.0.0 creado
- [ ] Merge a `main` completado

---

### Sprint 6: v1.1.0 - eCommerce Module
**Rama:** `sprint/6-ecommerce`

#### Tasks
- [ ] Modelo de Productos
- [ ] Categorías y variantes
- [ ] Carrito de compras
- [ ] Checkout process
- [ ] Gestión de órdenes

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing eCommerce OK
- [ ] Release v1.1.0 creada
- [ ] Tag v1.1.0 creado
- [ ] Merge a `main` completado

---

### Sprint 7: v1.2.0 - Services SaaS Module
**Rama:** `sprint/7-services`

#### Tasks
- [ ] Modelo de Servicios
- [ ] Sistema de reservas
- [ ] Calendario de disponibilidad
- [ ] Facturación recurrente
- [ ] Panel de administración

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing de servicios OK
- [ ] Release v1.2.0 creada
- [ ] Tag v1.2.0 creado
- [ ] Merge a `main` completado

---

### Sprint 8: v1.3.0 - Real Estate Module
**Rama:** `sprint/8-realestate`

#### Tasks
- [ ] Modelo de Propiedades
- [ ] Filtros avanzados
- [ ] Gestión de agentes
- [ ] Reservas de visita
- [ ] Documentos y firma digital

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing inmobiliario OK
- [ ] Release v1.3.0 creada
- [ ] Tag v1.3.0 creado
- [ ] Merge a `main` completado

---

### Sprint 9: v1.4.0 - Restaurant Module
**Rama:** `sprint/9-restaurant`

#### Tasks
- [ ] Modelo de Menú
- [ ] Gestión de mesas
- [ ] Sistema de reservas
- [ ] Órdenes internas
- [ ] QR menú

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing restaurante OK
- [ ] Release v1.4.0 creada
- [ ] Tag v1.4.0 creado
- [ ] Merge a `main` completado

---

### Sprint 10: v2.0.0 - Enterprise Features
**Rama:** `sprint/10-enterprise`

#### Tasks
- [ ] SSO (SAML/OIDC)
- [ ] API pública documentada
- [ ] Webhooks system
- [ ] Logs exportables
- [ ] SLA y backups dedicados

#### Definition of Done
- [ ] Código en rama `develop`
- [ ] Testing enterprise OK
- [ ] Release v2.0.0 creada
- [ ] Tag v2.0.0 creado
- [ ] Merge a `main` completado

---

## Resumen de Versiones

| Versión | Sprint | Enfoque | Estado |
|---------|--------|---------|--------|
| v0.1.0 | Sprint 0 | Foundation | Pending |
| v0.2.0 | Sprint 1 | Auth Multi-Tenant | Pending |
| v0.3.0 | Sprint 2 | Users & Tenants | Pending |
| v0.4.0 | Sprint 3 | Stripe Integration | Pending |
| v0.5.0 | Sprint 4 | Dashboard & Analytics | Pending |
| v1.0.0 | Sprint 5 | Core SaaS Release | Pending |
| v1.1.0 | Sprint 6 | eCommerce Module | Pending |
| v1.2.0 | Sprint 7 | Services Module | Pending |
| v1.3.0 | Sprint 8 | Real Estate Module | Pending |
| v1.4.0 | Sprint 9 | Restaurant Module | Pending |
| v2.0.0 | Sprint 10 | Enterprise Features | Pending |
