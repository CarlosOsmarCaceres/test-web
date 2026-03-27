## Proyecto QA Automation - Test Demo Web

Proyecto de automatización end-to-end con **Playwright + TypeScript**, organizado con **Page Object Model (POM)** para pruebas web sobre:

- **MercadoLibre** (flujos de búsqueda, compra y categorías)
- **OrangeHRM** (login y gestión de usuarios en módulo Admin)

## Tecnologías y framework

- **Node.js + npm** para ejecución y gestión de dependencias.
- **TypeScript** para tipado estático y mejor mantenibilidad.
- **Playwright Test** como framework principal de automatización E2E.
- **Allure** como reporter complementario para reportes avanzados.
- **dotenv** para cargar variables de entorno (`.env`).

## Requisitos previos

- Node.js 18 o superior.
- npm (incluido con Node.js).

## Instalación

```bash
npm install
npx playwright install
```

## Estructura del proyecto

```text
.
├── pages/
│   ├── MercadoLibrePage.ts    # Page Object de MercadoLibre
│   ├── OrangeLoginPage.ts     # Page Object de login OrangeHRM
│   └── OrangeAdminPage.ts     # Page Object del módulo Admin OrangeHRM
├── tests/
│   ├── mercado-libre/
│   │   └── ml.spec.ts         # Suite MercadoLibre (TC01-TC04)
│   └── orange-hrm/
│       ├── orange-hrm.spec.ts # Suite OrangeHRM (login + admin)
│       ├── test1.login.spec.ts
│       └── test2.admin.spec.ts
├── utils/
│   └── constants.ts           # URLs base y timeouts reutilizables
├── screenshots/               # Evidencias guardadas por pruebas
├── playwright.config.ts       # Configuración de Playwright (projects + reporters)
├── tsconfig.json              # Configuración TypeScript (paths @pages/* y @utils/*)
├── package.json               # Scripts npm y dependencias
└── .gitignore
```

> `test1.login.spec.ts` y `test2.admin.spec.ts` están vacíos a propósito para evitar duplicar ejecuciones; los tests activos están centralizados en `orange-hrm.spec.ts`.

## Configuración actual de Playwright

- **Projects**:
  - `MercadoLibre` (`tests/mercado-libre`, baseURL MercadoLibre)
  - `OrangeHRM` (`tests/orange-hrm`, baseURL OrangeHRM)
- **Reporters**:
  - HTML (`playwright-report`)
  - Allure (`allure-results`)
  - list (salida en terminal)
- **Evidencias**:
  - `trace: on-first-retry`
  - `screenshot: only-on-failure`
  - `video: retain-on-failure`

## Variables de entorno

Opcionalmente puedes crear un archivo `.env` en la raíz para credenciales de OrangeHRM:

```env
ORANGE_USER=Admin
ORANGE_PASS=admin123
```

Si no existen, los tests usan los valores por defecto definidos en el código.

## Comandos útiles de terminal

### Ejecución de pruebas

- Ejecutar toda la suite:

```bash
npm test
```

- Ejecutar en modo headed (navegador visible):

```bash
npm run test:headed
```

- Abrir la UI runner de Playwright:

```bash
npm run test:ui
```

- Ejecutar solo proyecto MercadoLibre:

```bash
npx playwright test --project=MercadoLibre
```

- Ejecutar solo proyecto OrangeHRM:

```bash
npx playwright test --project=OrangeHRM
```

- Ejecutar por archivo:

```bash
npx playwright test tests/mercado-libre/ml.spec.ts
npx playwright test tests/orange-hrm/orange-hrm.spec.ts
```

- Ejecutar por tags (`grep`):

```bash
npx playwright test --grep @ml
npx playwright test --grep @orange
npx playwright test --grep @screen
npx playwright test --grep @test
```

### Reportes

- Abrir reporte HTML de Playwright:

```bash
npm run report
```

- Generar reporte Allure:

```bash
npm run allure:generate
```

- Abrir reporte Allure:

```bash
npm run allure:open
```

## Casos cubiertos (resumen)

- **MercadoLibre**:
  - Búsqueda de producto.
  - Selección de primer resultado e intento de compra.
  - Agregado al carrito.
  - Navegación por categorías hasta tienda Samsung.

- **OrangeHRM**:
  - Login exitoso.
  - Login por teclado (accesibilidad).
  - Recuperación de contraseña.
  - Casos negativos de login (data-driven).
  - Módulo Admin: cambio de estado, filtro y eliminación de usuario.

## Buenas prácticas implementadas

- Uso de **POM** para desacoplar tests de selectores.
- Reutilización de constantes y timeouts en `utils/constants.ts`.
- Evidencias automáticas con screenshot/video/trace en escenarios de falla.
- Soporte de credenciales por entorno con `.env`.