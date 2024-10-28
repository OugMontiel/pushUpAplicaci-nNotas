[TOC]

# Descripción del Proyecto

La **Aplicación de Notas** es una plataforma diseñada para facilitar la gestión de notas personales de los usuarios. Su principal objetivo es proporcionar un entorno sencillo y accesible donde los usuarios puedan crear, editar, visualizar y eliminar sus notas de manera eficiente. La aplicación cuenta con funcionalidades como la búsqueda de notas, autenticación de usuario y un historial de cambios, lo que permite a los usuarios organizar sus pensamientos y mantener un registro de sus ideas de forma segura y privada.

La aplicación está desarrollada con una arquitectura que incluye tanto un frontend intuitivo como un backend sencillo, asegurando una experiencia de usuario fluida. Además, el uso de un token JWT para la autenticación garantiza que solo los usuarios autorizados puedan acceder a sus notas, protegiendo así la privacidad y seguridad de la información.



## Problemática

En un mundo cada vez más digitalizado, muchas personas enfrentan dificultades para organizar sus pensamientos y tareas diarias. Las aplicaciones de notas tradicionales a menudo carecen de características esenciales, como la capacidad de buscar rápidamente notas o llevar un registro de cambios. Esto puede llevar a la frustración y a la pérdida de información valiosa.

Algunos problemas específicos que la **Aplicación de Notas** busca resolver son:

1. **Falta de Organización**: Sin una herramienta adecuada, los usuarios pueden tener dificultades para mantener sus notas organizadas y accesibles. La aplicación permite categorizar y buscar notas fácilmente.

2. **Seguridad de la Información**: La falta de autenticación en muchas aplicaciones de notas puede poner en riesgo la privacidad de los usuarios. La implementación de un sistema de autenticación protege las notas de accesos no autorizados.

3. **Dificultad para Rastrear Cambios**: Muchas veces, los usuarios necesitan recordar versiones anteriores de sus notas o cambios realizados. La función de historial de cambios permite a los usuarios ver y restaurar versiones anteriores de sus notas si es necesario.

4. **Accesibilidad**: La aplicación ofrece una interfaz intuitiva que facilita su uso tanto para usuarios técnicos como para aquellos con menos experiencia, promoviendo una adopción más amplia.

   

## Funcionalidades Principales

- **Crear Nota:** Permitir al usuario agregar una nueva nota con un título y un contenido.

- **Editar Nota:** Permitir modificar el título y el contenido de una nota existente.

- **Eliminar Nota:** Eliminar una nota específica de la lista.

- **Ver Notas:** Mostrar una lista de todas las notas existentes con la posibilidad de ver los detalles de cada una.

- **Buscador:** Implementar una barra de búsqueda para que el usuario pueda buscar notas por el título o el contenido. A medida que el usuario escribe en el campo de búsqueda, se filtrarán las notas para mostrar solo aquellas que coincidan con los términos de búsqueda.

  

## Funcionalidades Adicionales

- **Búsqueda y Filtrado:** Implementar una barra de búsqueda para encontrar notas por título o contenido, y filtros por etiqueta o categoría.

- **Autenticación de Usuario:** Agregar una capa de autenticación para que los usuarios puedan crear una cuenta e iniciar sesión, de modo que sus notas sean privadas. Esto se implementará mediante una API que creará el usuario y proporcionará un token JWT para identificar al dueño de las notas en las APIs.

- **Historial de Cambios:** Mantener un registro de las ediciones, eliminaciones y creaciones para realizar un seguimiento de las actividades del usuario, las cuales se almacenarán en la base de datos. Este historial será visible únicamente para el administrador.

  

## Tecnologías Recomendadas

1. **Front-end**: 

   - **Figma**: https://www.figma.com/community/file/1014161465589596715
   - **Recurso:** https://drive.google.com/drive/folders/1klvUArDXJAT46JaSO76IJyBXKsRXhsxy?usp=sharing
   - **Opción 1 (Opcional) :** React o Vue.js para construir una interfaz de usuario interactiva y dinámica.
   - **Opción 2:** Desarrollo puro utilizando HTML, CSS y JavaScript para una implementación más sencilla.

2. **Backend**:

   - **Node.js** con Express para la creación de los endpoints.
   - **Java** con Spring Boot  para la creación de los endpoints. 
3. **Arquitectura del Proyecto**: Se pueden elegir entre dos opciones:

     - **MVC (Modelo-Vista-Controlador)**: Para separar la lógica de negocio de la interfaz de usuario.
     - **Arquitectura Hexagonal**: Para crear una aplicación que sea más fácil de probar y mantener, separando los componentes en diferentes capas.

4. **Base de Datos**:
   - **MongoDB**: Para un almacenamiento flexible de actividades y sus atributos.
   - **PostgreSQL** o **MySQL**: Si prefieres una base de datos relacional.

5. **Autenticación**:
   - **JWT (JSON Web Tokens):** para gestionar sesiones y autenticación de usuarios.
   - **Autenticación Segura (Opcional)**: Implementar autenticación con OAuth2.

6. **GitHub**: Para la gestión de versiones del código en el desarrollo, usando **conventional commits.**



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

| **Funcionalidad**                    | **Método HTTP** | **Endpoint**              | **Descripción**                                              |
| ------------------------------------ | --------------- | ------------------------- | ------------------------------------------------------------ |
| Crear Usuario                        | POST            | `/users`              | Crea un nuevo usuario y devuelve un token JWT.               |
| Iniciar Sesión                       | POST            | `/users/login`        | Permite a un usuario iniciar sesión y obtener un token JWT.  |
| **Cerrar Sesión (opcional)** | POST            | `/users/logout`       | Permite a un usuario cerrar sesión.                          |
| Crear Nota                           | POST            | `/notes`              | Crea una nueva nota.                                         |
| Obtener Todas las Notas              | GET             | `/notes`              | Obtiene una lista de todas las notas.                        |
| Obtener Nota Específica              | GET             | `/notes/{id}`         | Obtiene los detalles de una nota específica.                 |
| Actualizar Nota                      | PUT             | `/notes/{id}`         | Actualiza una nota existente.                                |
| Eliminar Nota                        | DELETE          | `/notes/{id}`         | Elimina una nota específica.                                 |
| Buscar Notas                         | GET             | `/notes/search`       | Busca notas por título o contenido.                          |
| Obtener Historial de Cambios de Nota | GET             | `/notes/{id}/history` | Obtiene el historial de cambios de una nota específica. **(solo admin)** |
| **Crear Nueva Versión de Nota**      | POST            | `/notes/{id}/history` | Guarda una nueva versión de una nota. **(Sin interfaz gráfica)** |
| **Actualizar Usuario (opcional)**     | PUT             | `/users/{id}`         | Actualiza la información del usuario específico **(solo admin).** |
| **Eliminar Usuario (opcional)**                 | DELETE          | `/users/{id}`         | Elimina un usuario específico **(solo admin).**              |

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

------



# Rúbrica Evaluativa

## 1.  Funcionalidad General (15%)

   - **0**: La aplicación no cumple con las funcionalidades básicas (crear, editar, eliminar notas).

   - **25**: Algunas funcionalidades están presentes, pero muchas no funcionan correctamente.

   - **50**: La mayoría de las funcionalidades funcionan, pero hay errores significativos en algunas.

   - **75**: Todas las funcionalidades principales están presentes y funcionan correctamente, con algunos errores menores.

   - **100**: Todas las funcionalidades están implementadas, funcionan correctamente y ofrecen una experiencia de usuario fluida.

     

## 2.  Interfaz de Usuario (UI) (15%)

   - **0**: La interfaz es inusable y no es intuitiva.

   - **25**: La interfaz es confusa y tiene problemas de diseño evidentes.

   - **50**: La interfaz es básica y funcional, pero carece de elementos visuales atractivos.

   - **75**: La interfaz es agradable, intuitiva y fácil de navegar, con algunos elementos de diseño que pueden mejorarse.

   - **100**: La interfaz es profesional, intuitiva y estéticamente atractiva, proporcionando una excelente experiencia de usuario.

     

## 3.  Autenticación y Seguridad (30%)

   - **0**: No se implementa ninguna medida de autenticación o seguridad.

   - **25**: Se implementa un sistema básico de autenticación, pero tiene fallos de seguridad significativos.

   - **50**: Se implementa un sistema de autenticación funcional, pero con algunas vulnerabilidades.

   - **75**: Se implementa un sistema de autenticación seguro y funcional, con mínimos riesgos.

   - **100**: La aplicación utiliza un sistema de autenticación robusto y seguro, protegiendo la información del usuario de manera efectiva.

     

## 4.  Documentación (10%)

   - **0**: No hay documentación proporcionada.

   - **25**: La documentación es mínima y poco clara.

   - **50**: La documentación está presente, pero carece de detalles importantes.

   - **75**: La documentación es clara y completa, pero falta alguna información adicional.

   - **100**: La documentación es exhaustiva, clara y bien estructurada, incluyendo ejemplos y guías de uso.

     

## 5. Entrega de la Base de Datos y sus Datos (20%)

   - **0**: No se entrega ninguna base de datos ni datos.
   
   - **25**: La base de datos está presente, pero contiene errores y falta información clave.
   
   - **50**: La base de datos está completa, pero con algunas inconsistencias o datos faltantes.
   
   - **75**: La base de datos está bien estructurada y contiene la mayoría de los datos necesarios, con algunas áreas que pueden mejorarse.
   
   - **100**: La base de datos está perfectamente estructurada, con todos los datos necesarios y en un formato accesible.
   
     

## 6.  GitHub y Entrega de Proyecto (10%)

   - **0 puntos:** 🚨No se entregó ningún repositorio, su visualización está oculta (o no compartida con el instructor) o hubo adulteración después de la fecha y hora establecida para su entrega. Evidencia de clonación o distribución del trabajo por cualquier medio de comunicación (verbal, digital, entre otros), se asumirá como cancelación del proyecto de manera definitiva. 🚨

   - **25 puntos:** Se creó el repositorio, pero en su rama principal no se encuentra el proyecto general ni ningún archivo relacionado.
     - **100 puntos:** Se creó exitosamente el repositorio, donde en su rama principal se encuentra el proyecto general y sus archivos, con evidencia de la participación del equipo completo de manera periódica.
