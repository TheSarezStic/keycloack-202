# 📘 Módulo 03 – Integración Frontend (Angular + OIDC)

---

## 🎯 Objetivo del módulo

Integrar una aplicación SPA en Angular con
**Keycloak**
usando:

* OpenID Connect
* Authorization Code Flow + PKCE
* Gestión de tokens en navegador
* Guards
* Control de sesión

Aquí pasamos de teoría a aplicación real.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Integrar `keycloak-js` en Angular
* Configurar Authorization Code + PKCE
* Proteger rutas con Guards
* Gestionar el ciclo de vida del token
* Implementar refresh automático
* Configurar logout centralizado
* Evitar errores comunes en SPA

---

## 📚 Contenidos del módulo

| Archivo                       | Tema                           |
| ----------------------------- | ------------------------------ |
| 01-keycloak-js.md             | Librería oficial para frontend |
| 02-authorization-code-pkce.md | Flujo seguro para SPA          |
| 03-guards.md                  | Protección de rutas            |
| 04-token-lifecycle.md         | Ciclo de vida del token        |
| 05-refresh-automatico.md      | Renovación automática          |
| 06-logout-centralizado.md     | Logout completo OIDC           |

---

# 🏗 Arquitectura del módulo

```text id="arch1"
Usuario
   ↓
Angular SPA
   ↓
Keycloak
   ↓
JWT
   ↓
Angular consume APIs
```

La SPA nunca guarda contraseña.
Solo gestiona tokens.

---

# 🔐 Authorization Code + PKCE

En SPA modernas:

✔ Authorization Code
✔ PKCE obligatorio
✔ Cliente público
✔ Sin client_secret

Flujo:

```text id="flow1"
SPA genera code_verifier
   ↓
Redirige a Keycloak
   ↓
Usuario login
   ↓
Keycloak devuelve code
   ↓
SPA intercambia por token
```

PKCE protege el code contra interceptación.

---

# 🛡 Guards

Permiten:

* Proteger rutas
* Redirigir si no autenticado
* Verificar roles
* Controlar acceso condicional

Modelo típico:

```text id="guard1"
Ruta protegida
   ↓
Guard verifica autenticación
   ↓
Permite o redirige
```

Nunca confiar solo en frontend para autorización real.

---

# 🔄 Ciclo de vida del token

Tipos de tokens en SPA:

* Access Token
* Refresh Token
* ID Token

Importante:

✔ Access token corto
✔ Refresh automático antes de expirar
✔ No guardar tokens en localStorage si es posible

---

# 🔥 Refresh automático

`keycloak-js` permite:

```text id="refresh1"
updateToken(minValidity)
```

Estrategia:

* Verificar expiración
* Renovar antes de expirar
* Evitar logout inesperado

---

# 🚪 Logout centralizado

Logout correcto debe:

1️⃣ Invalidar sesión en Keycloak
2️⃣ Limpiar tokens en SPA
3️⃣ Redirigir a URL segura

No basta con borrar variables locales.

---

# 🧪 Laboratorios incluidos

---

## 🔹 Lab 01 – Crear Angular App

* Generar proyecto
* Configurar base
* Ejecutar aplicación

---

## 🔹 Lab 02 – Integrar Keycloak

* Instalar keycloak-js
* Configurar init
* Validar login

---

## 🔹 Lab 03 – Guards

* Crear guard
* Proteger rutas
* Validar redirecciones

---

## 🔹 Lab 04 – Token UI

* Mostrar claims
* Simular expiración
* Implementar refresh

---

## 🔹 Lab 05 – Logout

* Logout centralizado
* Validar cierre de sesión real

---

# 🧠 Modelo mental correcto

```text id="mental1"
Frontend gestiona experiencia.
Backend valida permisos.
Keycloak emite identidad.
```

Nunca confiar únicamente en frontend.

---

# ⚠ Errores comunes

❌ No activar PKCE
❌ Usar implicit flow
❌ No validar expiración
❌ Guardar tokens en localStorage sin criterio
❌ No proteger redirect URIs

---

# 🚀 Resultado esperado

Al terminar este módulo deberías poder:

✔ Integrar SPA con Keycloak correctamente
✔ Usar Authorization Code + PKCE
✔ Proteger rutas con guards
✔ Gestionar refresh de tokens
✔ Implementar logout OIDC correcto
