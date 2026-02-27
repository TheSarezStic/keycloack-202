# 📘 Módulo 05 – MFA, Flujos y Extensibilidad

---

## 🎯 Objetivo del módulo

Aprender a reforzar la seguridad en
**Keycloak**
mediante:

* Authentication Flows
* Required Actions
* MFA con TOTP
* Políticas avanzadas de usuario
* Themes personalizados
* Extensiones mediante SPI

Aquí pasamos de “funciona” a “seguro”.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Entender cómo funcionan los Authentication Flows
* Forzar acciones obligatorias (Required Actions)
* Implementar MFA con TOTP
* Configurar políticas de seguridad
* Personalizar la UI de login
* Comprender cómo extender Keycloak con SPI

Este módulo transforma una instalación básica en una solución empresarial.

---

## 📚 Contenidos del módulo

| Archivo                         | Tema                              |
| ------------------------------- | --------------------------------- |
| 01-authentication-flows.md      | Cómo funciona el proceso de login |
| 02-required-actions.md          | Acciones obligatorias post-login  |
| 03-mfa-totp.md                  | Implementación de segundo factor  |
| 04-gestion-avanzada-usuarios.md | Hardening y políticas             |
| 05-themes.md                    | Personalización segura del login  |
| 06-introduccion-spi.md          | Extensiones avanzadas             |

---

# 🔐 Authentication Flows

Un Flow define:

> Cómo se autentica un usuario.

Ejemplo básico:

```text id="flow1"
Username
   ↓
Password
   ↓
OTP (si aplica)
   ↓
Login exitoso
```

Keycloak permite:

* Duplicar flows
* Modificar pasos
* Crear subflows condicionales

Nunca modificar el flow base directamente en producción.

---

# 🔁 Required Actions

Son acciones obligatorias tras login:

* UPDATE_PASSWORD
* VERIFY_EMAIL
* CONFIGURE_TOTP
* UPDATE_PROFILE

Modelo:

```text id="flow2"
Login correcto
   ↓
Required Action pendiente
   ↓
Acceso concedido
```

Muy útil en onboarding seguro.

---

# 🔥 MFA con TOTP

Implementación estándar:

* App autenticadora
* Código de 6 dígitos
* Validez ~30 segundos

Modelo:

```text id="flow3"
Password
   +
Código TOTP
   =
Autenticación fuerte
```

MFA es obligatorio en entornos críticos.

---

# 🛡 Gestión avanzada de usuarios

Configuraciones clave:

✔ Brute Force Detection
✔ Password Policy fuerte
✔ Expiración de contraseña
✔ Gestión de sesiones
✔ Revocación manual

IAM sin hardening es vulnerable.

---

# 🎨 Themes

Permiten:

* Personalizar login
* Cambiar branding
* Adaptar mensajes

Importante:

✔ Heredar del theme base
✔ No romper plantillas Freemarker
✔ No eliminar campos ocultos

Separar presentación de seguridad.

---

# 🧩 SPI (Service Provider Interface)

Permite:

* Autenticadores personalizados
* Required Actions custom
* Event listeners
* Integraciones corporativas

Se implementa en Java y se despliega como provider.

Uso recomendado solo en casos avanzados.

---

# 🧪 Laboratorios incluidos

---

## 🔹 Lab 01 – Custom Flow

* Duplicar flow
* Modificar ejecuciones
* Validar flujo

---

## 🔹 Lab 02 – MFA obligatorio

* Configurar TOTP
* Forzar Required Action
* Validar doble factor

---

## 🔹 Lab 03 – Theme personalizado

* Crear theme propio
* Modificar CSS
* Validar cambios

---

# 🧠 Modelo mental correcto

```text id="mental1"
Autenticación = quién eres
MFA = prueba adicional
Políticas = reglas del sistema
```

Seguridad por capas.

---

# ⚠ Errores comunes

❌ No activar brute force detection
❌ No forzar cambio de password inicial
❌ Permitir login sin TOTP en perfiles críticos
❌ Modificar flow base directamente
❌ Romper templates del theme

---

# 🔒 Seguridad recomendada

✔ PKCE obligatorio
✔ MFA en roles administrativos
✔ Password policy fuerte
✔ Logs de eventos activados
✔ Protección contra fuerza bruta

---

# 🚀 Resultado esperado

Al terminar este módulo deberías poder:

✔ Implementar MFA correctamente
✔ Diseñar flujos personalizados
✔ Forzar acciones obligatorias
✔ Endurecer la configuración
✔ Extender Keycloak si es necesario
