# 📘 Módulo 7 – Security

## 04 – Reverse Proxy con Nginx (Configuración real)

---

# 🎯 Objetivo

Desplegar **Keycloak** detrás de
**NGINX** correctamente configurado para:

* TLS termination
* Headers correctos
* Seguridad de cookies
* Compatibilidad OIDC
* Producción real

---

# 🧠 1️⃣ Arquitectura objetivo

![Image](https://i.sstatic.net/PRqTJ.png)

![Image](https://assets.digitalocean.com/articles/nginx_ssl_termination_load_balancing/nginx_ssl.png)

![Image](https://itknowledgeexchange.techtarget.com/coffee-talk/files/2022/05/nginx-reverse-proxy-diagram.jpg)

![Image](https://enhelp.supermap.io/iP/img/Reverse_Proxy.png)

Modelo:

```text
Browser (HTTPS)
   ↓
Nginx (TLS)
   ↓
Keycloak (HTTP interno)
```

Keycloak no expone puerto 8443 a internet.

---

# 🏗 2️⃣ Configuración mínima de Nginx

Ejemplo base:

```nginx
server {
    listen 443 ssl;
    server_name auth.midominio.com;

    ssl_certificate /etc/ssl/certs/fullchain.pem;
    ssl_certificate_key /etc/ssl/private/privkey.pem;

    location / {
        proxy_pass http://keycloak:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;

        proxy_http_version 1.1;
        proxy_set_header Connection "";
    }
}
```

Esto es lo mínimo funcional.

---

# 🔐 3️⃣ Configuración crítica en Keycloak

Arranque recomendado:

```bash
KC_PROXY=edge
KC_HOSTNAME=auth.midominio.com
KC_HOSTNAME_STRICT=true
```

Si no configuras esto:

❌ Redirecciones HTTP
❌ Cookies inseguras
❌ Problemas en OIDC

---

# 🔒 4️⃣ Seguridad TLS recomendada en Nginx

Añadir:

```nginx
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers on;

add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
add_header X-Content-Type-Options nosniff;
add_header X-Frame-Options DENY;
add_header X-XSS-Protection "1; mode=block";
```

---

# 🔄 5️⃣ WebSockets y HTTP/2

Keycloak usa HTTP/1.1 por defecto.

Asegurar:

```nginx
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```

Especialmente en entornos modernos.

---

# ⚠️ 6️⃣ Problema típico: redirect loop

Síntoma:

```text
Too many redirects
```

Causa común:

* Falta X-Forwarded-Proto
* KC_PROXY mal configurado
* Hostname incorrecto

---

# 🧠 7️⃣ Cookies y SameSite

Keycloak necesita cookies:

* Secure
* SameSite=Lax o None (según flujo)

Si proxy no está bien:

→ Login OIDC falla silenciosamente.

---

# 🏢 8️⃣ En Kubernetes (conceptual)

En vez de Nginx standalone:

* Ingress Controller
* ALB Ingress
* Traefik

El concepto es el mismo:

TLS termina fuera de Keycloak.

---

# 🔥 9️⃣ Validación final

Comprobar:

```bash
curl -I https://auth.midominio.com
```

Verificar:

* Strict-Transport-Security
* X-Forwarded headers correctos
* Redirecciones HTTPS correctas

---

# 🚨 10️⃣ Errores graves comunes

❌ Exponer puerto 8080 directamente
❌ No usar HTTPS
❌ No configurar hostname fijo
❌ Proxy sin HSTS
❌ Certificado caducado

---

# 🧠 11️⃣ Modelo mental correcto

```text
Reverse Proxy = Guardia de seguridad
Keycloak = Cerebro IAM
```

Separar responsabilidades.

---

# 🎯 Qué debes retener

* TLS debe terminar en proxy
* Headers X-Forwarded son críticos
* KC_PROXY obligatorio
* HSTS recomendable
* Nunca exponer Keycloak directo a internet

