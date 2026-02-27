# 📘 Módulo 2 – Realms

## 03 – Service Accounts

---

# 🧠 1️⃣ El problema

No todo acceso es usuario ↔ aplicación.

A veces necesitas:

* Microservicio → API
* Backend → Backend
* Job automático → API
* Script CI/CD → Servicio protegido

Aquí no hay usuario humano.

Aquí usamos **Service Accounts** en
**Keycloak**.

---

# 🤖 2️⃣ ¿Qué es una Service Account?

Es una identidad técnica asociada a un cliente (app).

No es un usuario normal.

Está ligada a un cliente **confidential**.

---

# 🔑 3️⃣ Flujo usado

Se usa:

👉 **Client Credentials Flow**

```text id="p9szk4"
Servicio A → Keycloak → Access Token → Servicio B
```

No hay login.
No hay ID Token.
Solo Access Token.

---

# 🏗 4️⃣ Cómo funciona internamente

Cuando activas "Service Accounts Enabled" en un cliente:

Keycloak crea automáticamente un usuario interno:

```text id="l8c7va"
service-account-{client-id}
```

Ese usuario puede:

* Tener roles
* Tener client roles
* Tener permisos

---

# 🔎 5️⃣ Ejemplo real

Cliente: `billing-api`

Activamos:

* Confidential
* Service Accounts Enabled

Luego:

* Asignamos client role `invoice.write`

Ahora ese cliente puede pedir token.

---

# 🔁 6️⃣ Solicitud de token

Ejemplo con curl:

```bash id="x7gn22"
curl -X POST \
  http://localhost:8080/realms/demo/protocol/openid-connect/token \
  -d "grant_type=client_credentials" \
  -d "client_id=billing-api" \
  -d "client_secret=xxxxx"
```

Respuesta:

```json id="3hsl9t"
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "expires_in": 300,
  "token_type": "Bearer"
}
```

---

# 🔐 7️⃣ Qué contiene ese token

El token incluye:

* client_id como sujeto
* Roles asignados
* No incluye identidad humana

Ejemplo claim típico:

```json id="1x9vte"
"azp": "billing-api"
```

---

# 🎯 8️⃣ Cuándo usar Service Accounts

| Caso                | ¿Usar Service Account? |
| ------------------- | ---------------------- |
| Microservicio → API | ✅ Sí                   |
| CI/CD pipeline      | ✅ Sí                   |
| Usuario humano      | ❌ No                   |
| SPA Angular         | ❌ No                   |

---

# 🚨 9️⃣ Error típico

Intentar usar:

* Authorization Code
* Password Flow

Para comunicación máquina a máquina.

Incorrecto.

Debe usarse Client Credentials.

---

# 🧠 10️⃣ Modelo mental correcto

```text id="t6kz3o"
Usuario → Authorization Code
Máquina → Client Credentials
```

Service Account = identidad técnica.

---

# 🎯 Qué debes retener

* Service Account es identidad técnica
* Solo funciona con cliente confidential
* Usa Client Credentials Flow
* No representa usuario humano
* Se le asignan roles igual que a un usuario

