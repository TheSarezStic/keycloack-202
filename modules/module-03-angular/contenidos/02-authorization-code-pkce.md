# 📘 Módulo 3 – Angular

## 02 – Authorization Code + PKCE (Deep Dive)

---

# 🎯 Objetivo

Entender exactamente qué ocurre cuando una SPA Angular se autentica con:

* **OpenID Connect**
* Sobre **OAuth 2.0**
* Usando Authorization Code + PKCE
* Con **Keycloak**

Aquí ya bajamos a nivel de red.

---

# 🧠 1️⃣ ¿Por qué PKCE existe?

Problema clásico:

En una SPA no puedes guardar un `client_secret`.

Si alguien intercepta el authorization code,
podría intercambiarlo por tokens.

PKCE protege contra eso.

---

# 🔐 2️⃣ ¿Qué es PKCE?

PKCE = Proof Key for Code Exchange

Añade dos elementos:

* code_verifier
* code_challenge

---

# 🏗 3️⃣ Flujo completo paso a paso

![Image](https://mintlify.s3.us-west-1.amazonaws.com/auth0/docs/images/cdy7uua7fh8z/3pstjSYx3YNSiJQnwKZvm5/33c941faf2e0c434a9ab1f0f3a06e13a/auth-sequence-auth-code-pkce.png)

![Image](https://images.ctfassets.net/xqb1f63q68s1/5Ud69swfPVJmDLCHukhzOY/47508569d79120dc260b9a4295327578/Authorization_Code_Flow__2_.png)

![Image](https://www.descope.com/_next/image?q=75\&url=https%3A%2F%2Fimages.ctfassets.net%2Fxqb1f63q68s1%2F63WVIYgsFsoLm3M2D1uV9K%2Fae1a1e1452881d530c85e99babe81efb%2FAuthorization_Code_Flow_With_PKCE.png\&w=3840)

![Image](https://developer.transmitsecurity.com/assets/auth_oidc_pkce.be0171123fc3a5b7b9a38dbaf20d55c9c7d909b434f2e7a717ccd34bb82a1141.2397ab79.png)

---

## 🔁 Paso 1 – Angular genera code_verifier

Ejemplo:

```text id="b9s1tr"
dGhpc2lzYXJhbmRvbXN0cmluZw==
```

Luego calcula:

```text id="u5m2xq"
code_challenge = SHA256(code_verifier)
```

---

## 🔁 Paso 2 – Redirección a Keycloak

Angular redirige a:

```text id="k2r9vp"
/protocol/openid-connect/auth?
response_type=code
client_id=angular-app
code_challenge=xxxx
code_challenge_method=S256
redirect_uri=...
```

---

## 🔁 Paso 3 – Usuario hace login

Keycloak:

* Valida credenciales
* Genera authorization code
* Redirige de vuelta:

```text id="q8zt1l"
http://localhost:4200?code=abc123
```

---

## 🔁 Paso 4 – Angular intercambia el code

Hace POST a:

```text id="v1cx7w"
/protocol/openid-connect/token
```

Con:

```text id="n4yp8z"
grant_type=authorization_code
code=abc123
code_verifier=original_value
```

---

## 🔐 Paso 5 – Keycloak valida

Keycloak:

1. Aplica SHA256 al code_verifier
2. Compara con code_challenge original
3. Si coincide → emite tokens

---

# 🎁 4️⃣ Resultado final

Angular recibe:

```json id="c7tz2p"
{
  "access_token": "...",
  "id_token": "...",
  "refresh_token": "..."
}
```

Y ya está autenticado.

---

# 🧠 5️⃣ ¿Por qué es seguro?

Porque aunque alguien robe el `code`:

* No tiene el `code_verifier`
* No puede intercambiarlo por tokens

PKCE evita el ataque “authorization code interception”.

---

# 🔥 6️⃣ Diferencia con Implicit Flow

Antes:

```text id="r4k8tu"
Tokens en URL
```

Ahora:

```text id="m8v3ql"
Solo code en URL
Tokens vía POST seguro
```

Mucho más seguro.

---

# ⚠️ 7️⃣ Errores típicos

❌ No activar PKCE en cliente público
❌ No configurar redirect URI exacta
❌ Permitir wildcards inseguros
❌ No usar HTTPS en producción

---

# 🧠 8️⃣ Modelo mental correcto

```text id="y2pt9c"
SPA no tiene secreto
PKCE sustituye al secreto
Code es temporal
Tokens se obtienen vía POST
```

---

# 🎯 Qué debes retener

* PKCE protege el Authorization Code Flow
* Es obligatorio en SPA modernas
* Sustituye client_secret
* Usa SHA256
* Evita interceptación
