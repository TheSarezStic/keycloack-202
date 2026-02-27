# 📘 Módulo 4 – Backend

## 02 – Validación JWT (firma real con JWKS)

---

# 🎯 Objetivo

Implementar validación real de tokens JWT en Node usando:

* Firma criptográfica
* JWKS
* Validación de issuer
* Validación de audience

Tokens emitidos por
**Keycloak**

---

# 🧠 1️⃣ El error más común

Muchos hacen:

```js
jwt.decode(token)
```

❌ Esto NO verifica firma.
❌ Cualquiera podría fabricar un token falso.

Siempre debemos verificar criptográficamente.

---

# 🔐 2️⃣ ¿Cómo funciona la firma?

Keycloak:

* Firma con clave privada (RS256)
* Publica clave pública vía JWKS

Endpoint típico:

```text
http://localhost:8080/realms/demo/protocol/openid-connect/certs
```

La API usa esa clave pública para validar.

---

# 📦 3️⃣ Instalar dependencias

En tu API Node:

```bash
npm install express jsonwebtoken jwks-rsa
```

---

# 🏗 4️⃣ Middleware profesional de validación

```js
import jwt from "jsonwebtoken";
import jwksClient from "jwks-rsa";

const client = jwksClient({
  jwksUri: "http://localhost:8080/realms/demo/protocol/openid-connect/certs"
});

function getKey(header, callback) {
  client.getSigningKey(header.kid, function (err, key) {
    const signingKey = key.getPublicKey();
    callback(null, signingKey);
  });
}

export function validateToken(req, res, next) {

  const authHeader = req.headers.authorization;

  if (!authHeader || !authHeader.startsWith("Bearer ")) {
    return res.status(401).json({ error: "No token provided" });
  }

  const token = authHeader.split(" ")[1];

  jwt.verify(
    token,
    getKey,
    {
      algorithms: ["RS256"],
      issuer: "http://localhost:8080/realms/demo",
      audience: "angular-app"
    },
    (err, decoded) => {

      if (err) {
        return res.status(401).json({ error: "Invalid token" });
      }

      req.user = decoded;
      next();
    }
  );
}
```

---

# 🔎 5️⃣ Qué valida esto

✔ Firma RS256
✔ Issuer correcto
✔ Audience correcta
✔ Token no expirado

Si algo falla → 401

---

# 🧠 6️⃣ ¿Qué es `kid`?

En el header del JWT:

```json
{
  "alg": "RS256",
  "kid": "abc123"
}
```

Keycloak puede rotar claves.

`kid` indica qué clave usar del JWKS.

---

# 🔥 7️⃣ Usar el middleware

```js
app.get("/protected", validateToken, (req, res) => {
  res.json({
    message: "Acceso permitido",
    user: req.user.sub
  });
});
```

---

# 🚨 8️⃣ Errores comunes

❌ No validar issuer
❌ No validar audience
❌ Aceptar cualquier algoritmo
❌ No limitar RS256
❌ No manejar rotación de claves

---

# 🧠 9️⃣ Flujo real completo

```text
Angular
   ↓
Bearer token
   ↓
API
   ↓
Verifica firma con JWKS
   ↓
Valida claims
   ↓
Permite o deniega
```

---

# 🎯 Qué debes retener

* Nunca usar decode sin verify
* JWKS permite validación segura
* Debes validar issuer y audience
* RS256 es estándar recomendado
* Backend es responsable final

