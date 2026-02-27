# 📘 Módulo 7 – Security

## 03 – SSL y Certificados (TLS en Producción)

---

# 🎯 Objetivo

Configurar correctamente HTTPS para
**Keycloak** en producción:

* TLS obligatorio
* Certificados válidos
* Reverse proxy correcto
* Seguridad de transporte fuerte

Aquí hablamos de capa 7 y criptografía real.

---

# 🧠 1️⃣ Por qué HTTPS es obligatorio

Sin TLS:

❌ Tokens pueden ser interceptados
❌ Authorization codes pueden ser robados
❌ Cookies pueden ser capturadas
❌ Ataques MITM posibles

IAM sin HTTPS no es IAM.

---

# 🔐 2️⃣ Arquitectura típica en producción

![Image](https://www.miniorange.com/blog/assets/2023/keycloak-reverse-proxy.webp)

![Image](https://projectcontour.io/img/aws-nlb-tls/fig.jpg)

![Image](https://kevalnagda.github.io/assets/img/keycloak/1.png)

![Image](https://i.sstatic.net/PRqTJ.png)

Modelo común:

```text
Internet
   ↓
Reverse Proxy (TLS termination)
   ↓
Keycloak (HTTP interno)
```

El proxy maneja certificados.

---

# 🏗 3️⃣ Opciones de TLS

## Opción A – TLS en Reverse Proxy (recomendado)

* Nginx
* HAProxy
* AWS ALB
* Ingress Controller

Keycloak corre en HTTP interno.

Más flexible y escalable.

---

## Opción B – TLS directo en Keycloak

Keycloak soporta:

```bash
--https-certificate-file
--https-certificate-key-file
```

Útil en entornos simples.

No ideal en cluster.

---

# 🔑 4️⃣ Tipos de certificados

| Tipo        | Uso                   |
| ----------- | --------------------- |
| Self-signed | Solo laboratorio      |
| CA pública  | Producción internet   |
| CA interna  | Entornos corporativos |

En producción → certificado válido firmado por CA.

---

# 🔒 5️⃣ Configuración crítica en Keycloak detrás de proxy

Cuando usas proxy debes configurar:

```bash
KC_PROXY=edge
KC_HOSTNAME=auth.midominio.com
KC_HOSTNAME_STRICT=true
```

Y asegurarte de enviar:

```text
X-Forwarded-For
X-Forwarded-Proto
```

Si no:

* Redirecciones incorrectas
* Cookies mal marcadas
* Errores en OIDC

---

# ⚠️ 6️⃣ Cookies seguras

Con HTTPS activo, Keycloak marca cookies como:

* Secure
* HttpOnly
* SameSite

Si no está bien configurado proxy:

→ Cookies pueden no marcarse como seguras.

---

# 🔥 7️⃣ Buenas prácticas TLS

✔ TLS 1.2 o superior
✔ Deshabilitar TLS 1.0 y 1.1
✔ Certificados RSA 2048+
✔ HSTS habilitado
✔ OCSP stapling (si aplica)

---

# 🧠 8️⃣ HSTS (HTTP Strict Transport Security)

Configurar en proxy:

```text
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

Evita downgrade attacks.

---

# 🚨 9️⃣ Errores comunes

❌ Usar HTTP en producción
❌ No configurar KC_PROXY
❌ No configurar hostname fijo
❌ Certificado vencido
❌ Wildcards mal gestionados

---

# 🧩 10️⃣ Flujo seguro completo

```text
Browser (HTTPS)
   ↓
Reverse Proxy (TLS)
   ↓
Keycloak (HTTP interno)
   ↓
JWT emitido
   ↓
API valida
```

Transporte cifrado en todo momento.

---

# 🧠 11️⃣ Modelo mental correcto

```text
Identidad sin TLS = identidad vulnerable
```

OIDC + JWT + PKCE no sirven sin HTTPS.

---

# 🎯 Qué debes retener

* HTTPS es obligatorio en IAM
* TLS debe terminar en proxy (idealmente)
* Configurar KC_PROXY correctamente
* Cookies deben ser Secure
* HSTS recomendado
