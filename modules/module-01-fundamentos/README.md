# 📘 Módulo 01 – Fundamentos de IAM y Keycloak

---

## 🎯 Objetivo del módulo

Comprender los fundamentos de:

* IAM moderno
* OAuth 2.0
* OpenID Connect
* JWT
* Flujos de autenticación
* Arquitectura de **Keycloak**

Este módulo construye la base conceptual necesaria para todo el curso.

Sin estos conceptos, el resto es solo configuración mecánica.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Entender qué es IAM y por qué es crítico
* Diferenciar OAuth2 vs OIDC
* Analizar un JWT manualmente
* Comprender Authorization Code Flow
* Entender el rol del Identity Provider
* Explicar la arquitectura básica de Keycloak
* Levantar un entorno local de desarrollo

---

## 📚 Contenidos del módulo

| Archivo                     | Tema                           |
| --------------------------- | ------------------------------ |
| 01-iam-moderno.md           | Qué es IAM moderno             |
| 02-oauth2-vs-oidc.md        | Diferencias OAuth2 y OIDC      |
| 03-tokens.md                | JWT y tipos de tokens          |
| 04-flujos-autenticacion.md  | Authorization Code Flow        |
| 05-arquitectura-keycloak.md | Componentes de Keycloak        |
| 06-instalacion-dev.md       | Instalación en modo desarrollo |

---

## 🏗 Conceptos clave

### 🔐 IAM (Identity & Access Management)

Sistema que gestiona:

* Identidad
* Autenticación
* Autorización
* Emisión de tokens

---

### 🔁 OAuth 2.0

Framework de autorización que define:

* Access tokens
* Flujos de autorización
* Delegación de permisos

---

### 🧾 OpenID Connect

Capa de identidad sobre OAuth2.

Introduce:

* ID Token
* Claims
* UserInfo endpoint

---

### 🧩 JWT (JSON Web Token)

Token firmado que contiene:

* Claims
* Roles
* Identidad del usuario

Emitido por el Identity Provider.

---

### 🏢 Rol de Keycloak

En esta arquitectura:

```text
Usuario
   ↓
Keycloak (IdP)
   ↓
Aplicación
```

Keycloak:

* Autentica
* Emite tokens
* Gestiona usuarios
* Aplica políticas

---

## 🧪 Laboratorios incluidos

Este módulo incluye 5 laboratorios progresivos:

### 🔹 Lab 01 – Entorno Keycloak Dev

* Crear docker-compose
* Definir servicio
* Levantar contenedor
* Acceder a admin console

### 🔹 Lab 02 – Primer Realm

* Crear realm
* Crear usuario
* Probar login UI

### 🔹 Lab 03 – Cliente OIDC

* Crear cliente público
* Configurar redirect URIs
* Revisar endpoints OIDC

### 🔹 Lab 04 – Authorization Code Flow

* Construir URL manual
* Obtener authorization code
* Intercambiar por token

### 🔹 Lab 05 – Análisis de Tokens

* Decodificar JWT
* Analizar claims
* Validar firma con JWKS

---

## 🧠 Modelo mental que debes adquirir

```text
Usuario se autentica
   ↓
Identity Provider valida credenciales
   ↓
IdP emite token firmado
   ↓
Aplicación valida token
   ↓
Acceso concedido
```

Nunca mezclar autenticación con autorización.

---

## ⚠ Errores comunes que debes evitar

* Confundir OAuth2 con autenticación
* No validar firma JWT
* No validar issuer
* Usar flujos inseguros
* No entender diferencia entre access token e ID token

---

## 🚀 Resultado esperado al terminar el módulo

Deberías poder:

✔ Explicar cómo funciona OIDC
✔ Construir manualmente un Authorization Code Flow
✔ Decodificar un JWT sin herramientas
✔ Levantar Keycloak en local
✔ Entender cómo se integra una app con un IdP
