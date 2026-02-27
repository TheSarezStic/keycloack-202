# 📘 Módulo 6 – Federation

## 04 – Identity Brokering

---

# 🎯 Objetivo

Entender cómo usar
**Keycloak**
como intermediario entre tu aplicación y otros proveedores de identidad:

* Azure AD
* Google
* Otro Keycloak
* SAML corporativo
* OIDC externo

Aquí ya hablamos de federación de login, no solo de usuarios.

---

# 🧠 1️⃣ ¿Qué es Identity Brokering?

Identity Brokering significa:

> Keycloak delega la autenticación a otro Identity Provider (IdP).

Pero:

* Keycloak sigue emitiendo el JWT final.
* Tus aplicaciones solo confían en Keycloak.
* El IdP externo nunca habla directamente con tu app.

---

# 🏗 2️⃣ Arquitectura conceptual

![Image](https://wjw465150.gitbooks.io/keycloak-documentation/content/server_admin/images/identity_broker_flow.png)

![Image](https://infosec.mozilla.org/guidelines/assets/images/OIDC_diagram.png)

![Image](https://techcommunity.microsoft.com/t5/s/gxcuf89792/images/bS00MTc0MjM4LTU5NDE4OGlBREJENjdFODBENjU5MTc5?image-dimensions=999x999\&revision=6)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1358/format%3Awebp/1%2AUFOMCUyl-xKDVWHddzMWHw.png)

Flujo típico:

```text id="flow1"
Usuario
   ↓
Keycloak
   ↓
Azure AD / Google / Otro IdP
   ↓
Autenticación
   ↓
Keycloak recibe identidad
   ↓
Keycloak emite JWT propio
   ↓
Aplicación
```

---

# 🔎 3️⃣ Diferencia con User Federation

| User Federation            | Identity Brokering               |
| -------------------------- | -------------------------------- |
| Consulta LDAP              | Redirige login completo          |
| Password validado por LDAP | Password nunca pasa por Keycloak |
| Usuario vive en directorio | Usuario vive en IdP externo      |
| Sin redirección visible    | Login redirige al IdP            |

Federation = backend directory
Brokering = login externo completo

---

# 🔐 4️⃣ Tipos de IdP soportados

Keycloak puede actuar como broker para:

* OpenID Connect (OIDC)
* SAML 2.0
* Social providers (Google, GitHub)
* Otro Keycloak

---

# 🛠 5️⃣ Configuración básica (OIDC externo)

Ir a:

```text
Identity Providers → Add provider → OpenID Connect v1.0
```

Configurar:

| Campo         | Ejemplo                                                                                                                                                              |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Alias         | azure                                                                                                                                                                |
| Discovery URL | [https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration](https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration) |
| Client ID     | xxxx                                                                                                                                                                 |
| Client Secret | xxxx                                                                                                                                                                 |

---

# 🔁 6️⃣ Flujo real en login

Usuario entra a tu app:

```text id="flow2"
Login
   ↓
Selecciona "Login con Azure"
   ↓
Redirección a Azure
   ↓
Login en Azure
   ↓
Redirección a Keycloak
   ↓
Keycloak crea usuario local
   ↓
Emite JWT
```

La app nunca ve Azure directamente.

---

# 👤 7️⃣ Creación automática de usuario

Opciones:

* Importar usuario automáticamente
* Vincular con usuario existente
* Forzar verificación email

Muy importante en entornos empresariales.

---

# 🔄 8️⃣ Vinculación de cuentas

Escenario:

```text id="link1"
Usuario ya existe en Keycloak
   ↓
Login con IdP externo
   ↓
Keycloak vincula identidad externa con usuario local
```

Evita duplicados.

---

# 🔥 9️⃣ Ventajas del brokering

✔ SSO corporativo real
✔ No gestionas passwords externos
✔ Puedes añadir MFA encima
✔ Centralizas emisión de tokens
✔ Puedes cambiar IdP sin tocar apps

---

# ⚠️ 10️⃣ Errores comunes

❌ No validar issuer externo
❌ No configurar redirect URI correctamente
❌ No mapear atributos (email, username)
❌ No definir política de creación de usuario
❌ No usar HTTPS

---

# 🧠 11️⃣ Modelo mental correcto

```text id="mental1"
IdP externo = Autenticación
Keycloak = Orquestador + Emisor JWT
Aplicación = Confía solo en Keycloak
```

Nunca mezclar confianza directa.

---

# 🎯 Qué debes retener

* Identity Brokering delega login
* Keycloak sigue emitiendo JWT
* Diferente de LDAP federation
* Permite SSO entre sistemas
* Es clave en integración corporativa
