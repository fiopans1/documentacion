quiero que me expliques generandolo directamente en una ruta llamada certificates en resources, y usando las propiedades privateKeyPath y publickeyPath, usando una publickey y una privatekey (sin certificados), jwtKeys.publickeyPath, jwtKeys.privatekeyPath
Necesitamos la dependencia nimbus-jose-jwt



crear claves

openssl genrsa -out private_key.pem 2048  

openssl rsa -pubout -in private_key.pem -out public_key.pem