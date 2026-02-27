# 📘 Módulo 3 – Angular

## 04 – Token Lifecycle

---

# 🎯 Objetivo

Entender qué ocurre con los tokens después del login:

* Expiración
* Refresh
* Sesión
* Logout
* Problemas reales en producción

Emitidos por **Keycloak** usando
**OpenID Connect**

---

# 🧠 1️⃣ Tipos de tokens y duración

Cuando haces login recibes:

| Token         | Duración típica   | Uso            |
| ------------- | ----------------- | -------------- |
| Access Token  | 1–5 min           | Llamar APIs    |
| ID Token      | 5–15 min          | Identidad      |
| Refresh Token | 10–30 min (o más) | Renovar sesión |

En Keycloak esto es configurable por realm.

---

# 🔄 2️⃣ Ciclo normal de vida

```text id="a1kz7p"
Login
  ↓
Access Token válido
  ↓
Expira
  ↓
Se usa Refresh Token
  ↓
Nuevo Access Token
```

La SPA nunca vuelve a pedir password.

---

# 🔐 3️⃣ Expiración del Access Token

Dentro del JWT:

```json id="4nt9xz"
{
  "exp": 1712345678
}
```

El frontend puede ver cuánto falta:

```ts id="m3z1wl"
keycloak.tokenParsed?.exp
```

---

# 🔁 4️⃣ Refresh automático

keycloak-js permite:

```ts id="p6s2du"
keycloak.updateToken(30)
```

Significa:

> Si el token expira en menos de 30 segundos → refrescar.

Ejemplo típico:

```ts id="t8c4lr"
setInterval(() => {
  keycloak.updateToken(30)
}, 10000);
```

---

# 🧠 5️⃣ ¿Qué pasa si el Refresh Token expira?

Escenario:

* Usuario deja la app abierta
* Refresh token expira
* updateToken falla

Entonces:

```ts id="v9q3kb"
keycloak.login();
```

Se fuerza nuevo login.

---

# 🚪 6️⃣ Logout

Cuando haces:

```ts id="n5x7ec"
keycloak.logout();
```

Ocurre:

1. Se invalida sesión en Keycloak
2. Se redirige a URL post-logout
3. Tokens dejan de ser válidos

---

# 🔄 7️⃣ Single Logout (SLO)

Si tienes múltiples apps:

```text id="y3u6qw"
App A login
App B login
Logout en App A
↓
App B pierde sesión
```

Porque sesión vive en Keycloak.

---

# ⚠️ 8️⃣ Problemas comunes en producción

❌ No refrescar token → 401 inesperados
❌ Guardar token en localStorage → XSS vulnerable
❌ No sincronizar logout entre pestañas
❌ Access token demasiado largo

---

# 🔥 9️⃣ Buenas prácticas

* Access Token corto (1–5 min)
* Refresh Token razonable
* Usar HTTPS siempre
* No exponer tokens en logs
* No guardar tokens persistentes en navegador

---

# 🧠 10️⃣ Modelo mental correcto

```text id="c4p9tz"
Access Token = credencial corta
Refresh Token = renovación
Sesión vive en IAM
SPA solo maneja tokens
```

---

# 🎯 Qué debes retener

* Access tokens expiran rápido
* Refresh mantiene sesión viva
* updateToken es obligatorio en SPA
* Logout invalida sesión central
* Seguridad depende de expiraciones correctas

