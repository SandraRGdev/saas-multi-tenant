# Blueprint Base - Arquitectura SaaS Multi-Tenant

## Arquitectura General

La clave no es la vertical. La clave es la **arquitectura base común**.

```
┌─────────────────────────────────────────────────────────────┐
│                    CAPA ENTERPRISE                          │
│  SSO | API Pública | Webhooks | Logs | SLA | Backups       │
└─────────────────────────────────────────────────────────────┘
                              ▲
┌─────────────────────────────────────────────────────────────┐
│                   CAPA MULTI-VERTICAL                       │
│  eCommerce | Servicios | Inmobiliario | Restaurante        │
└─────────────────────────────────────────────────────────────┘
                              ▲
┌─────────────────────────────────────────────────────────────┐
│                      CAPA BASE (CORE)                       │
│  Auth | Tenants | Users | Dashboard | Pagos | Notificaciones│
└─────────────────────────────────────────────────────────────┘
                              ▲
┌─────────────────────────────────────────────────────────────┐
│                 INFRAESTRURA (Neon + Dokploy)               │
│              PostgreSQL con RLS | Despliegue                │
└─────────────────────────────────────────────────────────────┘
```

---

## 1️⃣ CAPA BASE (CORE SaaS)

Funcionalidades que existen para **TODOS** los tenants.

### 🔐 Autenticación y Autorización

**Funcionalidades:**
- Login / Registro
- Gestión de sesiones
- Middleware multi-tenant
- Protección por RLS en Neon

**Opciones de Auth:**
- Email/Password
- Magic Link
- OAuth (opcional)

**Roles por Tenant:**
| Rol | Permisos |
|-----|----------|
| `Owner` | Control total del tenant, facturación, eliminar tenant |
| `Admin` | Gestión de usuarios, configuración, métricas |
| `Staff` | Acceso limitado a funciones operativas |
| `Client` | Acceso solo a recursos asignados |

**Implementación técnica:**
```
┌─────────────────────────────────────────────────────────┐
│                   Auth Flow                             │
└─────────────────────────────────────────────────────────┘

1. Usuario accede a: tenant.saas.com o cliente.com
2. Middleware detecta tenant (subdominio o dominio custom)
3. Extrae tenant_id y lo inyecta en context
4. RLS filtra todas las queries por tenant_id
5. Session incluye: user_id + tenant_id + role
```

---

### 🏢 Gestión de Tenants

**Cada tenant puede:**
- Configurar nombre y branding
- Personalizar logo y colores (tema)
- Configurar dominio personalizado
- Gestionar sus propios usuarios
- Ver métricas internas

**Opciones de dominio:**
- Subdominio automático: `tenant.saas.com`
- Dominio custom: `cliente.com`

**Modelo de datos:**
```typescript
interface Tenant {
  id: string
  name: string
  slug: string           // Para subdominio
  custom_domain?: string
  branding: {
    logo_url?: string
    primary_color: string
    secondary_color: string
  }
  subscription_id: string
  plan: 'Free' | 'Pro' | 'Business' | 'Enterprise'
  limits: {
    users: number
    records: number
    api_calls: number
  }
  created_at: Date
  updated_at: Date
}
```

---

### 👥 Gestión de Usuarios

**CRUD de usuarios dentro del tenant:**
- Crear usuarios
- Listar usuarios (filtrado por tenant via RLS)
- Editar usuarios
- Eliminar usuarios (soft delete)
- Activar/Desactivar usuarios

**Sistema de invitaciones:**
```
1. Admin invita usuario → email con magic link
2. Link incluye: tenant_id + inviter_id + token
3. Usuario completa registro
4. Se le asigna rol por defecto (Client)
5. Admin puede promover rol
```

**Permisos por rol:**
```typescript
interface Permissions {
  users: 'read' | 'write' | 'delete' | 'invite'
  settings: 'read' | 'write'
  billing: 'read' | 'write'
  analytics: 'read' | 'none'
  // Permisos específicos por vertical
}
```

**Auditoría de actividad:**
- Login/Logout
- Creación/edición de usuarios
- Cambios de configuración
- Acciones sensibles

---

### 📊 Dashboard Base

**Componentes del dashboard por tenant:**

| Sección | Contenido |
|---------|-----------|
| **Resumen** | Tarjetas con KPIs principales |
| **Actividad** | Gráfico de eventos últimos 30 días |
| **Usuarios** | Total usuarios, activos, nuevos |
| **Eventos** | Timeline de eventos recientes |
| **Suscripción** | Plan actual, límites, próxima renovación |
| **Facturación** | Próximo pago, historial, método de pago |

**KPIs principales:**
- Usuarios activos (MAU, WAU, DAU)
- Registros por período
- Consumo vs límites del plan
- Revenue (para owner del tenant)

---

### 💳 Sistema de Pagos

**Integración con Stripe:**

**Planes disponibles:**
| Plan | Precio | Usuarios | Registros | Features |
|------|--------|----------|-----------|----------|
| Free | 0€ | 3 | 100 | Básico |
| Pro | 29€/mes | 10 | 1,000 | Todo lo de Free + módulos |
| Business | 99€/mes | 50 | 10,000 | Todo lo de Pro + analytics |
| Enterprise | Custom | ∞ | ∞ | Todo + SSO + API |

**Opciones de pago:**
- Suscripción mensual
- Suscripción anual (2 meses gratis)
- Pago único
- Prueba gratuita (14 días trial)
- Plan gratuito limitado

**Cada plan controla:**
- Nº máximo de usuarios
- Nº máximo de registros
- Acceso a módulos verticales
- Límites de uso (API calls, storage, etc.)

**Flujo de suscripción:**
```
1. Usuario se registra → Plan Free por defecto
2. Upgrade de plan → Redirección a Stripe Checkout
3. Stripe crea subscription y envía webhook
4. Nuestro sistema recibe webhook → Actualiza tenant
5. Límites se actualizan automáticamente
```

**Webhooks de Stripe manejados:**
- `customer.subscription.created`
- `customer.subscription.updated`
- `customer.subscription.deleted`
- `invoice.paid`
- `invoice.payment_failed`

---

### 🔔 Sistema de Notificaciones

**Tipos de notificaciones:**

| Tipo | Canal | Casos de uso |
|------|-------|--------------|
| **Email transactional** | SMTP | Bienvenida, invitaciones, reset password |
| **Notificaciones internas** | In-app | Actualizaciones, menciones |
| **Alertas de plan** | Email + In-app | Límite alcanzado, pago fallido |
| **Alertas de uso** | Email + In-app | 80%, 90%, 100% de límites |

**Implementación:**
- Cola de notificaciones (ej: BullMQ con Redis)
- Templates de email (ej: React Email)
- Preferencias de notificación por usuario

---

### 📈 Métricas y Analítica

**Eventos tracked por tenant:**
- Eventos de usuario (login, page views, acciones)
- Métricas de uso (API calls, storage, bandwidth)
- Consumo por plan
- Logs técnicos

**Analytics por tenant:**
- Dashboard con métricas en tiempo real
- Export de datos (CSV, JSON)
- Retention analysis
- Funnel analysis (para verticales)

**Logs técnicos:**
- Error logs
- Performance logs
- Security logs (intentos fallidos, etc.)

---

## 2️⃣ CAPA MULTI-VERTICAL

La base es común. Cada vertical añade módulos específicos.

### 🛒 Vertical eCommerce

**Modelos principales:**
- Productos (con variantes, inventario)
- Categorías (jerárquicas)
- Carrito de compras
- Órdenes (con estados)
- Envíos
- Cupones y descuentos
- Impuestos (por región)

**Flujo de compra:**
```
1. Usuario navega catálogo
2. Agrega productos al carrito
3. Checkout (datos de envío + pago)
4. Se crea orden con estado "pending"
5. Pago confirmado → "processing"
6. Envío preparado → "shipped"
7. Entregado → "delivered"
```

**Opciones avanzadas:**
- Variantes (talla, color, etc.)
- Descuentos por volumen
- Multi-moneda
- Analytics de ventas

---

### 🧾 Vertical SaaS Servicios

**Modelos principales:**
- Servicios (duración, precio, capacity)
- Reservas (slot de tiempo)
- Calendario de disponibilidad
- Facturación recurrente
- Clientes

**Flujo de reserva:**
```
1. Cliente ve servicios disponibles
2. Selecciona servicio y slot de tiempo
3. Completa datos y confirma
4. Se crea reserva con estado "confirmed"
5. Recordatorio automático 24h antes
6. Después del servicio → "completed"
```

**Opciones:**
- Multi-staff (cada uno con su calendario)
- Recordatorios automáticos
- Reportes financieros
- Facturas descargables

---

### 🏠 Vertical Inmobiliario

**Modelos principales:**
- Propiedades (características, ubicación, media)
- Agentes (asignados a propiedades)
- Leads (interesados)
- Reservas de visita

**Flujo de interés:**
```
1. Usuario busca propiedades con filtros
2. Ve detalles de propiedad
3. Agenda visita o solicita info
4. Se crea lead → Agente contacta
5. Si cierre → propiedad "vendida" o "alquilada"
```

**Opciones:**
- Subida de documentos
- Firma digital
- Gestión de leads
- Calendario de visitas

---

### 🍽️ Vertical Restaurante

**Modelos principales:**
- Menú (categorías, items, precios)
- Mesas (capacidad, ubicación)
- Reservas (fecha, hora, mesa)
- Órdenes internas (comanda)

**Flujo de reserva:**
```
1. Cliente ve disponibilidad
2. Selecciona fecha, hora, personas
3. Confirma reserva
4. Restaurante recibe notificación
5. Cliente llega → Se marca asistió
```

**Opciones:**
- QR menú (escanea y ve menú digital)
- Pedido online
- Integración con delivery
- Gestión de cocina (KDS)

---

## 3️⃣ CAPA ENTERPRISE

Features para tenants avanzados.

| Feature | Descripción |
|---------|-------------|
| **SSO** | Single Sign-On via SAML/OIDC |
| **Logs exportables** | Export a SIEM (Splunk, Datadog) |
| **Webhooks** | Eventos personalizados a URLs externas |
| **API pública** | API REST/GraphQL documentada |
| **Multi-tenant jerárquico** | Sub-tenants dentro de un tenant |
| **SLA dedicado** | 99.9% uptime, soporte prioritario |
| **Backups dedicados** | Backups con punto de restauración |

---

## 4️⃣ ESTRATEGIA RLS (Row Level Security)

**Política RLS en Neon:**

Todas las tablas tienen `tenant_id` como columna. RLS se activa automáticamente:

```sql
-- Política ejemplo para tabla users
CREATE POLICY tenant_isolation ON users
  USING (tenant_id = current_setting('app.current_tenant_id')::uuid);

-- Antes de cada query, se setea el tenant
SET app.current_tenant_id = 'xxx-xxx-xxx';
```

**Middleware implementation:**
```typescript
// Middleware que inyecta tenant_id
export async function setTenantContext(req: Request) {
  const tenant = await getTenantFromRequest(req)
  await sql`SET LOCAL app.current_tenant_id = ${tenant.id}`
}
```

**Tablas con RLS:**
- tenants (excepción: solo access control)
- users
- sessions
- subscriptions
- products
- orders
- (todas las tablas de verticales)

---

## 5️⃣ MODELO DE MONETIZACIÓN

**Estrategia de precios:**

```
Base SaaS → Suscripción mensual/anual
Vertical modules → Add-ons (+€/mes)
Enterprise features → Plan Enterprise
```

**Ejemplo de pricing:**
| Componente | Precio |
|------------|--------|
| Base Pro | 29€/mes |
| eCommerce module | +19€/mes |
| Inmobiliario module | +25€/mes |
| Restaurante module | +15€/mes |
| Servicios module | +20€/mes |

**Revenue máximo por tenant:**
- Base Pro + 4 verticales = 29 + 19 + 25 + 15 + 20 = **108€/mes**

---

## 6️⃣ DIFERENCIADORES CLAVE

| Feature | Nuestro SaaS |
|---------|--------------|
| **Multi-tenant real** | RLS a nivel base de datos |
| **Fullstack moderno** | React/Next.js + TypeScript |
| **Modular por vertical** | Arquitectura de plugins |
| **Agentes IA** | Integrados en desarrollo |
| **Escalable** | Neon + Dokploy |
| **Monorepo limpio** | Turborepo + npm workspaces |

---

## 7️⃣ FUTURAS OPCIONES

- Marketplace de plugins (terceros)
- IA para análisis predictivo
- Automatizaciones (Zapier-like)
- Exportaciones avanzadas (PDF reports)
- Mobile app companion
- White-label para agencias
