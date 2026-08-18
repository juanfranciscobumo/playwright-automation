# Playwright Automation

Framework de automatización de pruebas end-to-end y API con Playwright y TypeScript.

## Requisitos previos

- Node.js >= 18
- npm o yarn

## Instalación

```bash
npm install
npx playwright install
```

## Scripts disponibles

| Comando | Descripción |
|---------|-------------|
| `npm test` | Ejecuta todos los tests |
| `npm run test:headed` | Ejecuta tests con navegador visible |
| `npm run test:ui` | Abre la UI de Playwright |
| `npm run test:debug` | Ejecuta tests en modo debug |
| `npm run test:api` | Ejecuta solo tests de API |
| `npm run report` | Abre el reporte HTML |

## Estructura del proyecto

```
├── fixtures/
│   └── test-data.ts       # Datos de prueba y fixtures
├── pages/
│   └── BasePage.ts        # Page Object base
├── utils/
│   └── api-client.ts      # Cliente API utility
├── tests/
│   └── api/               # Tests de API
│       ├── posts.spec.ts
│       ├── users.spec.ts
│       └── comments.spec.ts
└── playwright.config.ts
```

## Custom Fixtures y Helpers

### apiHelpers
Fixture personalizado para peticiones API:

```typescript
test("GET posts", async ({ apiHelpers }) => {
  const { status, data } = await apiHelpers.get<Post[]>("/posts");
  expect(status).toBe(200);
});
```

### Métodos disponibles
- `apiHelpers.get<T>(endpoint)` - Petición GET
- `apiHelpers.post<T>(endpoint, body)` - Petición POST
- `apiHelpers.put<T>(endpoint, body)` - Petición PUT
- `apiHelpers.delete<T>(endpoint)` - Petición DELETE
- `apiHelpers.patch<T>(endpoint, body)` - Petición PATCH

### testData
Datos de prueba centralizados:

```typescript
import { testData } from "../fixtures/test-data";

test("ejemplo", async ({ apiHelpers }) => {
  const { data } = await apiHelpers.get(testData.endpoints.posts);
});
```

## Ejemplo de uso

```typescript
import { test, expect, testData } from "../fixtures/test-data";

interface Post {
  id: number;
  title: string;
  body: string;
  userId: number;
}

test("GET - Debería obtener posts", async ({ apiHelpers }) => {
  const { status, data } = await apiHelpers.get<Post[]>(testData.endpoints.posts);
  
  expect(status).toBe(200);
  expect(data).toBeInstanceOf(Array);
});

test("POST - Debería crear post", async ({ apiHelpers }) => {
  const { status, data } = await apiHelpers.post<Post>(
    testData.endpoints.posts,
    testData.posts.newPost
  );
  
  expect(status).toBe(201);
  expect(data).toHaveProperty("id");
});
```

## API Under Test

Se utiliza [JSONPlaceholder](https://jsonplaceholder.typicode.com) como API de prueba.

### Base URL
```
https://jsonplaceholder.typicode.com
```

### Endpoints disponibles

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/posts` | Obtener todos los posts |
| GET | `/posts/:id` | Obtener un post por ID |
| GET | `/posts?userId=:id` | Obtener posts por usuario |
| POST | `/posts` | Crear un nuevo post |
| PUT | `/posts/:id` | Actualizar un post |
| DELETE | `/posts/:id` | Eliminar un post |
| GET | `/users` | Obtener todos los usuarios |
| GET | `/users/:id` | Obtener un usuario por ID |
| GET | `/users/:id/posts` | Obtener posts de un usuario |
| GET | `/comments` | Obtener todos los comentarios |
| GET | `/comments?postId=:id` | Obtener comentarios por post |
| GET | `/albums` | Obtener todos los álbumes |
| GET | `/todos` | Obtener todas las tareas |

### Headers
```json
{
  "Accept": "application/json",
  "Content-Type": "application/json"
}
```

## Configuración

La configuración principal se encuentra en `playwright.config.ts`:

- **baseUrl**: https://jsonplaceholder.typicode.com
- **Browsers**: Chromium, Firefox, WebKit, Mobile Chrome, Mobile Safari
- **Reporters**: HTML, List

## Tecnologías

- Playwright 1.40+
- TypeScript 5
