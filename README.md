# API Data Processor

> Production-ready REST API that consumes external data sources, validates CSV files, and exposes clean, paginated endpoints. Built with Node.js.

## What it solves

You have data coming from an external API in messy CSV format. You need a reliable, queryable interface to consume it without dealing with parsing errors, missing fields, or inconsistent formatting.

This service handles the complexity so your applications get clean, validated data.

## Key features

- **External API consumption** – Pulls data from public APIs, handles failures gracefully
- **CSV validation** – Validates hex codes (32 chars) and numeric fields, filters malformed rows
- **Flexible filtering** – Option to include partially valid rows (`includeEmpty=true`)
- **Pagination** – `limit` and `offset` parameters for large datasets
- **File listing** – Get available files from the source API
- **Consistent error responses** – Standard JSON error format across all endpoints

## Quick start

```bash
git clone https://github.com/cpineda1985/api-data-processor
cd api-data-processor
npm install
npm start
```
---
#### Endpoints

#### GET `/files/list`
- Devuelve la lista cruda de archivos disponible desde el API externo.
- Soporta filtro por nombre usando `?fileName=`.
- En caso de no encontrarse, responde con `404` en formato estándar.

---

## Manejo de errores

- Los errores del API externo o fallas por archivo se manejan sin interrumpir el procesamiento completo.
- La respuesta siempre es en formato JSON.
- Se loguean errores en consola para facilitar debugging.
- Todos los errores siguen un esquema común con `code`, `message`, `details` y `status`.

---

## Validaciones del CSV

- Se ignoran archivos vacíos o con formato inconsistente.
- Se filtran líneas inválidas (sin `text`, `number` no numérico o `hex` distinto de 32 caracteres).
- Si `includeEmpty=true`, se incluyen líneas con campos parcialmente válidos (`null`).
- Se utiliza `relax_column_count: true` para evitar errores por columnas faltantes o de más.

---

## Tests 

Test Included **Mocha**, **Chai**:

- endpoint validation `/files/data` y `/files/list`.
- success scenario, filters, 404 error, null lines.

To execute them:

```bash
npm test
```

---

## Linting


Uses **StandardJS** as coding styling. 

To validate:

```bash
npm run lint
```

---

## Install

```bash
npm install
npm start
```

---

## Project structure

```
api-challenge/
├── src/
│   ├── app.js
│   ├── server.js
│   ├── routes/
│   │   └── files.js
│   └── services/
│       └── fileService.js
├── test/
│   ├── files.test.js
│   └── filesList.test.js
├── package.json
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
├── .eslintrc.json
├── swagger.yaml
└── README.md

```

---

  ## 👤 Autor

Cesar Daniel Pineda  
📧 cesardanielpineda@gmail.com  
