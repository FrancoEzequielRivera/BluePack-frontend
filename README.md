# BluePack — Correo Internacional

## Requisitos
Este proyecto requiere que el backend esté corriendo.
Repositorio del backend: https://github.com/FrancoEzequielRivera/BluePack-backend

## Descripción

BluePack es una aplicación web full stack para la gestión de envíos de correo internacional. Permite a los usuarios crear y hacer seguimiento de sus pedidos, mientras que los empleados pueden gestionar el flujo completo de cada envío.

### Incluye:

• Autenticación con JWT y roles (usuario / empleado)
• Gestión de pedidos (crear, cancelar, cambiar estado)
• Sistema de tracking con historial de eventos
• Seguridad en frontend (prevención XSS, validación de datos, almacenamiento seguro de tokens)
• Servidor HTTPS con certificado autofirmado
• Rate limiting y logs de acceso con Morgan
• Sanitización de datos en el backend



## Tecnologías utilizadas
### Frontend
• HTML5
• CSS3
• JavaScript (Vanilla)


### Backend
• Node.js
• Express
• jsonwebtoken (JWT)
• express-validator
• sanitize-html
• Morgan
• express-rate-limit

---

## Cómo ejecutar el proyecto

**Requisitos previos**

- Node.js instalado
Extensión Live Server en VS Code
Certificado SSL autofirmado (ver instrucciones abajo)


## 1 Generar los certificados SSL

Por única vez desde la carpeta raíz del proyecto ejecuta:
`openssl req -nodes -new -x509 -keyout backend/server.key -out backend/server.crt -days 365`

Dar enter en todas las peticiones que te pidan (no es necesario rellenar datos)

## 2 Instalar dependencias del backend
Dentro de la carpeta BluePack ejecuta
`cd backend`
`npm install`

Abrí esta URL en el navegador y aceptá la advertencia de seguridad:
https://localhost:3443/

Ahora accedé al http://127.0.0.1:5500/frontend/index.htmlo con Live Server instalado toca el botón de abajo a la izquierda que dice "Go Live" en VS Code

**Usuarios de prueba:**
**Correo electrónico:**
usuario@correo.com
**Contraseña:**
Usuario123!

**Correo electrónico:**
empleado@correo.com
**Contraseña:**
Empleado123!


## Endpoints

### Auth

* **POST** `/login` → Login y generación de JWT

### Pedidos

* **GET** `/pedidos` → Listar pedidos
* **GET** `/pedidos/:id` → Obtener detalle + tracking
* **POST** `/pedidos` → Crear pedido
* **PATCH** `/pedidos/:id/estado` → Cambiar estado
* **DELETE** `/pedidos/:id` → Cancelar pedido

### Tracking

* **GET** `/tracking/:numero` → Consultar envío

---

## Estados del pedido

```txt id="zqk3yu"
pendiente   → aprobado | rechazado
aprobado    → en_transito | rechazado
en_transito → entregado
```

El número de tracking se genera automáticamente al aprobar un pedido.

---

## Respuestas HTTP

* **200** → OK
* **201** → Creado correctamente
* **400** → Datos inválidos
* **401** → Token inválido
* **403** → Acceso denegado
* **404** → Recurso no encontrado
* **422** → Transición inválida
* **429** → Demasiadas peticiones

---

## Seguridad

### Frontend

* Prevención de XSS usando `textContent` y `createTextNode`
* JWT almacenado en `sessionStorage`
* Validación de formularios
* Manejo seguro de errores
* Configuración separada en `config.js`

### Backend

* JWT obligatorio (`Authorization: Bearer`)
* Middleware de roles
* Validación con `express-validator`
* Sanitización con `sanitize-html`
* Rate limiting
* HTTPS con TLS
* Logs con Morgan

---

## sessionStorage

```txt id="m31o3w"
token  → JWT
role   → rol del usuario
nombre → nombre visible en la interfaz
```
---

## Páginas

### `index.html`

* Login y redirección según rol

### `usuario.html`

* Gestión y creación de pedidos

### `empleado.html`

* Administración y cambio de estados

### `tracking.html`

* Consulta de tracking
