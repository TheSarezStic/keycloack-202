# 📘 Módulo 5 – MFA

## 01 – Authentication Flows

---

# 🎯 Objetivo

Entender cómo funciona el motor interno de autenticación de
**Keycloak**:

* Qué es un Authentication Flow
* Cómo se encadenan pasos
* Cómo personalizar login
* Cómo preparar MFA

Aquí dejamos OAuth y entramos en el corazón del login.

---

# 🧠 1️⃣ ¿Qué es un Authentication Flow?

Un Authentication Flow es:

> Una secuencia de pasos que se ejecutan durante el login.

Ejemplo básico:

```text id="flow1"
Username
   ↓
Password
   ↓
Success
```

Pero puede ser mucho más complejo.

---

# 🏗 2️⃣ Estructura interna

![Image](https://www.keycloak.org/docs/latest/authorization_services/images/authz-arch-overview.png)

![Image](https://miro.medium.com/1%2A1McvnvrW6wh37ECYpmTSxw.png)

![Image](https://static.wixstatic.com/media/8d2ee4_6019a237021b4408ad66d0dc8dbc035d~mv2.png/v1/fill/w_980%2Ch_754%2Cal_c%2Cq_90%2Cusm_0.66_1.00_0.01%2Cenc_avif%2Cquality_auto/8d2ee4_6019a237021b4408ad66d0dc8dbc035d~mv2.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AGthnBLACrzV6xcBGDPoeXg.png)

Un flow contiene:

* Executions (pasos)
* Subflows
* Reglas (Required, Alternative, Disabled)

---

# 🔎 3️⃣ Tipos de flows principales

En Keycloak existen flows por defecto:

* Browser
* Direct Grant
* Client Authentication
* Registration
* Reset Credentials

El más importante para SPA:

👉 **Browser Flow**

---

# 🔐 4️⃣ Browser Flow por defecto

Flujo simplificado:

```text id="browser1"
Cookie
   ↓
Username Password Form
   ↓
Success
```

Significa:

1. Si ya tiene cookie → login automático
2. Si no → muestra formulario
3. Si credenciales correctas → login

---

# ⚙️ 5️⃣ Qué es una Execution

Cada paso es una execution.

Ejemplos:

* Username Password Form
* OTP Form
* Identity Provider Redirector
* Conditional OTP

Cada execution tiene estado:

| Tipo        | Significado   |
| ----------- | ------------- |
| Required    | Obligatorio   |
| Alternative | Uno de varios |
| Disabled    | No se ejecuta |

---

# 🔁 6️⃣ Subflows

Un subflow es un grupo de pasos.

Ejemplo MFA:

```text id="subflow1"
Username Password
   ↓
OTP Subflow
   ├── OTP Form
   └── Backup Code
```

Esto permite lógica condicional.

---

# 🧠 7️⃣ Modelo mental importante

```text id="mental1"
Flow = Árbol
Execution = Nodo
Subflow = Rama
```

No es una lista lineal simple.

---

# 🔥 8️⃣ Por qué esto es potente

Permite:

* MFA condicional
* Login diferente por cliente
* Login diferente por grupo
* Forzar cambio de password
* Integrar LDAP
* Integrar SAML
* Integrar IdP externo

Todo sin tocar código de tu app.

---

# ⚠️ 9️⃣ Error típico

Modificar el flow por defecto directamente.

Siempre:

1️⃣ Duplicar flow
2️⃣ Modificar copia
3️⃣ Asignarlo al realm

Así no rompes configuración base.

---

# 🎯 10️⃣ Qué debes retener

* Authentication Flow controla login
* Browser Flow es el principal
* Se compone de executions y subflows
* Permite lógica avanzada
* Es la base para MFA
