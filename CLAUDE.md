# airgap-front

Frontend de AirGap: Angular 22, standalone, signals, Vitest, Tailwind 4. Diseño en `../airgap-docs` (empezar por `02-arquitectura/frontend.md`). Reglas transversales en `../airgap-docs/CLAUDE.md`.

## Estructura (`src/app/`)

- `core/` — singletons: `auth/{guards,services,state}`, `http/interceptors`, `api` (un servicio por área), `config`, `layout`, `models`.
- `features/<área>/` — `auth`, `teams`, `projects`, `secrets`, `members`, `invitations`, `audit`, `account`. Cada una con rutas lazy (`<área>.routes.ts`) y `pages/`, `components/`, `services/`, `models/`.
- `shared/` — `ui`, `validators`, `pipes`, `directives`, `models`, `utils`. Sin lógica de negocio.

Todo en inglés (carpetas, archivos, identificadores). Rama de trabajo: `develop`.

Dependencias permitidas: `features → core, shared`; `core → shared`; `shared → nada` de la app. Una feature no importa de otra.

## Reglas de código

- Componentes standalone con `OnPush`; `inject()`; `input()`/`output()`; control de flujo `@if`/`@for`.
- Estado con signals; RxJS solo donde aporte (HTTP, eventos).
- Formularios reactivos tipados; validadores en `shared/validators`.
- Toda pantalla maneja estados de carga, vacío y error.
- Guards solo para UX; la autorización real es de B. No asumir permisos solo por el rol mostrado.
- Access token **solo en memoria**: nunca `localStorage`, `sessionStorage` ni IndexedDB. Un único refresh en vuelo compartido entre peticiones concurrentes.
- Valores revelados: ocultar tras un tiempo, no registrar en consola, no guardar en estado persistente. Copia al portapapeles con limpieza de mejor esfuerzo.
- Sin `innerHTML` ni `bypassSecurityTrust*` salvo justificación documentada.
- Accesibilidad: etiquetas, teclado, contraste, responsive.
- Peticiones a `/api/*` en el mismo origen (en desarrollo, proxy del dev server hacia A).
- TypeScript estricto; sin `any` sin justificación.

## Pruebas

Vitest. Cubrir servicios, stores, interceptor (incluido refresh concurrente), guards y componentes críticos (login, formulario de secreto, revelar, visibilidad por rol).

## Comandos

Gestor: `pnpm`. `pnpm start` (proxy de `/api` → `localhost:3000`), `pnpm test`, `pnpm lint`, `pnpm build`. Prettier: `printWidth` 100, comillas simples. Instalados: `angular-eslint`, `@angular/cdk`.
