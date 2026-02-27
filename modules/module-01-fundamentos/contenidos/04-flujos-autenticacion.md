# 📘 Módulo 1 – Fundamentos

## 04 – Flujos de Autenticación

---

# 🧠 1️⃣ ¿Qué es un flujo?

Un flujo define:

* Cómo se autentica el usuario
* Cómo se obtiene el token
* Qué actores participan
* Qué nivel de seguridad tiene

Todos los flujos están definidos en
**OAuth 2.0**
y extendidos por
**OpenID Connect**

---

# 🔥 2️⃣ Authorization Code Flow (EL IMPORTANTE)

👉 Es el flujo estándar moderno.

### Escenario típico:

SPA + Backend + Keycloak

---

## 🔁 Flujo simplificado

```text id="x8sh2a"
1. Usuario → App
2. App → redirige a IAM
3. Usuario se autentica
4. IAM → devuelve code
5. App intercambia code por tokens
```

---

## 🎯 Ventajas

* No expone tokens en URL
* Seguro
* Compatible con PKCE
* Estándar recomendado

---

# 🔐 3️⃣ Authorization Code + PKCE (para SPA)

PKCE = Proof Key for Code Exchange

Protege contra:

* Intercepción del authorization code
* Ataques en apps públicas

Hoy es obligatorio en:

* SPA
* Mobile
* Aplicaciones públicas

---

# 🤖 4️⃣ Client Credentials Flow

👉 No hay usuario.

Es máquina a máquina.

```text id="k2pt8m"
Service A → IAM → Access Token → Service B
```

Uso típico:

* Backend calling backend
* Microservicios
* Jobs automáticos

No hay ID Token.

---

# 🪪 5️⃣ Resource Owner Password (Obsoleto)

```text id="0eowbq"
App recoge username/password → IAM → Token
```

Problemas:

* La app ve la contraseña
* Rompe modelo moderno
* No recomendado

Hoy casi desaparecido.

---

# 📦 6️⃣ Implicit Flow (Deprecated)

Antes usado en SPA.

Hoy:

❌ No recomendado
❌ Inseguro
❌ Sustituido por Code + PKCE

---

# 🏗 7️⃣ Visual comparativo

## Authorization Code

```text id="7vh7v5"
Usuario → IAM → Code → Tokens
```

---

## Client Credentials

```text id="q8mp3s"
App → IAM → Access Token
```

---

# 🧠 8️⃣ ¿Cuál usar y cuándo?

| Escenario           | Flujo                     |
| ------------------- | ------------------------- |
| SPA Angular         | Authorization Code + PKCE |
| Backend tradicional | Authorization Code        |
| Microservicios      | Client Credentials        |
| Login moderno       | OIDC                      |

---

# 🔐 9️⃣ En Keycloak

**Keycloak** permite configurar:

* Tipo de cliente (public / confidential)
* Activar PKCE
* Activar service account
* Definir flujos personalizados

---

# 🚨 10️⃣ Error típico

Pensar que:

> “Todos los clientes usan el mismo flujo”

Incorrecto.

Cada tipo de aplicación requiere un flujo diferente.

---

# 🎯 Qué debes retener

* El flujo define cómo se obtienen tokens
* Authorization Code + PKCE es el estándar actual
* Client Credentials es para máquina a máquina
* Implicit está muerto
* Password flow no se recomienda