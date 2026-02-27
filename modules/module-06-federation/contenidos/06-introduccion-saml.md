# 📘 Módulo 6 – Federation

## 06 – Introducción a SAML

---

# 🎯 Objetivo

Entender qué es **Security Assertion Markup Language** y cómo lo soporta
**Keycloak** en escenarios corporativos.

Aquí hablamos de entornos legacy y enterprise clásicos.

---

# 🧠 1️⃣ ¿Qué es SAML?

SAML (Security Assertion Markup Language) es un estándar de autenticación basado en:

* XML
* Firmas digitales
* Intercambio de assertions
* Redirecciones navegador

Es anterior a OAuth2 y OIDC.

Muy común en:

* Grandes corporaciones
* Aplicaciones legacy
* SAP
* Portales empresariales antiguos

---

# 🏗 2️⃣ Arquitectura SAML básica

![Image](https://learn.microsoft.com/en-us/entra/architecture/media/authentication-patterns/saml-auth.png)

![Image](https://i.stack.imgur.com/2hTqa.png)

![Image](https://developer.okta.com/img/saml/saml_guidance_saml_flow.png)

![Image](https://wiki.deepnetsecurity.com/download/attachments/35947896/image2020-4-20_16-14-19.png?api=v2\&modificationDate=1587395659000\&version=1)

Flujo típico:

```text
Usuario
   ↓
Service Provider (SP)
   ↓
Redirección al IdP
   ↓
Login
   ↓
Assertion SAML firmada
   ↓
SP valida firma
   ↓
Acceso concedido
```

---

# 🔎 3️⃣ Terminología clave

| Concepto  | Significado                        |
| --------- | ---------------------------------- |
| IdP       | Identity Provider                  |
| SP        | Service Provider                   |
| Assertion | Documento XML con identidad        |
| Metadata  | Configuración intercambiada        |
| ACS URL   | Endpoint donde vuelve la respuesta |

---

# 🔐 4️⃣ ¿Cómo encaja Keycloak?

Keycloak puede actuar como:

* SAML IdP (lo más común)
* SAML SP (menos común)
* Broker SAML → OIDC
* Broker SAML → JWT

Ejemplo típico:

```text
App legacy (SAML)
   ↓
Keycloak (IdP SAML)
   ↓
Usuario autenticado
```

---

# 🔁 5️⃣ SAML vs OIDC

| SAML                    | OIDC             |
| ----------------------- | ---------------- |
| XML                     | JSON             |
| Basado en Browser       | Basado en API    |
| Más pesado              | Más ligero       |
| Muy usado en enterprise | Estándar moderno |
| Assertions XML          | JWT              |

Hoy en proyectos nuevos → OIDC.
Pero en enterprise grande → SAML sigue vivo.

---

# 🧩 6️⃣ Caso real empresarial

Escenario híbrido:

```text
App moderna → OIDC
App legacy → SAML
LDAP → Federation
Azure AD → Brokering
          ↓
       Keycloak
```

Keycloak unifica todo.

---

# 🛠 7️⃣ Configuración básica SAML en Keycloak

Para crear cliente SAML:

```text
Clients → Create → Client Protocol = SAML
```

Configurar:

* Client ID
* ACS URL
* NameID format
* Firmas requeridas

Intercambiar metadata XML con el SP.

---

# 🔒 8️⃣ Seguridad en SAML

✔ Firmar assertions
✔ Validar certificados
✔ Usar HTTPS obligatorio
✔ Validar destino (ACS URL)
✔ Controlar replay attacks

SAML es potente pero complejo.

---

# 🧠 9️⃣ Modelo mental correcto

```text
OIDC = APIs modernas
SAML = Enterprise legacy
Keycloak = Puente universal
```

Keycloak permite que:

* App moderna no sepa que el login fue SAML
* App legacy no sepa que el backend usa JWT

---

# 🎯 Qué debes retener

* SAML es estándar enterprise clásico
* Usa XML y assertions firmadas
* Keycloak puede actuar como IdP SAML
* Puede coexistir con OIDC
* Es clave en entornos híbridos

---

Con esto cierras Module 6 completo:

✔ LDAP
✔ Mappers
✔ Identity Brokering
✔ Arquitecturas híbridas
✔ SAML