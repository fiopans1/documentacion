quiero que me expliques generandolo directamente en una ruta llamada certificates en resources, y usando las propiedades privateKeyPath y publickeyPath, usando una publickey y una privatekey (sin certificados), jwtKeys.publickeyPath, jwtKeys.privatekeyPath
Necesitamos la dependencia nimbus-jose-jwt



crear claves

openssl genrsa -out private_key.pem 2048  

openssl rsa -pubout -in private_key.pem -out public_key.pem




Creamos LoginDto con user y pass

Creamos ResponseDto con numofErrors y messages

Creamos en validations un UserValidation con las diferentes validaciones, lo necesitamos instanciar con un @Bean


Configurar los cors, para adMapping /** y alowed origins la url de origen y alowedmethods lso metodos permitidos, y allowedheaders
origins, contenttype  y accept y authorization, y allowCredentialstrue para indicar que tiene que haber credenciales, y maxAge 3600, si en allowedorigins ponemos un * se acepta cualquier ruta

Crear una interfaz de IJWTUtilityService, este debe cargar la clave privada