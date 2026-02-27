# 📘 Módulo 5 – MFA

## 03 – MFA con TOTP

---

# 🎯 Objetivo

Implementar autenticación de doble factor (2FA) usando:

* TOTP
* App autenticadora (Google Authenticator, Microsoft Authenticator, etc.)
* Configuración profesional en
  **Keycloak**

---

# 🧠 1️⃣ ¿Qué es TOTP?

TOTP = Time-based One-Time Password

Es un código:

* De 6 dígitos
* Válido ~30 segundos
* Generado desde app autenticadora
* Basado en una clave secreta compartida

Estándar definido por RFC 6238.

---

# 🔐 2️⃣ Cómo funciona internamente

Cuando el usuario configura TOTP:

1️⃣ Keycloak genera un secreto
2️⃣ Muestra QR
3️⃣ Usuario lo escanea con app
4️⃣ App genera códigos temporales
5️⃣ En login se pide ese código

---

# 🏗 3️⃣ Flujo completo con MFA

```text id="flow1"
Username
   ↓
Password
   ↓
OTP Code
   ↓
Login exitoso
```

---

# 🔧 4️⃣ Activar TOTP en Keycloak

Ir a:

```text id="menu1"
Authentication → Flows → Browser
```

Buscar:

```text id="otp1"
OTP Form
```

Opciones:

* Required → obligatorio
* Alternative → opcional
* Disabled → desactivado

---

# 🔁 5️⃣ Mejor práctica: duplicar flow

Nunca modificar el Browser flow por defecto.

Pasos correctos:

1️⃣ Duplicar Browser flow
2️⃣ Activar OTP Form como Required
3️⃣ Asignar nuevo flow en:

```text id="menu2"
Realm Settings → Authentication → Browser Flow
```

---

# 🔥 6️⃣ Forzar configuración inicial

Activar Required Action:

```text id="totp2"
CONFIGURE_TOTP
```

Marcar como Default Action.

Resultado:

Primer login:

```text id="flow2"
Login
   ↓
Configurar TOTP
   ↓
Escanear QR
   ↓
Validar código
   ↓
Acceso concedido
```

---

# ⚙️ 7️⃣ Parámetros configurables

En:

```text id="menu3"
Realm Settings → Security Defenses → OTP Policy
```

Puedes definir:

* Tipo: TOTP
* Algoritmo: HmacSHA1 (estándar)
* Dígitos: 6
* Periodo: 30 segundos
* Ventana de tolerancia

---

# 🧠 8️⃣ ¿Dónde se guarda el secreto?

Se almacena:

* En base de datos de Keycloak
* Asociado al usuario
* De forma segura

El backend no ve el secreto.

---

# 🚨 9️⃣ Seguridad importante

✔ Siempre usar HTTPS
✔ No permitir login sin segundo factor si está activo
✔ Activar brute force protection
✔ Considerar backup codes

---

# 🔎 10️⃣ TOTP condicional (nivel avanzado)

Puedes crear subflow:

```text id="flow3"
Si usuario pertenece a grupo "admins"
   ↓
OTP Required
Si no
   ↓
OTP Alternative
```

Esto permite MFA selectivo.

---

# 🧠 11️⃣ Modelo mental correcto

```text id="mental1"
Password prueba conocimiento
TOTP prueba posesión
MFA = algo que sabes + algo que tienes
```

---

# 🎯 Qué debes retener

* TOTP es segundo factor estándar
* Se integra vía Authentication Flow
* Requiere configurar OTP Form
* Se puede forzar con Required Action
* Es esencial en producción empresarial

