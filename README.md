# 🚀 Template & Blueprint: Full Stack Apps en Hetzner + Coolify

Ecosistema base, estándares de arquitectura, reglas de IA y herramientas para la creación y despliegue continuo de aplicaciones web empresariales en **VPS Hetzner Cloud administrado con Coolify v4**.

---

## 🏛️ Stack Tecnológico Oficial

| Capa | Tecnología | Justificación y Reglas |
| :--- | :--- | :--- |
| **Framework Full Stack** | **Next.js 16+ (App Router)** | Server Components para consultas a BD, Server Actions para mutaciones y Turbopack para compilación rápida. `output: 'standalone'` en `next.config.ts`. |
| **Librería UI** | **React 19** | Tipado nativo y hooks modernos (`useActionState`, `useOptimistic`). |
| **Lenguaje** | **TypeScript 5 (Strict)** | Tipado de punta a punta (`end-to-end`) entre base de datos, backend y frontend. Sin uso de `any`. |
| **Base de Datos** | **PostgreSQL 16** | Nativo en contenedor Docker en desarrollo (`docker-compose.yml`), y recurso PostgreSQL administrado en Coolify en producción. |
| **ORM & Migraciones** | **Drizzle ORM & Drizzle Kit** | Máximo rendimiento, esquemas SQL declarativos en `src/db/schema.ts` y migraciones con `drizzle-kit push`. |
| **Estilos & UI** | **Tailwind CSS v4 + Lucide React** | Estética *Enterprise Dark*, paletas HSL/Zinc, glassmorphism (`backdrop-blur`), y **React Portals** (`createPortal(..., document.body)`) para todos los modales. |
| **Autenticación & RBAC** | **JWT con `jose` + `bcryptjs`** | Autenticación propia sin dependencias de servicios externos de pago. Cookies `HttpOnly`, `SameSite=Lax`, `Secure` y Middleware centralizado para control de acceso por roles. |
| **Contenerización** | **Dockerfile Multi-Stage** | Construcción ligera en 3 etapas (`deps` -> `builder` -> `runner`) basada en `node:20-alpine`, exponiendo el puerto `3000`. |
| **Despliegue & Redes** | **Coolify v4 + Traefik** | Despliegue automatizado por Git Webhook, SSL automático vía Let's Encrypt / Traefik y proxies inversos con Cloudflare DNS. |

---

## 🧩 Módulos Condicionales (Según el Rubro)

No todas las aplicaciones necesitan el stack extendido:

- 📄 **PDF Engine (`jspdf` + `jspdf-autotable`)**: Solo si el rubro exige emitir facturas, recetas médicas, cotizaciones formales o diplomas impresos.
- 🔐 **Bóveda Criptográfica (AES-256 en Node.js `crypto`)**: Solo si la app custodia contraseñas de terceros, tokens API o claves bancarias.
- 🖼️ **Procesamiento de Imágenes (`sharp`)**: Solo si hay subida de fotos, comprobantes o imágenes que deban comprimirse a formato WebP.
- 🔍 **Buscadores y Filtros Reactivos**: Integración de tablas dinámicas con filtrado instantáneo del lado del cliente vía `useMemo`.

---

## 📂 Contenido de este Repositorio

```text
├── .agents/
│   └── skills/
│       ├── new-app-scaffolding/       # Skill con el blueprint de arquitectura y protocolo de inicio
│       │   └── SKILL.md
│       └── semver-version-control/    # Skill de control de versiones SemVer (MAJOR, MINOR, PATCH)
│           ├── SKILL.md
│           └── scripts/
│               └── bump-version.js    # Script multiplataforma para versionado automático
├── templates/
│   └── RULE_TEMPLATE.md               # Plantilla de regla para colocar en .agents/rules/ de cada nuevo proyecto
└── README.md                          # Este documento
```

---

## 📋 Prompt Maestro Refinado (Listo para copiar y arrancar un nuevo proyecto)

Copia y pega este prompt al iniciar el chat con el asistente de IA para tu nueva aplicación:

```markdown
Eres un Arquitecto de Software y Desarrollador Full Stack experto. Vamos a crear desde cero una nueva aplicación web llamada "[NOMBRE_DEL_PROYECTO]" para el rubro de [RUBRO_O_ACTIVIDAD: Ej. Centro Médico / Gimnasio / Inmobiliaria / Logística].

La aplicación será desplegada en producción en mi VPS en Hetzner Cloud administrada con Coolify v4.

Debes aplicar de forma estricta nuestro Stack de Ingeniería:
1. Arquitectura: Next.js 16+ (App Router, Server Actions, Server Components, output 'standalone' en next.config.ts).
2. Lenguaje: TypeScript 5 con tipado estricto end-to-end (sin any).
3. Base de Datos & ORM: PostgreSQL 16 con Drizzle ORM y Drizzle Kit (esquemas en src/db/schema.ts).
4. Estilos & UI: Tailwind CSS v4, Lucide React Icons y estética Enterprise Dark Mode con Glassmorphism suave.
5. Modales & Diálogos: Todos los modales deben estar desacoplados mediante React Portals (createPortal(..., document.body)). Prohibido el uso de window.confirm o window.alert.
6. Autenticación & RBAC: Sistema propio con JWT firmado mediante 'jose', contraseñas hasheadas con 'bcryptjs', cookies HttpOnly y Middleware de Next.js para validación de roles ([ROLES: Ej. SuperAdmin, Secretaria, Profesional, Paciente]).
7. Despliegue: Dockerfile multi-stage basado en node:20-alpine optimizado para Coolify v4 y Traefik.

Módulos Opcionales para esta aplicación:
- Generador de PDF (jspdf + jspdf-autotable): [SI / NO]
- Bóveda de Criptografía AES-256 (crypto): [SI / NO]
- Procesamiento y compresión WebP de imágenes (sharp): [SI / NO]

Módulos y Lógica de Negocio requerida:
1. [MÓDULO 1: Ej. Agenda de Turnos con calendario interactivo y estados de atención]
2. [MÓDULO 2: Ej. Ficha de Pacientes / Clientes con historial clínico o de compras]
3. [MÓDULO 3: Ej. Finanzas e Ingresos con métodos de pago Efectivo / Transferencia]
4. [MÓDULO 4: Ej. Barra de búsqueda y filtros reactivos en tiempo real en cada tabla]

Comienza planteando el esquema de base de datos SQL con Drizzle ORM y la estructura de carpetas inicial.
```

---

## 🏷️ Control de Versiones SemVer

El repositorio incluye el script en `.agents/skills/semver-version-control/scripts/bump-version.js`:

```bash
# Ver versión actual y próximos saltos
node .agents/skills/semver-version-control/scripts/bump-version.js check

# Incrementar según el tipo de cambio
node .agents/skills/semver-version-control/scripts/bump-version.js patch   # Bug fixes / ajustes menores
node .agents/skills/semver-version-control/scripts/bump-version.js minor   # Nuevas pantallas o funcionalidades
node .agents/skills/semver-version-control/scripts/bump-version.js major   # Breaking changes
```
