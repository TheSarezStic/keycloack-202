# 📘 Módulo 3 – Angular

## 01 – keycloak-js

---

# 🎯 Objetivo

Entender cómo integrar una SPA Angular con
**Keycloak**
usando la librería oficial:

👉 `keycloak-js`

---

# 🧠 1️⃣ ¿Qué es keycloak-js?

Es el adaptador JavaScript oficial para:

* Login OIDC
* Gestión de tokens
* Refresh automático
* Logout
* Protección de rutas

Implementa:

* **OpenID Connect**
* Authorization Code + PKCE

---

# 🏗 2️⃣ Arquitectura SPA moderna

![Image](https://wso2.com/asgardeo/docs/assets/img/guides/applications/oidc/auth_code_flow_with_pkce.png)

![Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1649336853356/1-hPW3ycj.jpg?auto=compress)

![Image](https://s3-eu-central-1.amazonaws.com/bootify-prod/ext/webp/securityResourceServer/resource-server-flow.webp)

![Image](https://user-images.githubusercontent.com/35077725/138667048-b4cbc465-e39c-47e3-bad7-ac1591d90777.PNG)

Flujo:

```text
Angular → Redirección a Keycloak
        → Login
        → Code
        → Tokens
        → Angular usa Access Token
```

La SPA:

* No guarda password
* Solo maneja tokens
* Usa PKCE automáticamente

---

# 📦 3️⃣ Instalación

En tu proyecto Angular:

```bash
npm install keycloak-js
```

---

# 🔧 4️⃣ Configuración básica

Crear un servicio de inicialización:

```ts
import Keycloak from 'keycloak-js';

const keycloak = new Keycloak({
  url: 'http://localhost:8080',
  realm: 'demo',
  clientId: 'angular-app'
});

export default keycloak;
```

---

# ▶️ 5️⃣ Inicialización

En `main.ts` o `app.module.ts`:

```ts
keycloak.init({
  onLoad: 'login-required',
  pkceMethod: 'S256'
}).then(authenticated => {
  console.log('Authenticated:', authenticated);
});
```

---

# 🔐 6️⃣ Qué hace internamente

Cuando haces `init()`:

1. Redirige a Keycloak
2. Usuario se autentica
3. Keycloak devuelve authorization code
4. keycloak-js intercambia code por tokens
5. Guarda tokens en memoria

No usa localStorage por defecto.

---

# 🧾 7️⃣ Acceso a tokens

```ts
keycloak.token           // Access Token
keycloak.idToken         // ID Token
keycloak.refreshToken    // Refresh Token
```

Claims decodificados:

```ts
keycloak.tokenParsed
```

---

# 🔄 8️⃣ Refresh automático

```ts
setInterval(() => {
  keycloak.updateToken(30).then(refreshed => {
    if (refreshed) {
      console.log('Token refreshed');
    }
  });
}, 10000);
```

Renueva si faltan <30s para expirar.

---

# 🚪 9️⃣ Logout

```ts
keycloak.logout();
```

Cierra sesión en Keycloak y redirige.

---

# ⚠️ 10️⃣ Errores comunes

❌ Usar Implicit Flow
❌ Guardar tokens en localStorage
❌ No activar PKCE
❌ No configurar redirect URIs correctamente

---

# 🧠 Modelo mental correcto

```text
Angular no autentica.
Angular delega autenticación.
Keycloak emite tokens.
Angular usa tokens.
```

---

# 🎯 Qué debes retener

* keycloak-js implementa OIDC
* Usa Authorization Code + PKCE
* Maneja login y refresh
* Guarda tokens en memoria
* Es el puente SPA ↔ Keycloak
