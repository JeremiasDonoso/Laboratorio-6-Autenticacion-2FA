# Laboratorio de Autenticación con 2FA

## Descripción General

Este laboratorio consiste en el desarrollo de un sistema de autenticación segura usando PHP y MySQL. El objetivo principal fue aplicar buenas prácticas de seguridad en aplicaciones web, incorporando mecanismos de protección para el registro y autenticación de usuarios, así como una segunda capa de verificación mediante autenticación de dos factores (2FA) utilizando Google Authenticator.

## Funcionalidades Implementadas

### Gestión de Usuarios

Se desarrolló un sistema de registro de usuarios que permite almacenar información básica como nombre, apellido, nombre de usuario, correo electrónico, sexo y contraseña. Durante el proceso de registro se realizan validaciones tanto del lado del cliente como del lado del servidor para garantizar la integridad de la información ingresada.

Entre las validaciones implementadas se encuentran:

* Verificación de campos obligatorios.
* Validación de formato de correo electrónico.
* Verificación de coincidencia entre contraseña y confirmación de contraseña.
* Validación de longitud mínima de contraseña.
* Prevención de usuarios y correos electrónicos duplicados.

### Sanitización de Datos

Se creó una clase especializada para la limpieza y validación de entradas del usuario.

### Protección contra CSRF

Se implementó una clase de protección contra ataques CSRF (Cross-Site Request Forgery).

Para ello se generan tokens únicos almacenados en la sesión del usuario, los cuales son incluidos automáticamente en los formularios mediante campos ocultos. Al procesar cada formulario, el sistema verifica que el token recibido coincida con el almacenado en la sesión, impidiendo solicitudes fraudulentas provenientes de sitios externos.

### Hash de Contraseñas

Las contraseñas nunca se almacenan en texto plano dentro de la base de datos.

Durante el registro se utiliza la función `password_hash()` de PHP para generar hashes seguros de las contraseñas. Posteriormente, durante el inicio de sesión, se utiliza `password_verify()` para comprobar la validez de la contraseña ingresada.

Adicionalmente, se desarrolló una interfaz independiente que permite generar hashes y validar contraseñas manualmente con fines educativos y de demostración.

### Autenticación de Dos Factores (2FA)

Como segunda capa de seguridad se integró Google Authenticator mediante la librería Sonata Google Authenticator.

Durante el registro:

1. Se genera un secreto único para el usuario.
2. El secreto se almacena en la base de datos.
3. Se genera un código QR.
4. El usuario escanea el QR utilizando Google Authenticator.
5. Se solicita un código de verificación para confirmar la activación del segundo factor.

Posteriormente, durante cada inicio de sesión:

1. El usuario introduce su correo y contraseña.
2. Se valida la contraseña almacenada.
3. Se solicita el código generado por Google Authenticator.
4. Solo después de validar correctamente el código 2FA se concede acceso al sistema.

### Activación Inicial del 2FA

Se implementó un mecanismo de confirmación inicial mediante el campo `fa_confirmado` que se encuentra en la base de datos.

Esto permite detectar usuarios que se registraron pero nunca completaron la configuración de Google Authenticator. Si un usuario intenta iniciar sesión sin haber confirmado previamente el segundo factor, el sistema vuelve a mostrar el código QR para completar correctamente la activación.

### Control de Sesiones

Se implementó un sistema de sesiones para controlar el acceso a las diferentes páginas del sistema.

Las páginas protegidas verifican que el usuario haya completado correctamente tanto la autenticación tradicional como la validación del segundo factor antes de permitir el acceso.

Asimismo, se desarrolló un mecanismo de cierre de sesión que elimina las variables de sesión activas y redirige al usuario a la pantalla de inicio de sesión.

### Registro de Eventos (Logs)

Se desarrolló un sistema de auditoría mediante una tabla de registros (`logs_sistema`) que almacena eventos relevantes de seguridad.

Entre los eventos registrados se incluyen:

* Registros exitosos.
* Intentos de registro fallidos.
* Inicios de sesión exitosos.
* Intentos de inicio de sesión fallidos.
* Validaciones correctas del código 2FA.
* Intentos fallidos de validación 2FA.
* Cierres de sesión.
* Accesos directos no autorizados a funcionalidades restringidas.

Esta información permite realizar seguimiento de actividades relevantes dentro del sistema.

### Seguridad de Base de Datos

Como parte de las buenas prácticas de seguridad, se creó un usuario específico para la aplicación con privilegios mínimos sobre la base de datos.

Se evitó el uso de la cuenta root, limitando los permisos únicamente a las operaciones necesarias para el funcionamiento del sistema:

* SELECT
* INSERT
* UPDATE
* DELETE

## Tecnologías Utilizadas

* PHP
* MySQL
* Composer
* Sonata Google Authenticator
* HTML5
* CSS3
* XAMPP

## Resultado

El sistema desarrollado permite gestionar usuarios de forma segura, proteger formularios mediante tokens CSRF, almacenar contraseñas cifradas, implementar autenticación de dos factores utilizando Google Authenticator y registrar eventos importantes de seguridad, cumpliendo con los requisitos establecidos para el laboratorio de autenticación segura.

