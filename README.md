# PracticAMZTL

Repositorio de prácticas en PHP con ejemplos de programación web y una API REST
para la gestión de productos.

## Contenido

- [Requisitos](#requisitos)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Configuración de la API](#configuración-de-la-api)
- [Ejecutar el proyecto](#ejecutar-el-proyecto)
- [Endpoints](#endpoints)
- [Prueba con Postman](#prueba-con-postman)
- [Extensiones recomendadas para VS Code](#extensiones-recomendadas-para-vs-code)
- [Solución de problemas](#solución-de-problemas)

## Requisitos

- XAMPP con PHP y MySQL/MariaDB
- Git
- Postman
- Visual Studio Code

## Estructura del proyecto

```text
practicamztl/
├── diplomado/
│   └── ejemplo-1/
│       ├── config/       # Conexión a la base de datos
│       ├── controllers/ # Lógica de los endpoints
│       ├── models/      # Consultas y acceso a datos
│       ├── public/      # Front controller de la API
│       ├── routes/      # Registro de rutas
│       └── sql/         # Esquema y datos de ejemplo
├── postman/             # Colecciones, entornos y documentos de Postman
└── practica1/           # Ejercicio introductorio de PHP
```

## Configuración de la API

### 1. Crear la base de datos

Importa el archivo `diplomado/ejemplo-1/sql/productos.sql` desde phpMyAdmin
o ejecútalo desde MySQL. Este archivo crea la base de datos `rest`, la tabla
`productos` y datos de ejemplo.

### 2. Configurar las variables de conexión

La aplicación admite las siguientes variables de entorno:

| Variable | Valor predeterminado | Descripción |
|---|---|---|
| `DB_HOST` | `127.0.0.1` | Host de MySQL |
| `DB_PORT` | `3306` | Puerto de MySQL |
| `DB_NAME` | `diplomado` | Nombre de la base de datos |
| `DB_USER` | `root` | Usuario de MySQL |
| `DB_PASS` | vacío | Contraseña de MySQL |

Para esta base de datos, `DB_NAME` debe ser `rest`.

## Ejecutar el proyecto

### Con XAMPP

1. Copia el repositorio dentro de `C:\xampp\htdocs`.
2. Inicia **Apache** y **MySQL** desde el Panel de Control de XAMPP.
3. Abre en el navegador:

   ```text
   http://localhost/practicamztl/diplomado/ejemplo-1/public/
   ```

Si no existe la variable de entorno `DB_NAME`, en Windows puedes configurarla
como variable de usuario y reiniciar Apache. Como alternativa, agrega temporalmente
las variables al proceso de Apache con `SetEnv` en su configuración.

### Con el servidor integrado de PHP

Ejecuta el proyecto desde la raíz del repositorio:

```powershell
$env:DB_HOST = "127.0.0.1"
$env:DB_PORT = "3306"
$env:DB_NAME = "rest"
$env:DB_USER = "root"
$env:DB_PASS = ""

php -S localhost:8000 -t diplomado/ejemplo-1/public diplomado/ejemplo-1/public/index.php
```

Después, abre `http://localhost:8000`.

## Endpoints

### Obtener todos los productos

```http
GET /productos
```

Respuesta exitosa:

```json
{
  "mensaje": "Lista de productos",
  "total": 3,
  "data": []
}
```

### Obtener un producto por ID

```http
GET /productos/{id_producto}
```

Ejemplo:

```text
http://localhost:8000/productos/1
```

### Respuestas de error

| Código | Situación |
|---|---|
| `400` | El ID proporcionado no es válido |
| `404` | El producto o endpoint no existe |
| `405` | El método HTTP no está permitido |
| `500` | Error interno del servidor |

Sin URL Rewrite, las rutas también pueden enviarse mediante el parámetro `ruta`:

```text
http://localhost/practicamztl/diplomado/ejemplo-1/public/index.php?ruta=/productos
http://localhost/practicamztl/diplomado/ejemplo-1/public/index.php?ruta=/productos/1
```

## Prueba con Postman

1. Crea una colección nueva o utiliza los archivos de `postman/`.
2. Configura la dirección base según el método de ejecución.
3. Agrega una solicitud `GET` a `/productos`.
4. Envía la solicitud y verifica el arreglo `data` en la respuesta.

## Extensiones recomendadas para VS Code

- PHP Debug
- PHP Extension Pack
- Material Icon Theme

## Solución de problemas

### Error de conexión a MySQL

- Confirma que MySQL esté iniciado.
- Verifica `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER` y `DB_PASS`.
- Comprueba que la base de datos `rest` exista.

### Endpoint no encontrado

- Revisa que la ruta incluida en la URL termine o empiece correctamente según
  el método de ejecución.
- Prueba la ruta con el parámetro `index.php?ruta=/productos`.
- Reinicia Apache después de cambiar variables de entorno.