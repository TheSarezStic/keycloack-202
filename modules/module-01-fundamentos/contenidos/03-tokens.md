# 📘 Módulo 1 – Fundamentos

## 03 – Tokens

---

# 🧠 1️⃣ ¿Qué es un token?

Un **token** es una credencial digital que representa:

* Identidad
* Permisos
* Sesión

En IAM moderno, el token **sustituye a la sesión tradicional en servidor**.

---

# 🔑 2️⃣ Tipos de tokens en OIDC

Cuando usas **OpenID Connect** recibes:

### 🔹 ID Token

Contiene identidad del usuario.

### 🔹 Access Token

Se usa para acceder a APIs.

### 🔹 Refresh Token

Permite renovar tokens sin volver a loguearse.

---

# 🧾 3️⃣ JWT (JSON Web Token)

La mayoría de tokens modernos son:

**JSON Web Token**

Formato:

```text
xxxxx.yyyyy.zzzzz
```

Son 3 partes separadas por punto:

1. Header
2. Payload
3. Signature

---

# 🔎 4️⃣ Estructura interna

### 🔹 Header

Indica algoritmo y tipo:

```json
{
  "alg": "RS256",
  "typ": "JWT"
}
```

---

### 🔹 Payload (claims)

Contiene información:

```json
{
  "sub": "123456",
  "email": "user@email.com",
  "realm_access": {
    "roles": ["admin"]
  },
  "exp": 1712345678
}
```

---

### 🔹 Signature

Garantiza que el token no ha sido manipulado.

Se firma con clave privada del IAM.

Las APIs validan usando la clave pública (JWKS).

---

# 🔐 5️⃣ Claims importantes

| Claim        | Significado                 |
| ------------ | --------------------------- |
| sub          | Identificador único usuario |
| iss          | Emisor (issuer)             |
| aud          | Audiencia (cliente destino) |
| exp          | Expiración                  |
| iat          | Emitido en                  |
| realm_access | Roles de realm              |

---

# 🚨 6️⃣ Muy importante

El token:

* ❌ No es cifrado (normalmente)
* ✅ Está firmado
* ❌ No debe contener secretos sensibles
* ✅ Tiene expiración corta

---

# 🏗 7️⃣ Validación en backend

Una API valida:

1. Firma correcta
2. No expirado
3. Issuer válido
4. Audience válida
5. Roles necesarios

---

# 🧠 8️⃣ Cambio mental

Antes:

```text
Servidor guarda sesión en memoria
```

Ahora:

```text
Cliente guarda token
API valida token
```

La API no guarda estado.

Es arquitectura stateless.

---

# 🔥 9️⃣ Ejemplo real en Keycloak

**Keycloak** emite:

* ID Token firmado con RS256
* Access Token JWT
* Endpoint JWKS público

Las APIs usan:

```
/realms/{realm}/protocol/openid-connect/certs
```

para validar firma.

---

# 🎯 10️⃣ Qué debes retener

* Un JWT tiene 3 partes
* Está firmado, no cifrado
* Tiene claims estándar
* Es la base del IAM moderno
* Reemplaza la sesión tradicional