# API Data Processor   
El proyecto consume un API externo, procesa información en formato CSV y expone endpoints REST que devuelven los datos procesados y opcionalmente filtrados.

El desarrollo fue realizado utilizando **Node.js v14.17.6**. El código está estructurado de forma modular para facilitar su extensión, testeo y mantenimiento.

---

## Características implementadas

### Endpoints principales

#### GET `/files/data`
- Devuelve un arreglo con archivos válidos procesados desde el API externo.
- Permite filtrar por archivo usando `?fileName=`.
- Permite incluir líneas con campos parcialmente válidos usando `?includeEmpty=true`.
- Paginación: `?limit=` y `?offset=` (solo cuando **no** se filtra por archivo específico).
- Si se filtra por un archivo no existente o sin líneas válidas, responde:

```json
{
  "code": "SYS-ERR",
  "message": "Not Found",
  "details": null,
  "status": 404
}
```

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

## Tests automatizados

Se incluyen tests con **Mocha**, **Chai**:

- Validación de endpoints `/files/data` y `/files/list`.
- Casos de éxito, filtro válido, 404 por archivo inexistente, e inclusión de líneas nulas.

Para ejecutarlos:

```bash
npm test
```

---

## Linting


Se usa **StandardJS** como estilo de codificación. 
Para mantener una sintaxis clara y consistente se incluyó un archivo `.eslintrc.json` para facilitar la validación del estilo y detectar errores comunes.

Para validarlo:

```bash
npm run lint
```

---

## Instalación y ejecución local

```bash
npm install
npm start
```

---

## Estructura del proyecto

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

## Consideraciones

- Se busco que el código sea ES6+ válido para Node.js 14.x.
- Se adiciona un swagger con ejemplos para el fácil uso/consumo del API.
- Se utiliza ESLint con configuración StandardJS para evitar variables no usadas o mal definidas y poder limpiar el código. 
  - El archivo `.eslintrc.json` se incluyó para explicitar la configuración utilizada:
  - Esto garantiza que los archivos de prueba también se validen correctamente con `eslint` sin marcar como errores las funciones globales de Mocha (`describe`, `it`, etc.).

  ## 👤 Autor

Cesar Daniel Pineda  
📧 cesardanielpineda@gmail.com  
