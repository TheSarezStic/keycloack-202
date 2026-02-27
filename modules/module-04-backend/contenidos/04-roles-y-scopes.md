# 📘 Módulo 4 – Backend

## 04 – Service Account (Machine-to-Machine)

---

# 🎯 Objetivo

Implementar autenticación **sin usuario humano** usando:

* Client Credentials Flow
* Service Account
* Validación JWT en backend
* Tokens emitidos por **Keycloak**

---

# 🧠 1️⃣ Cuándo usar esto

Casos reales:

* Microservicio A → API B
* Job programado → API
* CI/CD → Servicio interno
* Backend → Backend

Aquí NO hay login.

---

# 🔐 2️⃣ Recordatorio del flujo

Usa:

👉 Client Credentials Flow (OAuth2)

```text id="m2mflow1"
Servicio A → Keycloak → Access Token → Servicio B
```

No hay ID Token.
Solo Access Token.

---

# 🏗 3️⃣ Configuración en Keycloak

En el cliente (ej: `billing-service`):

* Access Type → Confidential
* Service Accounts Enabled → ON
* Asignar client roles al service account

Internamente Keycloak crea:

```text id="svcuser1"
service-account-billing-service
```

---

# 🔑 4️⃣ Obtener token desde backend

Ejemplo con Node (axios):

```js id="m2mgettoken"
import axios from "axios";

async function getToken() {

  const response = await axios.post(
    "http://localhost:8080/realms/demo/protocol/openid-connect/token",
    new URLSearchParams({
      grant_type: "client_credentials",
      client_id: "billing-service",
      client_secret: "YOUR_SECRET"
    }),
    {
      headers: { "Content-Type": "application/x-www-form-urlencoded" }
    }
  );

  return response.data.access_token;
}
```

---

# 📦 5️⃣ Token recibido

Ejemplo simplificado:

```json id="m2mtoken"
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "expires_in": 300,
  "token_type": "Bearer"
}
```

En el token verás:

```json id="m2mclaims"
{
  "azp": "billing-service",
  "client_id": "billing-service"
}
```

No hay identidad humana (`email`, `name`, etc).

---

# 🔎 6️⃣ Consumir API protegida

```js id="m2mcall"
const token = await getToken();

await axios.get("http://localhost:3000/protected", {
  headers: {
    Authorization: `Bearer ${token}`
  }
});
```

---

# 🔐 7️⃣ Validación en backend receptor

El backend B valida exactamente igual que antes:

* Firma
* Issuer
* Audience
* Roles

Pero ahora validará roles asignados al service account.

---

# 🧠 8️⃣ Validar que es máquina

Puedes comprobar:

```js id="checkm2m"
if (req.user.azp === "billing-service") {
  console.log("Request from service account");
}
```

O validar client roles específicos:

```js id="checkm2mrole"
requireRole("internal.write")
```

---

# ⚠️ 9️⃣ Buenas prácticas críticas

❌ Nunca exponer client_secret en frontend
❌ Nunca usar client credentials en SPA
❌ No usar mismo cliente para todo
❌ No dar permisos excesivos

Separar clientes por servicio.

---

# 🔥 10️⃣ Arquitectura profesional microservicios

```text id="m2march"
Service A
   ↓ (client credentials)
Keycloak
   ↓ (access token)
Service B
   ↓
JWT validation
```

Sin sesión.
Sin cookies.
100% stateless.

---

# 🧠 11️⃣ Diferencia clave con usuario humano

| Usuario                | Service Account |
| ---------------------- | --------------- |
| Tiene ID Token         | No              |
| Tiene identidad humana | No              |
| Usa Authorization Code | No              |
| Usa Client Credentials | Sí              |
| Representa persona     | No              |

---

# 🎯 Qué debes retener

* Service Account = identidad técnica
* Usa Client Credentials Flow
* Solo clientes confidential
* Backend valida igual que JWT normal
* Nunca exponer secretos

---

Con esto tienes:

✅ SPA profesional
✅ Backend validando JWT
✅ Autorización por roles
✅ Machine-to-machine seguro

