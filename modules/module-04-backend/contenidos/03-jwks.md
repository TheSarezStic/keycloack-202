# 📘 Módulo 4 – Backend

## 03 – Validar Roles y Scopes

---

# 🎯 Objetivo

Después de validar el JWT, ahora debemos validar:

* ¿Tiene el usuario el rol correcto?
* ¿Tiene el scope necesario?
* ¿Puede ejecutar esta operación?

Tokens emitidos por
**Keycloak**

---

# 🧠 1️⃣ Recordatorio importante

Validar firma ≠ validar permisos.

Primero:

✔ Token válido
Luego:

✔ Permisos correctos

Son dos pasos distintos.

---

# 🔎 2️⃣ Dónde viven los roles en el token

## 🔹 Realm roles

```json id="v6j3xp"
"realm_access": {
  "roles": ["admin"]
}
```

---

## 🔹 Client roles

```json id="g3l8kr"
"resource_access": {
  "facturacion-api": {
    "roles": ["invoice.read"]
  }
}
```

Las APIs normalmente validan client roles.

---

# 🏗 3️⃣ Middleware para validar client roles

```js id="c5k8vn"
export function requireRole(role) {

  return (req, res, next) => {

    const resourceAccess = req.user.resource_access;
    const clientRoles = resourceAccess?.["facturacion-api"]?.roles || [];

    if (!clientRoles.includes(role)) {
      return res.status(403).json({ error: "Forbidden" });
    }

    next();
  };
}
```

---

# 🔐 4️⃣ Uso en rutas

```js id="v8m2xt"
app.get(
  "/invoices",
  validateToken,
  requireRole("invoice.read"),
  (req, res) => {
    res.json({ message: "Listado de facturas" });
  }
);
```

Si no tiene el rol → 403

---

# 🔥 5️⃣ Validar múltiples roles

```js id="e4t7zm"
export function requireAnyRole(roles) {

  return (req, res, next) => {

    const clientRoles = req.user.resource_access?.["facturacion-api"]?.roles || [];

    const hasRole = roles.some(r => clientRoles.includes(r));

    if (!hasRole) {
      return res.status(403).json({ error: "Forbidden" });
    }

    next();
  };
}
```

---

# 🧠 6️⃣ ¿Qué pasa con scopes?

En algunos casos el token incluye:

```json id="m2w9cr"
"scope": "invoice.read invoice.write"
```

Puedes validar:

```js id="k9v4tz"
const scopes = req.user.scope?.split(" ") || [];

if (!scopes.includes("invoice.read")) {
  return res.status(403).json({ error: "Insufficient scope" });
}
```

---

# ⚠️ 7️⃣ 401 vs 403 bien usados

| Caso                      | Código |
| ------------------------- | ------ |
| Token inválido            | 401    |
| Token válido pero sin rol | 403    |

Separar bien estos casos es profesional.

---

# 🧠 8️⃣ Modelo mental correcto

```text id="n8q1kp"
JWT válido → autenticado
Rol correcto → autorizado
```

Son capas distintas.

---

# 🚨 9️⃣ Error típico

❌ Validar solo realm roles
❌ No especificar client
❌ Usar roles del frontend
❌ No desacoplar permisos técnicos

Siempre validar lo que usa la API.

---

# 🎯 10️⃣ Patrón limpio recomendado

Separar:

* validateToken → autenticación
* requireRole → autorización

Middleware encadenados.

---

# Flujo profesional completo

```text id="z4v2xq"
Request
   ↓
validateToken
   ↓
requireRole
   ↓
Controller
```

---

# 🎯 Qué debes retener

* Firma válida no significa permiso
* Validar client roles en backend
* 401 ≠ 403
* Middleware desacoplados
* Backend es autoridad final

