# 📘 Módulo 1 – Fundamentos

## 02 – OAuth2 vs OIDC

---

## 🧠 1️⃣ El problema que intentan resolver

Cuando una aplicación quiere acceder a recursos protegidos:

* ¿Cómo delega acceso sin compartir contraseña?
* ¿Cómo autentica usuarios sin implementar login propio?

Aquí aparecen:

* **OAuth 2.0**
* **OpenID Connect**

---

# 🔐 2️⃣ OAuth 2.0 (Autorización)

👉 OAuth2 **NO es autenticación**.
Es un framework de **delegación de acceso**.

Permite decir:

> “Esta app puede acceder a este recurso en mi nombre”

### 🎯 Ejemplo clásico

Login con Google:

* No das tu password a la app
* Google autoriza a la app
* Se entrega un access token

OAuth2 responde a:

> ¿Puede esta aplicación acceder a este recurso?

No responde a:

> ¿Quién es exactamente el usuario?

---

## 🏗 Componentes OAuth2

```text
Resource Owner → Usuario
Client → Aplicación
Authorization Server → IAM
Resource Server → API
```

---

## 🔑 Resultado principal

OAuth2 emite:

* **Access Token**

Ese token se usa para acceder a APIs.

---

# 🆔 3️⃣ OpenID Connect (Autenticación)

OIDC es una capa encima de OAuth2.

Añade:

* Identidad del usuario
* ID Token (JWT)
* Endpoint /userinfo
* Descubrimiento automático

OIDC responde a:

> ¿Quién es el usuario autenticado?

---

## 🔑 Resultado principal

OIDC emite:

* **ID Token**
* Access Token
* (Opcional) Refresh Token

---

# 🔎 4️⃣ Diferencia clave

| OAuth2              | OIDC                      |
| ------------------- | ------------------------- |
| Autorización        | Autenticación             |
| Access Token        | ID Token + Access Token   |
| No define identidad | Define identidad estándar |
| API access          | Login moderno             |

---

## 🧾 5️⃣ Visualmente

### OAuth2 puro

```text
App → Authorization Server → Access Token → API
```

---

### OIDC

```text
App → Authorization Server
        ↓
   ID Token + Access Token
```

ID Token = identidad
Access Token = permiso

---

## 🧠 6️⃣ ¿Por qué hoy casi todo usa OIDC?

Porque las SPA necesitan:

* Saber quién es el usuario
* Mostrar nombre/email
* Gestionar sesión
* Validar identidad

OAuth2 solo no basta.

---

## 🔥 7️⃣ En Keycloak

**Keycloak** implementa:

* OAuth2
* OIDC
* SAML

Pero en aplicaciones modernas:

👉 Usamos **OIDC + Authorization Code + PKCE**

---

## ⚠️ 8️⃣ Error típico

Muchos dicen:

> “OAuth2 es autenticación”

Incorrecto.

OAuth2 delega acceso.
OIDC autentica identidad.

---

## 🎯 9️⃣ Qué debes retener

* OAuth2 = permisos
* OIDC = identidad
* OIDC usa OAuth2 por debajo
* El login moderno es OIDC

