# 📘 Módulo 04 – Backend Seguro (Node.js + JWT)

---

## 🎯 Objetivo del módulo

Aprender a proteger una API backend validando correctamente los tokens emitidos por
**Keycloak**.

Aquí pasamos de “login funciona” a:

> Seguridad real en servidor.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Validar JWT correctamente en backend
* Usar JWKS para validar firmas
* Verificar issuer y audience
* Proteger endpoints por roles
* Aplicar scopes correctamente
* Implementar autenticación machine-to-machine

Este módulo es crítico para evitar falsos positivos de seguridad.

---

## 📚 Contenidos del módulo

| Archivo                  | Tema                          |
| ------------------------ | ----------------------------- |
| 01-bearer-tokens.md      | Qué es un Bearer Token        |
| 02-validacion-jwt.md     | Validación correcta de JWT    |
| 03-jwks.md               | Validación dinámica de firma  |
| 04-roles-y-scopes.md     | Autorización basada en roles  |
| 05-service-to-service.md | Integración backend ↔ backend |

---

# 🏗 Arquitectura del módulo

```text id="arch1"
Usuario
   ↓
Frontend (Angular)
   ↓
JWT
   ↓
Backend API (Node.js)
   ↓
Validación firma + claims
```

El backend **no confía en el frontend**.
Confía en la firma criptográfica del token.

---

# 🔐 Bearer Tokens

El cliente envía:

```http
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...
```

El backend debe:

✔ Extraer token
✔ Validar firma
✔ Validar issuer
✔ Validar audience
✔ Validar expiración

Si no → es vulnerable.

---

# 🔎 Validación correcta del JWT

Nunca hacer solo:

```js
jwt.decode(token)
```

Eso solo decodifica.

Debes hacer:

```js
jwt.verify(token, publicKey)
```

Y validar:

* `iss`
* `aud`
* `exp`
* `azp`

---

# 🔑 JWKS (JSON Web Key Set)

Keycloak publica sus claves en:

```text id="jwks1"
/realms/{realm}/protocol/openid-connect/certs
```

El backend debe:

1️⃣ Obtener clave pública
2️⃣ Verificar firma
3️⃣ Cachear claves

Esto permite rotación automática de claves.

---

# 🛡 Autorización basada en roles

JWT contiene:

```json id="roles1"
{
  "realm_access": {
    "roles": ["admin"]
  }
}
```

El backend debe:

✔ Verificar rol requerido
✔ Rechazar si no existe
✔ No confiar en frontend

Ejemplo:

```text id="flow1"
Request → Middleware → Validación JWT → Validación Rol → Handler
```

---

# 🤖 Service-to-Service (Client Credentials)

Para backend ↔ backend:

1️⃣ Cliente confidencial
2️⃣ Service Account habilitado
3️⃣ Flujo client credentials
4️⃣ Token sin usuario humano

Flujo:

```text id="flow2"
Servicio A
   ↓
Token client credentials
   ↓
Servicio B valida JWT
```

No hay login humano.

---

# 🧪 Laboratorios incluidos

---

## 🔹 Lab 01 – Crear API Node

* Inicializar proyecto
* Crear servidor Express
* Crear endpoint público

---

## 🔹 Lab 02 – Validación JWT

* Instalar librerías
* Crear middleware
* Validar firma con JWKS

---

## 🔹 Lab 03 – Proteger Endpoints

* Validar roles
* Validar scopes
* Probar con curl

---

## 🔹 Lab 04 – Service Account

* Crear cliente confidencial
* Obtener token client credentials
* Consumir API protegida

---

# 🧠 Modelo mental correcto

```text id="mental1"
Frontend puede mentir.
JWT firmado no miente.
```

La seguridad vive en backend.

---

# ⚠ Errores comunes

❌ Solo decodificar token
❌ No validar firma
❌ No validar audience
❌ No validar issuer
❌ No validar expiración
❌ No validar roles

---

# 🔒 Seguridad recomendada

✔ Validar firma RS256
✔ Validar issuer exacto
✔ Validar audience específica
✔ Rechazar tokens expirados
✔ No aceptar tokens de otros realms

---

# 🚀 Resultado esperado

Al terminar este módulo deberías poder:

✔ Proteger API con JWT real
✔ Validar roles correctamente
✔ Implementar client credentials
✔ Entender JWKS
✔ Diseñar backend Zero Trust

