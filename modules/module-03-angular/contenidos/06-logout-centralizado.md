# 📘 Módulo 3 – Angular

## 06 – Logout centralizado y gestión de sesión

---

# 🎯 Objetivo

Entender cómo funciona realmente el logout en un sistema con:

* SPA Angular
* **Keycloak**
* Múltiples aplicaciones
* Sesión centralizada

Aquí hablamos de SSO y SLO reales.

---

# 🧠 1️⃣ La sesión NO vive en Angular

Modelo correcto:

```text id="m1q9xv"
Angular → Tokens
Keycloak → Sesión real
```

Angular solo guarda tokens en memoria.

La sesión real está en Keycloak.

---

# 🔐 2️⃣ Logout simple

En Angular:

```ts id="z9k3vb"
keycloak.logout({
  redirectUri: window.location.origin
});
```

Esto hace:

1. Redirige a Keycloak
2. Invalida sesión en el realm
3. Redirige de vuelta

---

# 🔄 3️⃣ Single Sign-On (SSO)

Escenario:

```text id="t8w4ne"
App A login
App B login
```

Ambas comparten sesión en Keycloak.

No vuelve a pedir credenciales.

---

# 🚪 4️⃣ Single Logout (SLO)

Escenario:

```text id="p3v7qa"
Logout en App A
   ↓
Keycloak invalida sesión
   ↓
App B pierde sesión
```

En la práctica:

* App B detecta token inválido
* Redirige a login

---

# 🧠 5️⃣ Problema típico en SPA

Si tienes dos pestañas abiertas:

```text id="k6z2tu"
Pestaña 1 → logout
Pestaña 2 → sigue con token en memoria
```

Hasta que:

* Intente refrescar
* O haga una llamada API

Entonces obtiene 401.

---

# 🔥 6️⃣ Solución profesional: check-sso silencioso

keycloak-js soporta:

```ts id="w5u8lx"
keycloak.init({
  onLoad: 'check-sso',
  silentCheckSsoRedirectUri: window.location.origin + '/silent-check-sso.html'
});
```

Esto permite:

* Verificar sesión sin redirección visible
* Detectar logout externo
* Sin romper UX

---

# 📄 7️⃣ silent-check-sso.html

Archivo mínimo:

```html id="h8k1mp"
<!DOCTYPE html>
<html>
<body>
<script>
  parent.postMessage(location.href, location.origin);
</script>
</body>
</html>
```

Se coloca en `assets/`.

---

# ⚠️ 8️⃣ Logout mal implementado

Errores comunes:

❌ Solo borrar token local
❌ No llamar al endpoint logout
❌ No configurar redirect URI
❌ No usar HTTPS

Si solo haces:

```ts id="x9m4qc"
keycloak.clearToken();
```

NO invalidas sesión real.

---

# 🔐 9️⃣ Configuración importante en Keycloak

En el cliente debes configurar:

* Valid Redirect URIs
* Valid Post Logout Redirect URIs
* Web Origins correctos

Si no:

* Logout falla
* Redirección bloqueada

---

# 🧠 10️⃣ Modelo mental correcto

```text id="r4t9zs"
Login vive en Keycloak
Sesión vive en Keycloak
Logout vive en Keycloak
SPA solo refleja estado
```

---

# 🏗 11️⃣ Flujo profesional completo

```text id="v6y3nx"
Angular → keycloak.login()
        → Tokens en memoria
        → HTTP interceptor
        → Refresh automático
        → Logout centralizado
        → check-sso sincroniza pestañas
```

---

# 🎯 Qué debes retener

* La sesión real está en IAM
* Logout debe invalidar sesión central
* clearToken no es suficiente
* check-sso mejora UX
* SSO y SLO dependen del IdP

---

Con esto:

✅ Tienes integración SPA profesional completa
✅ Autenticación
✅ Guards
✅ Refresh seguro
✅ Logout centralizado