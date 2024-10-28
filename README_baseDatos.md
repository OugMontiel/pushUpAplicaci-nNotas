[TOC]

# Diseño de base de datos

## Diagrama ER

```bash
+------------------+             +------------------+
|      Usuario     |             |       Nota       |
|------------------|             |------------------|
|                  |             |                  |
+------------------+             +------------------+
          |                                |
          |                                |
          |                                |
          | 1                            N |
          |                                |
          |                                |
+------------------+                       |
|     Historial    |-----------------------+
|------------------|
|                  |
+------------------+
```



# API de Notas

## Endpoints que deben desarrollarse

| **Funcionalidad**                    | **Método HTTP** | **Endpoint**          | **Descripción**                                              |
| ------------------------------------ | --------------- | --------------------- | ------------------------------------------------------------ |
| Crear Usuario                        | POST            | `/users`              | Crea un nuevo usuario y devuelve un token JWT.               |
| Iniciar Sesión                       | POST            | `/users/login`        | Permite a un usuario iniciar sesión y obtener un token JWT.  |
| **Cerrar Sesión (opcional)**         | POST            | `/users/logout`       | Permite a un usuario cerrar sesión.                          |
| Crear Nota                           | POST            | `/notes`              | Crea una nueva nota.                                         |
| Obtener Todas las Notas              | GET             | `/notes`              | Obtiene una lista de todas las notas.                        |
| Obtener Nota Específica              | GET             | `/notes/{id}`         | Obtiene los detalles de una nota específica.                 |
| Actualizar Nota                      | PUT             | `/notes/{id}`         | Actualiza una nota existente.                                |
| Eliminar Nota                        | DELETE          | `/notes/{id}`         | Elimina una nota específica.                                 |
| Buscar Notas                         | GET             | `/notes/search`       | Busca notas por título o contenido.                          |
| Obtener Historial de Cambios de Nota | GET             | `/notes/{id}/history` | Obtiene el historial de cambios de una nota específica. **(solo admin)** |
| **Crear Nueva Versión de Nota**      | POST            | `/notes/{id}/history` | Guarda una nueva versión de una nota. **(Sin interfaz gráfica)** |
| **Actualizar Usuario (opcional)**    | PUT             | `/users/{id}`         | Actualiza la información del usuario específico **(solo admin).** |
| **Eliminar Usuario (opcional)**      | DELETE          | `/users/{id}`         | Elimina un usuario específico **(solo admin).**              |

**Nota:** Para eliminar o actualizar la información de un usuario, esta acción se realizará únicamente desde la base de datos. No es necesario crear un API desde el **backend**, aunque si se desea implementar uno, es opcional.



## Estructura de la API (Especificaciones Técnicas)

1. **Acceso a la API:**

   - Es necesario estar logueado.
   - Cada router debe validar la sesión activa con el formato **JWT**.
   - Las sesiones tienen un tiempo máximo de expiración de 30 minutos.
   - Mensaje al caducar: "sesión expirada" (con el Formato de Respuesta).

2. **Tasas de solicitudes por tipo de método:**

   - Métodos POST - **login**:
     - Máximo de 3 solicitudes.
     - Se refrescan después de 3 minutos.
   - Métodos GET:
     - Máximo de 25 solicitudes.
     - Se refrescan después de 15 minutos.
   - Métodos POST:
     - Máximo de 45 solicitudes.
     - Se refrescan después de 15 minutos.
   - Métodos DELETE:
     - Máximo de 10 solicitudes.
     - Se refrescan después de 10 minutos.
   - Métodos PUT:
     - Máximo de 45 solicitudes.
     - Se refrescan después de 15 minutos.

3. **Mensajes al alcanzar la tasa máxima:**

   - Mensaje para **login**  "Espera 3 minutos antes de volver a intentarlo." (con el Formato de Respuesta).

   - Mensaje de "tasa superada" (con el Formato de Respuesta).



## Formato de Respuesta

Todas las respuestas seguirán un formato estándar:

```json
{
    "status": "status code", // https://http.cat/
    "message": "Mensaje opcional",
    "data": { /* Datos solicitados */ } // Si se obtienen más de un dato, la representación será de la forma [{...}], mientras que si es solo uno, será de la forma {}.
    
}
```

En caso de error:

```json
{
    "status":"status code", // https://http.cat/
    "message": "Descripción del error"
}
```



## Formato de documentación

**Nota:** El repositorio debe contener un archivo **README.md** que incluya la documentación detallada de cada API, junto con las instrucciones para instalar las dependencias del proyecto. Además, es necesario especificar la versión de **NodeJS** utilizada. Si el proyecto está desarrollado con **Spring Boot** en Java, se debe indicar que requiere al menos **JDK 17**, así como listar las dependencias utilizadas con sus versiones.



### Ejemplo de la documentación de las API.

# Crear usuario

**Method** : `GET, POST, PUT, DELETE`

**URL** : `http://localhost:3000/users/login`

**Auth required** : `True`

**header**: 

```json
{
    "Content-Type": "application/json",
    "Authorization": "Bearer ...."
}
```

**params** : `/Miguel/Castro/15` 

**URL query** : `?nombre="Miguel"&apellido="Castro"&edad=15 `

**body** : 

```json
{
    "nombre": "Miguel",
    "apellido": "Castro",
    "edad": 15
}
```

**Success Responses**

**Code** : `200 OK, 201 Created ...  `

```json
{
    "status": "status code", // https://http.cat/
    "message": "Mensaje opcional",
    "data": { /* Datos solicitados */ } // Si se obtienen más de un dato, la representación será de la forma [{...}], mientras que si es solo uno, será de la forma {}.
    
}
```

------

**Error** : ` 404 Not Found, 500 Internal Server Error ....  `

```json
{
    "status":"status code", // https://http.cat/
    "message": "Descripción del error"
}
```


