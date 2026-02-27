# 📘 Módulo 7 – Security

## 01 – PKCE Deep Dive (Nivel Seguridad)

---

# 🎯 Objetivo

Entender en profundidad por qué PKCE es crítico y qué ataques evita cuando usamos:

* **OAuth 2.0**
* **OpenID Connect**
* Con **Keycloak**
* En SPA públicas (Angular)

Aquí ya hablamos en términos de amenaza y mitigación.

---

# 🧠 1️⃣ El problema original del Authorization Code

En OAuth2 clásico:

```text
App → Authorization Server → Code → Token
```

El `code` viaja por el navegador.

Si un atacante intercepta ese code:

→ Puede intercambiarlo por tokens.

En apps públicas (SPA), no existe `client_secret`.

Por tanto, el code queda vulnerable.

---

# 🔐 2️⃣ Ataque: Authorization Code Interception

Escenario:

```text
Usuario inicia login
   ↓
Code vuelve a SPA
   ↓
Malware intercepta URL
   ↓
Atacante intercambia code por token
```

Sin PKCE, esto es posible en clientes públicos.

---

# 🔒 3️⃣ Cómo PKCE lo evita

PKCE introduce:

* code_verifier (secreto temporal)
* code_challenge (hash enviado en auth request)

Flujo real:

```text
SPA genera code_verifier
   ↓
Envía code_challenge = SHA256(verifier)
   ↓
Usuario login
   ↓
Code vuelve
   ↓
SPA envía code + verifier
   ↓
Servidor valida hash
```

Si el atacante roba el `code`:

→ No tiene el `code_verifier`.

No puede intercambiarlo.

---

# 🧩 4️⃣ Seguridad criptográfica real

PKCE usa:

* SHA-256
* Base64URL encoding
* Comparación hash determinista

El servidor nunca almacena el verifier en texto plano.

Solo guarda el challenge.

---

# 🏗 5️⃣ Flujo detallado

![Image](https://mintlify.s3.us-west-1.amazonaws.com/auth0/docs/images/cdy7uua7fh8z/3pstjSYx3YNSiJQnwKZvm5/33c941faf2e0c434a9ab1f0f3a06e13a/auth-sequence-auth-code-pkce.png)

![Image](https://images.ctfassets.net/xqb1f63q68s1/5Ud69swfPVJmDLCHukhzOY/47508569d79120dc260b9a4295327578/Authorization_Code_Flow__2_.png)

![Image](https://is.docs.wso2.com/en/6.1.0/assets/img/deploy/authorization-code-grant-type-flow.png)

Secuencia técnica:

```text
1. SPA genera random 128 bytes
2. Crea code_verifier
3. Calcula SHA256 → code_challenge
4. Envía challenge
5. Server guarda challenge
6. Recibe code + verifier
7. Recalcula hash
8. Compara
```

---

# 🔥 6️⃣ Ataques que PKCE mitiga

✔ Code interception
✔ Malicious browser extensions
✔ Reverse proxy leaks
✔ Log exposure accidental

No protege contra:

❌ XSS (si el token ya está en memoria)
❌ Malware en el dispositivo

---

# 🧠 7️⃣ Por qué PKCE es obligatorio hoy

RFC actual recomienda:

* PKCE obligatorio en clientes públicos
* PKCE incluso en clientes confidenciales

Hoy Authorization Code sin PKCE es considerado débil.

---

# ⚙️ 8️⃣ Configuración en Keycloak

En cliente público:

```text
Client → Settings
   ↓
Proof Key for Code Exchange → Required
```

Nunca dejarlo en “Not required” en SPA.

---

# 🚨 9️⃣ Error crítico común

Usar:

```text
Authorization Code Flow
PKCE desactivado
```

En SPA.

Es mala práctica de seguridad moderna.

---

# 🧠 10️⃣ Modelo mental final

```text
Client confidential → usa client_secret
Client público → usa PKCE
PKCE reemplaza secreto
```

PKCE convierte cliente público en cliente verificable.

---

# 🎯 Qué debes retener

* PKCE protege el Authorization Code
* Es obligatorio en SPA modernas
* Sustituye client_secret en clientes públicos
* Mitiga interceptación
* Debe estar siempre activado