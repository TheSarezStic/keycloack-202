# 📘 Módulo 07 – Seguridad Avanzada y Hardening

---

## 🎯 Objetivo del módulo

Profundizar en la seguridad real de
**Keycloak**
y su integración en entornos productivos mediante:

* PKCE en profundidad
* Estrategias de expiración
* SSL y certificados
* Reverse Proxy seguro
* X-Forwarded headers
* Hardening completo

Este módulo transforma una instalación funcional en una instalación segura.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Entender en profundidad PKCE y sus amenazas
* Diseñar estrategias de expiración seguras
* Configurar HTTPS correctamente
* Integrar Keycloak detrás de NGINX
* Configurar correctamente cabeceras proxy
* Aplicar hardening real en producción

Este es el módulo que diferencia un entorno de laboratorio de uno empresarial.

---

## 📚 Contenidos del módulo

| Archivo                      | Tema                             |
| ---------------------------- | -------------------------------- |
| 01-pkce-deep-dive.md         | Seguridad del Authorization Code |
| 02-estrategias-expiracion.md | Control de sesión y tokens       |
| 03-ssl-y-certificados.md     | HTTPS y TLS                      |
| 04-reverse-proxy-nginx.md    | Arquitectura segura con proxy    |
| 05-x-forwarded-headers.md    | Cabeceras críticas               |
| 06-hardening.md              | Checklist de producción          |

---

# 🔐 PKCE Deep Dive

PKCE protege contra:

* Intercepción del authorization code
* Ataques en SPA
* Code injection

Modelo:

```text
code_verifier → hash → code_challenge
```

Sin PKCE en SPA → vulnerabilidad crítica.

---

# ⏳ Estrategias de expiración

Debes definir correctamente:

* Access Token lifespan
* Refresh Token lifespan
* Session timeout
* Offline session timeout

Modelo recomendado:

```text
Access token corto
Refresh controlado
Sesión limitada
```

Nunca tokens de larga duración sin control.

---

# 🔒 SSL y Certificados

HTTPS es obligatorio.

Opciones:

* TLS en reverse proxy
* TLS directo en Keycloak
* Certificados firmados (ACME / CA corporativa)

Buenas prácticas:

✔ TLS 1.2 mínimo
✔ Certificados válidos
✔ HSTS si aplica
✔ Redirección HTTP → HTTPS

---

# 🏗 Reverse Proxy (NGINX)

Arquitectura típica:

```text
Internet
   ↓
NGINX (TLS)
   ↓
Keycloak
```

Ventajas:

* Terminación TLS
* Control de cabeceras
* Rate limiting
* Protección adicional

Nunca exponer Keycloak directamente en internet sin proxy en entornos serios.

---

# 📡 X-Forwarded Headers

Cabeceras críticas:

* X-Forwarded-For
* X-Forwarded-Proto
* X-Forwarded-Host

Configuración incorrecta puede causar:

❌ URLs mal generadas
❌ Redirect loops
❌ Fallos OIDC

Debe configurarse coherentemente entre proxy y Keycloak.

---

# 🛡 Hardening Checklist

Configuraciones obligatorias en producción:

✔ Brute Force Detection activado
✔ Password policy fuerte
✔ MFA en roles sensibles
✔ Clients restringidos
✔ Redirect URIs específicas
✔ CORS controlado
✔ Health endpoints protegidos
✔ Logs de eventos habilitados

IAM es superficie crítica de ataque.

---

# 🧪 Laboratorios incluidos

---

## 🔹 Lab 01 – NGINX

* Definir servicio proxy
* Configurar upstream
* Validar headers

---

## 🔹 Lab 02 – SSL

* Generar certificado
* Configurar HTTPS
* Validar canal seguro

---

## 🔹 Lab 03 – Hardening

* Configurar expiraciones
* Restringir clientes
* Validar seguridad

---

# 🧠 Modelo mental correcto

```text
Identidad es el perímetro.
Si IAM cae, todo cae.
```

La seguridad de Keycloak no es opcional.

---

# ⚠ Errores comunes

❌ No usar PKCE
❌ Tokens demasiado largos
❌ No usar HTTPS
❌ Redirect URIs abiertas
❌ No validar audience
❌ No proteger endpoints administrativos

---

# 🔒 Seguridad recomendada mínima

✔ Authorization Code + PKCE
✔ TLS obligatorio
✔ MFA en roles administrativos
✔ Validación estricta de JWT
✔ Reverse proxy correctamente configurado

---

# 🚀 Resultado esperado

Al terminar este módulo deberías poder:

✔ Endurecer Keycloak en producción
✔ Configurar reverse proxy seguro
✔ Implementar TLS correctamente
✔ Diseñar expiración adecuada
✔ Detectar configuraciones inseguras