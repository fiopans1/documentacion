## Guía Completa: Proteger tu Página Web Usando Cloudflare como Proxy y Configurarlo

Cloudflare es una plataforma popular para mejorar la seguridad y el rendimiento de tu página web. Actúa como un proxy inverso, ayudando a proteger tu sitio contra ataques DDoS, bots maliciosos y otras amenazas. A continuación, te explico cómo configurar y usar Cloudflare para proteger tu página web.

---

### **1. Crear una Cuenta en Cloudflare**

1. Ve a [Cloudflare](https://www.cloudflare.com/) y crea una cuenta.
2. Confirma tu correo electrónico siguiendo el enlace que te enviarán.

---

### **2. Agregar tu Sitio Web a Cloudflare**

1. Inicia sesión en tu cuenta de Cloudflare.
2. Haz clic en **"Agregar un sitio"**.
3. Ingresa el dominio de tu página web (ejemplo: `tusitio.com`) y haz clic en **"Agregar"**.
4. Selecciona un plan adecuado (el plan gratuito incluye funciones básicas de protección).

---

### **3. Configurar los Servidores de Nombre (DNS)**

1. Cloudflare escaneará automáticamente tus registros DNS. Verifica que todos los registros sean correctos.
2. Cambia los servidores de nombre de tu dominio a los que proporciona Cloudflare.  
   - Esto se hace en el panel de administración de tu proveedor de dominio (ejemplo: GoDaddy, Namecheap, etc.).
   - Reemplaza los servidores de nombre actuales con los proporcionados por Cloudflare.

💡 **Nota:** La propagación del cambio de DNS puede tardar hasta 24 horas.

---

### **4. Activar Cloudflare como Proxy**

En el panel de DNS de Cloudflare:

1. Asegúrate de que el icono de nube naranja esté activado junto a los registros DNS de tu dominio (por ejemplo, el registro `A` o `CNAME` que apunta a tu servidor).  
   - **Nube naranja** significa que Cloudflare actúa como un proxy inverso para ese subdominio.
   - **Nube gris** significa que el tráfico pasa directamente a tu servidor sin protección.

---

### **5. Configurar HTTPS (SSL/TLS)**

1. En el panel de Cloudflare, ve a la sección **"SSL/TLS"**.
2. Selecciona el modo SSL adecuado:
   - **Flexible:** El tráfico entre Cloudflare y tu servidor no está encriptado. (No recomendado).
   - **Full:** Requiere un certificado SSL en tu servidor. (Recomendado para seguridad básica).
   - **Full (Strict):** Requiere un certificado SSL válido en tu servidor. (Recomendado para máxima seguridad).
3. Habilita la redirección automática de HTTP a HTTPS en la pestaña **"Edge Certificates"**.

---

### **6. Configurar el Firewall de Cloudflare**

1. Ve a la sección **"Firewall"** en el panel de Cloudflare.
2. Configura reglas de firewall personalizadas para proteger tu sitio:
   - **Bloqueo de IPs sospechosas:** Añade IPs específicas o rangos que deseas bloquear.
   - **Desafío Captcha:** Aplica un desafío Captcha para visitantes sospechosos.
   - **Bloquear países:** Si tu sitio no opera en ciertas regiones, puedes bloquear accesos desde esos países.

---

### **7. Activar la Protección Contra Ataques DDoS**

1. En **"Security"**, habilita **"DDoS Protection"** (disponible en todos los planes).
2. Ajusta el nivel de seguridad:
   - **Bajo:** Ideal para sitios con mucho tráfico legítimo.
   - **Alto:** Filtra tráfico agresivamente (recomendado durante un ataque).

---

### **8. Optimizar la Configuración de Bots**

1. Ve a **"Bots"** en el panel de Cloudflare.
2. Activa la opción **"Bot Fight Mode"** para bloquear bots maliciosos automáticamente.  
   *(Esta función está disponible en el plan gratuito)*.

---

### **9. Configurar Page Rules (Reglas de Página)**

1. Ve a la sección **"Page Rules"**.
2. Crea reglas específicas para optimizar y proteger tu sitio:
   - **Forzar HTTPS:** Configura una regla que redirija todo el tráfico a HTTPS.
   - **Caché agresiva:** Mejora el rendimiento para páginas estáticas.
   - **Desactivar funcionalidades:** Reduce el riesgo de ataques en áreas específicas, como `/admin` o `/login`.

---

### **10. Monitorizar el Tráfico y los Registros**

1. En **"Analytics"**, revisa las estadísticas de tráfico y amenazas bloqueadas.
2. Ajusta las configuraciones si detectas comportamientos sospechosos o ataques.

---

### **11. (Opcional) Habilitar Seguridad Avanzada**

Si deseas un nivel adicional de protección, considera activar estas funciones en planes pagos:

- **WAF (Web Application Firewall):** Protege tu sitio contra vulnerabilidades comunes como SQL Injection o XSS.
- **Rate Limiting:** Limita el número de solicitudes por usuario para prevenir abusos.
- **Access Control:** Configura accesos limitados a ciertas áreas usando políticas basadas en IP o autenticación.

---

### **12. Probar tu Configuración**

1. Verifica que tu sitio web esté funcionando correctamente detrás de Cloudflare.
2. Prueba la redirección a HTTPS y las reglas configuradas.
3. Usa herramientas como [SSL Labs](https://www.ssllabs.com/ssltest/) para comprobar la configuración SSL.

---

### **Resumen**

Al seguir esta guía, tu página web estará protegida contra la mayoría de las amenazas en línea. Cloudflare no solo mejora la seguridad, sino también el rendimiento de tu sitio mediante su red de distribución global. Recuerda revisar regularmente tu configuración en función de las necesidades de tu sitio y posibles nuevos riesgos. 

