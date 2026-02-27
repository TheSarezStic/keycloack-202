# 📘 Módulo 06 – Federation, LDAP y SSO Empresarial

---

## 🎯 Objetivo del módulo

Aprender a integrar
**Keycloak**
con sistemas externos de identidad:

* LDAP / Active Directory
* Identity Brokering (Azure AD, Google, otro IdP)
* Arquitecturas híbridas
* SAML empresarial

Este módulo convierte a Keycloak en un **hub de identidad corporativa**.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Integrar LDAP correctamente
* Mapear atributos y grupos
* Diseñar sincronización segura
* Configurar Identity Brokering (OIDC y SAML)
* Diseñar arquitectura híbrida
* Entender diferencias entre Federation y Brokering

Este es el módulo clave para entornos empresariales reales.

---

## 📚 Contenidos del módulo

| Archivo                      | Tema                         |
| ---------------------------- | ---------------------------- |
| 01-user-federation.md        | Concepto de federación       |
| 02-ldap.md                   | Integración LDAP paso a paso |
| 03-sincronizacion.md         | Mappers y sincronización     |
| 04-identity-brokering.md     | Delegación de login          |
| 05-arquitecturas-hibridas.md | Diseño empresarial           |
| 06-introduccion-saml.md      | Integración con SAML         |

---

# 🌍 User Federation

Permite que Keycloak:

* No almacene credenciales
* Delegue autenticación en LDAP
* Siga emitiendo JWT
* Aplique roles locales

Modelo:

```text
Usuario
   ↓
Keycloak
   ↓
LDAP
   ↓
Validación password
   ↓
JWT emitido por Keycloak
```

LDAP valida identidad.
Keycloak controla autorización.

---

# 🔐 LDAP / Active Directory

Integración típica:

✔ Bind DN técnico
✔ Base DN correcta
✔ LDAPS en producción
✔ Import Users activado
✔ Mappers configurados

LDAP sigue siendo la fuente de verdad organizativa.

---

# 🔄 Mappers y Sincronización

Permiten:

* Mapear atributos (mail → email)
* Importar grupos
* Sincronizar cambios
* Aplicar ABAC (Attribute-Based Access Control)

Modelo recomendado:

```text
LDAP Groups → Keycloak Groups → Realm Roles → Client Roles
```

Separación limpia entre organización y permisos técnicos.

---

# 🔁 Identity Brokering

Permite delegar login completo a:

* Azure AD
* Google
* Otro Keycloak
* SAML corporativo

Modelo:

```text
Usuario
   ↓
Keycloak
   ↓
IdP externo
   ↓
Identidad validada
   ↓
Keycloak emite JWT propio
```

Las aplicaciones solo confían en Keycloak.

---

# 🏗 Arquitecturas híbridas

Escenario empresarial típico:

```text
Usuarios internos → LDAP
Usuarios externos → Azure AD
Aplicaciones modernas → OIDC
Aplicaciones legacy → SAML
           ↓
        Keycloak
```

Keycloak actúa como Identity Hub.

---

# 🧾 Introducción a SAML

SAML es un estándar más antiguo basado en XML.

Características:

* Assertions firmadas
* Redirecciones browser
* Muy usado en aplicaciones enterprise legacy

Keycloak puede actuar como:

* IdP SAML
* SP SAML
* Broker SAML ↔ OIDC

---

# 🧪 Laboratorios incluidos

---

## 🔹 Lab 01 – OpenLDAP

* Definir servicio LDAP
* Levantar LDAP
* Validar conexión

---

## 🔹 Lab 02 – Integración LDAP

* Configurar federación
* Sincronizar usuarios
* Validar login LDAP

---

## 🔹 Lab 03 – Identity Brokering

* Configurar segundo realm
* Configurar IdP externo
* Validar login delegado

---

# 🧠 Modelo mental correcto

```text
Fuente de identidad ≠ Emisor de token
```

* LDAP valida credenciales
* Azure valida login externo
* Keycloak siempre emite el JWT final

Nunca mezclar responsabilidades.

---

# ⚠ Errores comunes

❌ No usar LDAPS en producción
❌ No mapear grupos correctamente
❌ No definir política de vinculación de cuentas
❌ No validar issuer externo
❌ No separar realms adecuadamente

---

# 🔒 Buenas prácticas

✔ LDAP en red privada
✔ Cuenta bind con permisos mínimos
✔ Sincronización periódica
✔ Validar claims externos
✔ Controlar creación automática de usuarios

---

# 🚀 Resultado esperado

Al terminar este módulo deberías poder:

✔ Integrar LDAP real
✔ Delegar login a Azure/Google
✔ Diseñar arquitectura híbrida
✔ Entender SAML básico
✔ Centralizar identidad empresarial

