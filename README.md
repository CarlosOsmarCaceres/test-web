# Test Demo Web · Automatización E2E

Suite de pruebas end-to-end con **Playwright** y **TypeScript**, pensada como **portfolio técnico**: muestra cómo estructuro tests web reutilizables, reportes y entornos sin acoplar el código a selectores frágiles.

[![Node.js](https://img.shields.io/badge/node.js-%3E%3D18-339933?logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Playwright](https://img.shields.io/badge/Playwright-1.58-2EAD33?logo=playwright&logoColor=white)](https://playwright.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg)](https://opensource.org/licenses/ISC)

**Aplicaciones bajo prueba:** [MercadoLibre](https://www.mercadolibre.com/) (flujos de búsqueda, carrito y categorías) y **OrangeHRM** (login, accesibilidad por teclado y módulo Admin).

---

## Lo que demuestra este repo (para leads y reclutadores)

En menos de un minuto puedes ver:

| Enfoque | Qué hay en el código |
|--------|----------------------|
| **Mantenibilidad** | Page Object Model (POM) en `pages/` |
| **Escalabilidad** | Proyectos separados en Playwright (`MercadoLibre` / `OrangeHRM`) |
| **Cobertura funcional** | Casos positivos, negativos y data-driven en login |
| **Observabilidad** | Reportes HTML + Allure, trace/screenshot/video en fallos |
| **Configuración** | Variables de entorno con `.env` (sin hardcodear secretos reales) |

---

## Inicio rápido

```bash
git clone <tu-repo>
cd test-demo-web
npm install
npx playwright install
npm test
```

Reporte HTML después de correr tests:

```bash
npm run report
```

---

## Tabla de contenidos

- [Stack tecnológico](#stack-tecnológico)
- [Requisitos previos](#requisitos-previos)
- [Instalación](#instalación)
- [Credenciales de OrangeHRM (demo pública)](#credenciales-de-orangehrm-demo-pública)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Configuración de Playwright](#configuración-de-playwright)
- [Variables de entorno](#variables-de-entorno)
- [Comandos útiles](#comandos-útiles)
- [Casos cubiertos](#casos-cubiertos-resumen)
- [Buenas prácticas implementadas](#buenas-prácticas-implementadas)

---

## Stack tecnológico

| Herramienta | Rol en el proyecto |
|-------------|-------------------|
| **Node.js + npm** | Runtime y dependencias |
| **TypeScript** | Tipado y mantenibilidad |
| **Playwright Test** | Motor E2E, proyectos y reporters |
| **Allure** | Reportes complementarios |
| **dotenv** | Carga de `.env` en local |

---

## Requisitos previos

- **Node.js 18+**
- **npm** (incluido con Node.js)

---

## Instalación

```bash
npm install
npx playwright install
```

---

## Credenciales de OrangeHRM (demo pública)

Los valores por defecto **`Admin` / `admin123`** son las credenciales típicas del **entorno de demostración público** de OrangeHRM. Están en el código y en el ejemplo de `.env` **a propósito**, para que cualquiera pueda clonar el repo y ejecutar los tests sin configuración extra.

> **Importante:** no son secretos de producción. En un proyecto real, las credenciales reales irían solo en variables de entorno, secretos de CI o un vault; nunca commiteadas.

---

## Estructura del proyecto

```text
.
├── pages/
│   ├── MercadoLibrePage.ts    # Page Object MercadoLibre
│   ├── OrangeLoginPage.ts     # Page Object login OrangeHRM
│   └── OrangeAdminPage.ts     # Page Object módulo Admin OrangeHRM
├── tests/
│   ├── mercado-libre/
│   │   └── ml.spec.ts         # Suite MercadoLibre (TC01–TC04)
│   └── orange-hrm/
│       ├── orange-hrm.spec.ts # Suite OrangeHRM (login + admin)
│       ├── test1.login.spec.ts
│       └── test2.admin.spec.ts
├── utils/
│   └── constants.ts           # URLs base y timeouts reutilizables
├── screenshots/               # Evidencias generadas por pruebas
├── playwright.config.ts       # Projects + reporters
├── tsconfig.json              # Paths @pages/* y @utils/*
├── package.json
└── .gitignore
```

`test1.login.spec.ts` y `test2.admin.spec.ts` están vacíos a propósito para no duplicar ejecuciones; la suite activa de OrangeHRM vive en `orange-hrm.spec.ts`.

---

## Configuración de Playwright

- **Projects:** `MercadoLibre` (`tests/mercado-libre`) y `OrangeHRM` (`tests/orange-hrm`), cada uno con su `baseURL`.
- **Reporters:** HTML (`playwright-report`), Allure (`allure-results`), list en terminal.
- **Evidencias en fallo:** `trace: on-first-retry`, `screenshot: only-on-failure`, `video: retain-on-failure`.

---

## Variables de entorno

Opcional: archivo `.env` en la raíz para sobrescribir usuario y clave de OrangeHRM (útil si cambiás de instancia demo).

```env
ORANGE_USER=Admin
ORANGE_PASS=admin123
```

Si no definís estas variables, los tests usan los mismos valores por defecto en código (siempre credenciales de **demo pública**, ver sección anterior).

---

## Comandos útiles

### Ejecución de pruebas

| Objetivo | Comando |
|----------|---------|
| Toda la suite | `npm test` |
| Navegador visible | `npm run test:headed` |
| UI runner de Playwright | `npm run test:ui` |
| Solo MercadoLibre | `npx playwright test --project=MercadoLibre` |
| Solo OrangeHRM | `npx playwright test --project=OrangeHRM` |

Por archivo:

```bash
npx playwright test tests/mercado-libre/ml.spec.ts
npx playwright test tests/orange-hrm/orange-hrm.spec.ts
```

Por tags:

```bash
npx playwright test --grep @ml
npx playwright test --grep @orange
npx playwright test --grep @screen
npx playwright test --grep @test
```

### Reportes

| Objetivo | Comando |
|----------|---------|
| Abrir reporte HTML Playwright | `npm run report` |
| Generar Allure | `npm run allure:generate` |
| Abrir Allure | `npm run allure:open` |

---

## Casos cubiertos (resumen)

**MercadoLibre**

- Búsqueda de producto.
- Selección de primer resultado e intento de compra.
- Agregado al carrito.
- Navegación por categorías hasta tienda Samsung.

**OrangeHRM**

- Login exitoso.
- Login por teclado (accesibilidad).
- Recuperación de contraseña.
- Login negativo (escenarios data-driven).
- Módulo Admin: cambio de estado, filtro y eliminación de usuario.

---

## Buenas prácticas implementadas

- **POM** para separar tests de selectores y flujos de página.
- Constantes y timeouts centralizados en `utils/constants.ts`.
- Evidencias automáticas (screenshot, video, trace) cuando falla un caso.
- Credenciales configurables por entorno vía `.env`, documentando que los valores por defecto son solo para demo pública.
