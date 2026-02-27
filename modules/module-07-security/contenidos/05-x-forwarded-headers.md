# 📘 Módulo 7 – Security

## 05 – X-Forwarded Headers en profundidad

---

# 🎯 Objetivo

Entender qué ocurre realmente cuando
**Keycloak**
está detrás de un proxy como
**NGINX**
o un Load Balancer.

Aquí hablamos de:

* Esquema HTTP vs HTTPS
* Hostname real
* IP real del cliente
* Seguridad en redirecciones OIDC

---

# 🧠 1️⃣ El problema real

Cuando usas proxy:

```text
Cliente → HTTPS → Nginx → HTTP → Keycloak
```

Desde el punto de vista de Keycloak:

* El esquema parece HTTP
* La IP parece la del proxy
* El host puede ser interno

Si no se corrige:

❌ Redirecciones erróneas
❌ Cookies inseguras
❌ OIDC falla
❌ Loops de redirección

---

# 🔎 2️⃣ Qué son los X-Forwarded Headers

El proxy añade cabeceras:

```text
X-Forwarded-For
X-Forwarded-Proto
X-Forwarded-Host
```

Ejemplo real:

```http
X-Forwarded-For: 83.45.12.100
X-Forwarded-Proto: https
X-Forwarded-Host: auth.midominio.com
```

Esto informa al backend:

“Lo que el cliente realmente usó”.

---

# 🔐 3️⃣ Cabeceras críticas

## X-Forwarded-Proto

Dice si el cliente vino por:

* http
* https

Si no está:

→ Keycloak cree que es HTTP
→ No marca cookies como Secure

---

## X-Forwarded-For

Contiene IP real del cliente.

Si no se pasa:

→ Todos los logins parecerán venir del proxy.
→ Brute force detection pierde efectividad.

---

## X-Forwarded-Host

Permite que redirecciones OIDC usen el hostname correcto.

---

# 🏗 4️⃣ Flujo real con headers

```text
Browser (https://auth.midominio.com)
   ↓
Nginx añade:
   X-Forwarded-Proto: https
   X-Forwarded-For: IP_real
   X-Forwarded-Host: auth.midominio.com
   ↓
Keycloak interpreta correctamente
```

---

# ⚙️ 5️⃣ Configuración necesaria en Keycloak

Al arrancar:

```bash
KC_PROXY=edge
KC_HOSTNAME=auth.midominio.com
KC_HOSTNAME_STRICT=true
```

`KC_PROXY=edge` indica:

→ Confía en los headers del proxy.

---

# 🚨 6️⃣ Riesgo de seguridad importante

Si habilitas confianza en headers pero:

❌ Keycloak es accesible directamente
❌ Proxy no está aislado

Un atacante podría enviar headers falsos:

```http
X-Forwarded-Proto: https
```

Y engañar al backend.

Solución:

✔ Solo permitir tráfico desde proxy
✔ No exponer puerto 8080

---

# 🔄 7️⃣ Problema clásico: redirect infinito

Síntoma:

```text
ERR_TOO_MANY_REDIRECTS
```

Causa común:

* No se pasa X-Forwarded-Proto
* KC_PROXY mal configurado
* Hostname no coincide

---

# 🔐 8️⃣ Cookies y SameSite

OIDC depende de cookies correctas.

Si el backend cree que es HTTP:

→ No marca `Secure`
→ Navegador rechaza cookie en flujo cross-site
→ Login falla

Muchos “errores misteriosos” vienen de aquí.

---

# 🧠 9️⃣ En Kubernetes

Ingress controllers añaden:

```text
X-Forwarded-Proto
X-Forwarded-For
```

Pero debes:

* Configurar trust correctamente
* No permitir acceso directo al pod

Conceptualmente es igual que Nginx.

---

# 🔥 10️⃣ Modelo mental final

```text
Proxy ve Internet.
Keycloak ve Proxy.
Headers traducen la realidad.
```

Si la traducción falla → seguridad falla.

---

# 🎯 Qué debes retener

* X-Forwarded-Proto es crítico
* X-Forwarded-For impacta en seguridad
* KC_PROXY=edge es obligatorio
* Nunca exponer Keycloak sin proxy
* Redirect loops casi siempre son headers
