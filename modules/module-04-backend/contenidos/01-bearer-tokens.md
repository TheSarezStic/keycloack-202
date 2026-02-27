# 📘 Módulo 4 – Backend

## 01 – Bearer Tokens

---

# 🎯 Objetivo

Entender cómo funciona la autenticación en backend usando:

* Access Tokens
* Header Authorization
* Validación en API
* Tokens emitidos por **Keycloak**

Aquí dejamos la UX y entramos en seguridad real.

---

# 🧠 1️⃣ ¿Qué es un Bearer Token?

Cuando Angular llama a una API protegida envía:

```http
Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
```

Ese token:

* Representa identidad
* Contiene roles
* Está firmado
* Tiene expiración

---

# 🔐 2️⃣ ¿Por qué se llama Bearer?

“Bearer” significa:

> Quien porta el token, tiene acceso.

No requiere:

* Cookies
* Sesión en servidor
* Estado persistente

Es arquitectura stateless.

---

# 🏗 3️⃣ Flujo real en producción

```text id="x9f2lp"
Angular
   ↓
HTTP Interceptor
   ↓
Authorization: Bearer <token>
   ↓
API Node
   ↓
Validación JWT
   ↓
Respuesta
```

---

# 🔎 4️⃣ Qué debe hacer el backend SIEMPRE

Antes de procesar la request:

1. Extraer token del header
2. Verificar firma
3. Verificar expiración
4. Verificar issuer
5. Verificar audience
6. Verificar roles necesarios

Si algo falla → 401 o 403

---

# 🧾 5️⃣ Ejemplo básico en Express

Sin validación aún, solo extracción:

```js id="v6c9rt"
app.get('/protected', (req, res) => {

  const authHeader = req.headers.authorization;

  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'No token provided' });
  }

  const token = authHeader.split(' ')[1];

  res.json({ token });
});
```

Esto aún NO es seguro.

---

# 🧠 6️⃣ Qué NO debe hacer el backend

❌ Confiar en que el frontend ya validó
❌ Confiar solo en presencia del token
❌ Decodificar JWT sin verificar firma
❌ Usar solo `jwt.decode()`

Siempre verificar firma criptográfica.

---

# 🔐 7️⃣ ¿Quién firma el token?

Keycloak firma el token con su clave privada.

La API debe verificar con la clave pública (JWKS).

Endpoint típico:

```text id="p3z7wt"
/realms/{realm}/protocol/openid-connect/certs
```

---

# 🔥 8️⃣ Diferencia 401 vs 403

| Código | Significado                  |
| ------ | ---------------------------- |
| 401    | No autenticado               |
| 403    | Autenticado pero sin permiso |

Muy importante separar ambos.

---

# 🧠 9️⃣ Modelo mental correcto

```text id="n5q1cx"
Frontend protege experiencia
Backend protege seguridad
```

El backend es la autoridad final.

---

# 🎯 Qué debes retener

* Bearer token va en header Authorization
* Backend debe validar JWT siempre
* No confiar en frontend
* Validar firma, expiración, issuer y roles
* API es responsable de seguridad real