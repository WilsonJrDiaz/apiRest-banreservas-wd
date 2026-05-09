# Automatización API - Desafío 3

Pruebas funcionales e integración para la [API de Libros](https://rahulshettyacademy.com/Library) usando **Postman**.

## Endpoints cubiertos

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/Addbook.php` | Agregar un libro |
| GET | `/GetBook.php` | Obtener un libro por ID |
| POST | `/DeleteBook.php` | Eliminar un libro |

## Requisitos

- [Postman](https://www.postman.com/downloads/) (versión de escritorio)
- [Newman](https://www.npmjs.com/package/newman) (opcional, para ejecución por línea de comandos)

---

## Importar la colección en Postman

1. Abre **Postman**.
2. Haz clic en **Import** (botón superior izquierdo).
3. Selecciona la pestaña **File**.
4. Haz clic en **Choose Files** y selecciona `LibraryAPI.postman_collection.json`.
5. Haz clic en **Import**.

La colección aparecerá en el panel izquierdo bajo el nombre **Library API - Desafio 3**.

---

## Ejecutar las pruebas en Postman

### Opción A: Request individual

1. Expande la colección en el panel izquierdo.
2. Selecciona cualquier request.
3. Haz clic en **Send**.
4. Ve a la pestaña **Test Results** en el panel de respuesta para ver los resultados.

### Opción B: Collection Runner (recomendado para correr todo en orden)

1. Haz clic derecho sobre la colección **Library API - Desafio 3**.
2. Selecciona **Run collection**.
3. En la ventana del Runner:
   - Asegúrate de que todos los requests estén seleccionados.
   - Mantén el orden original (es importante para las pruebas de integración).
4. Haz clic en **Run Library API - Desafio 3**.
5. Al finalizar verás el resumen con los tests pasados y fallados.

---

## Ejecutar con Newman (línea de comandos)

Newman permite ejecutar la colección sin abrir Postman y generar reportes automáticos.

### Instalación

```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```

### Ejecutar la colección

```bash
newman run LibraryAPI.postman_collection.json
```

### Ejecutar y generar reporte HTML

```bash
newman run LibraryAPI.postman_collection.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reporte.html
```

Abre `reporte.html` en el navegador para ver el reporte completo.

---

## Estructura de pruebas

```
Library API - Desafio 3
├── 1. Agregar un libro
│   ├── 1a-1b-1c-1d - Agregar libro exitosamente (200 OK)
│   └── 1e           - Agregar libro con aisle inválido (500 Error)
├── 2. Obtener un libro
│   ├── 2a-2b-2c     - Obtener libro existente por ID (200 OK)
│   └── 2d           - Obtener libro con ID inexistente (404)
├── 3. Eliminar un libro
│   ├── Setup        - Crear libro para eliminar
│   ├── 3a-3b-3c     - Eliminar libro existente (200 OK)
│   └── 3d           - Eliminar libro con ID inexistente (404)
└── 4. Pruebas de Integración
    ├── 4i           - Crear libro 1 y libro 2
    ├── 4ii          - Consultar libro 1 y libro 2
    ├── 4iii         - Eliminar libro 1 y libro 2
    └── 4iv          - Verificar que ambos libros ya no existen (404)
```

## Validaciones implementadas

| Letra | Validación |
|-------|-----------|
| **a** | Código de respuesta HTTP esperado (200, 404, 500) |
| **b** | Estructura del JSON: nombre de cada key y tipo de dato |
| **c** | Tiempo de respuesta inferior a 500 ms |
| **d** | Campo `ID` = concatenación de `isbn` + `aisle` (AddBook) / 404 con mensaje para IDs inexistentes |
