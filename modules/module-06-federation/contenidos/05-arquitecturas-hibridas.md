# 📘 Módulo 6 – Federation

## 05 – Arquitecturas híbridas (LDAP + Brokering + MFA)

---

# 🎯 Objetivo

Diseñar una arquitectura empresarial combinando:

* LDAP corporativo
* Identity Brokering (Azure AD / Google / otro IdP)
* MFA
* Roles locales
* Emisión centralizada de JWT

Todo orquestado por
**Keycloak**

---

# 🧠 1️⃣ Escenario real corporativo

Empresa multinacional:

* Empleados internos → Active Directory
* Partners externos → Azure AD B2B
* Usuarios SaaS → Google
* Aplicaciones internas → JWT propios
* Seguridad obligatoria → MFA

No existe un único sistema de identidad.

Necesitas un hub.

---

# 🏗 2️⃣ Arquitectura híbrida típica

![Image](https://techcommunity.microsoft.com/t5/s/gxcuf89792/images/bS00MTc0MjM4LTU5NDE4OGlBREJENjdFODBENjU5MTc5?image-dimensions=999x999\&revision=6)

![Image](https://wjw465150.gitbooks.io/keycloak-documentation/content/server_admin/images/identity_broker_flow.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AlWQNQdw8U4olam3ZZb3STw.png)

![Image](https://miro.medium.com/1%2AN1QKeDhRXPeOCNC1DIoH1A.gif)

Modelo conceptual:

```text
Usuarios internos → LDAP
Usuarios externos → Azure AD
Usuarios sociales → Google
          ↓
        Keycloak
          ↓
     Emite JWT único
          ↓
     Aplicaciones
```

Keycloak es el punto único de confianza.

---

# 🔎 3️⃣ Flujo combinado

## 👔 Usuario interno

```text
Login
  ↓
Keycloak
  ↓
LDAP bind
  ↓
JWT emitido
```

---

## 🌐 Usuario externo

```text
Login
  ↓
Keycloak
  ↓
Redirección a Azure
  ↓
Autenticación externa
  ↓
Keycloak recibe identidad
  ↓
JWT emitido
```

---

# 🔐 4️⃣ MFA en arquitectura híbrida

Tienes dos opciones:

### Opción A – MFA en IdP externo

Azure ya fuerza MFA.

Keycloak solo recibe identidad ya verificada.

---

### Opción B – MFA central en Keycloak

Keycloak añade TOTP encima de LDAP o Azure.

Ventaja:
Control homogéneo.

---

# 🧩 5️⃣ Modelo recomendado empresarial

Separar capas:

```text
Fuente de identidad → LDAP / Azure
Control de autenticación → Keycloak
Control de autorización → Roles + Policies
Token estándar → JWT emitido por Keycloak
```

---

# 🔥 6️⃣ Beneficio clave

Tus aplicaciones:

❌ No saben si el usuario viene de LDAP
❌ No saben si viene de Azure
❌ No saben si viene de Google

Solo validan:

✔ JWT emitido por Keycloak

Desacoplamiento total.

---

# 🧠 7️⃣ Estrategia de roles correcta

Modelo limpio:

```text
LDAP Groups → Keycloak Groups
Azure Claims → Mappers
Realm Roles → Modelo organizativo
Client Roles → Permisos técnicos
```

No mezclar identidad con autorización técnica.

---

# ⚠️ 8️⃣ Problemas comunes en híbrido

❌ Duplicación de usuarios
❌ Conflicto de emails
❌ Username diferente entre sistemas
❌ No definir política de vinculación
❌ No definir prioridad de IdP

---

# 🔄 9️⃣ Account Linking

Escenario:

```text
Usuario LDAP
   ↓
Login con Azure
   ↓
Keycloak detecta mismo email
   ↓
Vincula cuentas
```

Evita duplicados.

Debe planificarse cuidadosamente.

---

# 🔒 10️⃣ Seguridad crítica en híbrido

✔ Definir IdP default claramente
✔ Restringir redirecciones abiertas
✔ Validar issuer externo
✔ Controlar mapeo de claims
✔ Revisar expiraciones de sesión

---

# 🧠 11️⃣ Modelo mental final de Federation

```text
Identidad puede venir de muchos sitios.
Confianza vive en Keycloak.
Autorización vive en Keycloak.
Tokens viven en Keycloak.
```

Keycloak es el hub.

---

# 🎯 Qué debes retener

* Arquitectura híbrida es común en empresa
* Keycloak actúa como Identity Hub
* Apps solo confían en JWT local
* LDAP y Brokering pueden coexistir
* MFA puede centralizarse

---

Con esto cierras Module 6 completo:

✔ LDAP
✔ Mappers
✔ Identity Brokering
✔ Arquitectura híbrida
