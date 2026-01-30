# Auth0-Tech-Challenge
Auth0 Tech Challenge
# Pizzería (Next.js + Auth0 + Prisma/SQLite)

Web app de pedidos para una pizzería:

- **Catálogo público** (sin login)
- **Login con Auth0**
- **Bloqueo de pedidos si `email_verified=false`** (el usuario puede navegar, pero no comprar)
- **Persistencia con Prisma + SQLite** (historial de pedidos por usuario)

## Requisitos

- Node.js + npm
- Cuenta/tenant de Auth0 (Application tipo Regular Web App)

## Variables de entorno

Crea un archivo `.env.local` en la raíz del proyecto (`pizzeria/.env.local`) con:

```bash
# Auth0
AUTH0_DOMAIN=YOUR_TENANT.us.auth0.com
AUTH0_CLIENT_ID=...
AUTH0_CLIENT_SECRET=...
AUTH0_SECRET=...

# App
APP_BASE_URL=http://localhost:3000

# Prisma
DATABASE_URL=file:./dev.db
```

Notas:

- `APP_BASE_URL` debe ser **HTTP** en dev (si no estás sirviendo HTTPS), de lo contrario el navegador puede fallar con errores SSL.
- `AUTH0_SECRET` es un secreto para firmar la sesión de la app.

## Configuración en Auth0

En Auth0 Dashboard: **Applications → [tu app] → Settings**.

**Allowed Callback URLs**

- `http://localhost:3000/auth/callback`

**Allowed Logout URLs**

- `http://localhost:3000/`

**Allowed Web Origins**

- `http://localhost:3000`

## Cómo correr (dev)

Instala dependencias y levanta el server:

```bash
npm install
npm run dev
```

Abre:

- `http://localhost:3000`

Importante:

- Ejecuta `npm run dev` desde la carpeta `pizzeria/` (no desde la raíz del workspace) para evitar problemas de resolución de dependencias.

## Endpoints

- `GET /api/menu` (público)
- `GET /api/me` (protegido)
- `GET /api/orders` (protegido)
- `POST /api/orders` (protegido + requiere `email_verified=true`)

## Flujo de uso

- `GET /` muestra el menú
- `Ingresar` redirige a Auth0 (`/auth/login`)
- Si el email no está verificado, la UI muestra un aviso y el backend rechaza el pedido con `403 email_not_verified`
- Con email verificado, el pedido se crea y se ve en `GET /orders`

## Marketing y privacidad (nota)

Esta app persiste datos mínimos para operar y habilitar marketing básico:

- Identificador del usuario de Auth0 (**`auth0Sub`**)
- Email (si existe)
- Pedidos (items, totales y timestamps)

Recomendación:

- Define una política de privacidad y consentimiento antes de usar estos datos para campañas de marketing.
