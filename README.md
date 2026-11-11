# GMP — Sistema de Gestión de Servicios

Plataforma full-stack para la gestión de servicios técnicos: administración de empresas, sedes y usuarios, registro de reportes de servicio con exportación a PDF y Excel, y encuestas de satisfacción con enlace público y código QR.

## Características

- 🏢 **Gestión de empresas y sedes** — clientes corporativos con sus puntos de trabajo.
- 👥 **Usuarios con roles** — `admin` y `tecnico`, con autenticación JWT vía Supabase Auth.
- 📋 **Reportes de servicio** — creación, consulta y exportación a **PDF** y **Excel**.
- 📊 **Dashboard administrativo** — métricas y resumen de la operación.
- 📝 **Encuestas de satisfacción** — preguntas configurables, formulario público con enlace y QR.
- 📱 **PWA instalable** — funciona en escritorio y móvil, con manifest y service worker.

## Stack Tecnológico

| Capa | Tecnologías |
|------|-------------|
| Backend | Python 3.11, FastAPI, Uvicorn, Pydantic, Supabase (PostgreSQL + Auth JWT), reportlab, openpyxl, Pillow |
| Frontend | React 18, TypeScript, Vite, Tailwind CSS, React Router 7, vite-plugin-pwa, sonner, SweetAlert2 |
| Base de datos | PostgreSQL (Supabase) con schema y migraciones SQL |
| Despliegue | Vercel (backend Python + frontend estático) |

## Estructura del Proyecto

```
Gmp/
├── backend/                  # API FastAPI
│   ├── main.py               # Punto de entrada de la app
│   ├── config.py             # Configuración y clientes Supabase
│   ├── deps.py               # Dependencias (auth, sesión)
│   ├── schemas.py            # Modelos Pydantic
│   ├── utils.py              # Utilidades (PDF, Excel)
│   ├── routers/
│   │   ├── auth.py           # Autenticación
│   │   ├── admin.py          # Administración (usuarios, sedes, empresas)
│   │   ├── catalogos.py      # Catálogos
│   │   ├── reportes.py       # Reportes de servicio
│   │   └── encuestas.py      # Encuestas de satisfacción
│   └── fonts/                # Fuentes para generación de PDF
├── frontend/                 # SPA React + TypeScript
│   ├── src/
│   │   ├── components/
│   │   │   ├── admin/        # Vistas de administración
│   │   │   ├── tecnico/      # Vista del técnico
│   │   │   └── encuesta/     # Encuesta pública y formulario
│   │   ├── contexts/         # AuthContext
│   │   ├── hooks/            # Hooks de datos (admin y técnico)
│   │   └── assets/           # Logo e iconos SVG
│   └── public/               # Iconos PWA y manifest
├── database/
│   ├── schema.sql            # Schema completo de la base de datos
│   ├── migracion.sql         # Migraciones
│   └── migracion_optimizaciones.sql
├── vercel.json               # Configuración de despliegue
└── runtime.txt               # Versión de Python para despliegue
```

## Requisitos Previos

- **Python 3.11+**
- **Node.js** (con npm)
- **Cuenta de Supabase** (proyecto con base de datos y Auth habilitados)

## Instalación y Ejecución Local

### 1. Base de datos

Ejecuta `database/schema.sql` en el **SQL Editor** de Supabase para crear las tablas, índices y triggers.

### 2. Backend

```bash
cd backend
python -m venv .venv
.venv\Scripts\activate        # Windows (source .venv/bin/activate en Linux/Mac)
pip install -r requirements.txt
```

Crea un archivo `.env` en `backend/` con las siguientes variables:

```env
SUPABASE_URL=https://<tu-proyecto>.supabase.co
SUPABASE_SERVICE_ROLE_KEY=<service-role-key>
SUPABASE_ANON_KEY=<anon-key>
FRONTEND_URL=http://localhost:5173
```

> ⚠️ Las tres variables de Supabase son obligatorias; la app no arranca sin ellas.

Levanta el servidor:

```bash
uvicorn main:app --reload
```

La API queda disponible en `http://localhost:8000` y la documentación interactiva (Swagger) en `http://localhost:8000/docs`.

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
```

La aplicación queda disponible en `http://localhost:5173`.

## API

La API se organiza en módulos (routers):

| Módulo | Descripción |
|--------|-------------|
| `auth` | Inicio de sesión y autenticación JWT |
| `admin` | Gestión de usuarios, empresas, sedes, dashboard y preguntas de encuesta |
| `catalogos` | Catálogos de datos |
| `reportes` | Reportes de servicio (CRUD, PDF, Excel) |
| `encuestas` | Encuestas de satisfacción y respuestas públicas |

Explora todos los endpoints con la documentación automática en **`/docs`** (Swagger UI) cuando el backend esté en ejecución.

## Despliegue

El proyecto está configurado para **Vercel** (`vercel.json`):

- `backend/main.py` se despliega como función Python (Python 3.11 según `runtime.txt`).
- El frontend se construye como sitio estático (`dist`).
- Las rutas `/api/*` apuntan al backend; el resto sirve la SPA (`index.html`).
- Configura las variables de entorno de Supabase en el dashboard de Vercel.

## Autor

Alejandro Álvarez Patiño — 2026
