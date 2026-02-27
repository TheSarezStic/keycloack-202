# 📘 Módulo 1 – Fundamentos

## 01 – IAM moderno

---

## 🧠 1️⃣ ¿Qué es IAM?

IAM = **Identity and Access Management**

Es el conjunto de procesos y tecnologías que permiten:

* 👤 Identificar usuarios
* 🔐 Autenticarlos
* 🎯 Autorizar qué pueden hacer
* 📜 Auditar accesos

Hoy casi siempre se implementa con soluciones como
**Keycloak**, Auth0, Azure AD, etc.

---

## 🔥 2️⃣ Problema del modelo antiguo

Antes:

```text
App → tabla usuarios → password en DB → sesión en servidor
```

Problemas:

* Passwords duplicadas en cada app
* No hay SSO
* Difícil escalar
* Difícil federar con LDAP / terceros
* Seguridad débil

Cada aplicación era su propio “mini sistema de identidad”.

---

## 🚀 3️⃣ Modelo IAM moderno

Ahora:

```text
Frontend → IAM → Token → Backend
```

La aplicación ya **no gestiona credenciales directamente**.

El IAM:

* Centraliza autenticación
* Emite tokens (JWT)
* Gestiona roles
* Permite SSO
* Permite MFA
* Permite federación

---

## 🔑 4️⃣ Conceptos clave

### 🔹 Autenticación

Demostrar quién eres.

> Login correcto → identidad confirmada.

---

### 🔹 Autorización

Determinar qué puedes hacer.

> ¿Puede este usuario borrar facturas?

---

### 🔹 Identity Provider (IdP)

Sistema que autentica usuarios.

Ejemplo: Keycloak.

---

### 🔹 Service Provider (SP)

Aplicación que confía en el IdP.

Ejemplo: tu Angular o API Node.

---

## 🏗 5️⃣ Arquitectura moderna

![Image](https://bok.idpro.org/article/id/38/image9.png)

![Image](https://docs.oracle.com/cd/E82085_01/160027/JOS%20Implementation%20Guide/Output/img/oauth2-arch.png)

![Image](https://miro.medium.com/1%2AquwFs1fFCvTvLT80e_QHVA.png)

![Image](https://s3.amazonaws.com/onelogin-screenshots/dev_site/images/oidc-basic-flow.png)

Flujo típico:

1. Usuario entra en app
2. App redirige a IAM
3. IAM autentica
4. IAM devuelve token
5. App usa token para llamar APIs

---

## 🔐 6️⃣ Ventajas del modelo moderno

* ✅ Single Sign-On (SSO)
* ✅ MFA centralizado
* ✅ Roles globales
* ✅ No guardar passwords en cada app
* ✅ Escalabilidad
* ✅ Integración con LDAP / Google / Azure
* ✅ Delegación con OAuth2

---

## ⚠️ 7️⃣ Cambio mental importante

La aplicación ya no controla la sesión.

El token es la sesión.

La API no pregunta:

> “¿Está logueado?”

Pregunta:

> “¿Este token es válido y tiene permisos?”

---

## 🧩 8️⃣ Stack típico actual

* Frontend SPA (Angular / React)
* IAM (Keycloak)
* API REST
* JWT
* HTTPS obligatorio

---

## 🎯 9️⃣ Objetivo del curso

En este módulo vas a entender:

* Por qué IAM es crítico
* Cómo funciona OAuth2 y OIDC
* Qué es realmente un token
* Cómo Keycloak implementa todo esto
