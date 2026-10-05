# Blueprint Arquitectónico Global (ARCHITECT.md)

## 1. Visión General del Sistema
- **Nombre del Proyecto:** [Nombre de la App]
- **Propósito:** [Descripción corta de qué hace la aplicación en general]
- **Patrón de Arquitectura:** [Ej: Arquitectura Limpia, Monolito Modular, MVC, DDD, etc.]

## 2. Stack Tecnológico Autorizado
El agente de IA no debe utilizar herramientas, frameworks o librerías fuera de este listado:
- **Frontend / UI:** [Ej: React 19, Next.js, Tailwind CSS]
- **Backend / API:** [Ej: Node.js, Express, FastAPI, Go standard library]
- **Base de Datos / Persistencia:** [Ej: PostgreSQL con Prisma ORM, MongoDB, SQLite local]
- **Gestión de Estado / Caché:** [Ej: Zustand, Redis, Context API]

## 3. Estructura Global de Carpetas
[Describe cómo debe organizarse el código para que la IA cree los nuevos archivos en el lugar correcto]
```text
mi-proyecto/
├── src/
│   ├── config/       # Variables de entorno y setups iniciales
│   ├── modules/      # Módulos encapsulados por dominio de negocio
│   ├── shared/       # Componentes, utilidades o tipos reutilizables
└── specs/            # Carpetas del flujo SDD
```

## 4. Convenciones de Código y Estándares de Ingeniería
Directrices técnicas de alto nivel para mantener la consistencia en todo el repositorio:
- **Nomenclatura:** [Ej: camelCase para variables/funciones, PascalCase para clases/componentes, snake_case para base de datos]
- **Paradigma:** [Ej: Programación Funcional estricta, Orientada a Objetos, etc.]
- **Estrategia Git:** [Ej: Commits usando Conventional Commits (feat:, fix:, docs:)]
- **Seguridad:** [Ej: Nunca guardar credenciales en código, usar siempre variables de entorno (.env). Validar y sanitizar todas las entradas de usuario].

## 5. Estado Actual del Sistema y Roadmap
[Un mapa en alta definición de qué módulos ya existen y cuáles están pendientes, para que la IA entienda el progreso del software]
- [x] Inicialización del repositorio y entornos de desarrollo.
- [ ] Módulo de Autenticación (Pendiente).
- [ ] Módulo de Usuarios (Pendiente).
