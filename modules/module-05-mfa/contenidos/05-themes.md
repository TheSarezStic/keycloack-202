# 📘 Módulo 5 – MFA

## 05 – Themes (Personalización del Login)

---

# 🎯 Objetivo

Aprender a personalizar la interfaz de login en
**Keycloak** sin romper:

* Seguridad
* Flujos de autenticación
* MFA
* Required Actions

Aquí hablamos de branding corporativo real.

---

# 🧠 1️⃣ ¿Qué es un Theme?

Un theme es un conjunto de:

* Plantillas HTML (Freemarker)
* CSS
* Imágenes
* Recursos estáticos

Que controlan:

* Login
* Registro
* Error pages
* Account console
* Email templates

---

# 🏗 2️⃣ Tipos de themes

Keycloak soporta themes para:

* login
* account
* admin
* email
* welcome

El más común: **login**

---

# 🔎 3️⃣ Estructura básica de un theme

Dentro del contenedor:

```text
/opt/keycloak/themes/
   └── mytheme/
       └── login/
           ├── theme.properties
           ├── login.ftl
           ├── styles.css
           └── resources/
```

---

# 🔧 4️⃣ Crear un theme mínimo

### 📄 theme.properties

```properties
parent=keycloak
styles=css/styles.css
```

Esto hereda el theme base.

---

# 🎨 5️⃣ Personalizar CSS

Ejemplo simple:

```css
body {
  background-color: #0d1b2a;
}

.pf-c-button.pf-m-primary {
  background-color: #1b263b;
}
```

Sin tocar lógica.

---

# 🔄 6️⃣ Activar theme en el realm

Ir a:

```text
Realm Settings → Themes
```

Seleccionar:

```text
Login Theme → mytheme
```

Refrescar navegador.

---

# ⚠️ 7️⃣ Nunca modificar el theme base

❌ No tocar directamente:

```text
keycloak theme
```

Siempre:

✔ Crear theme propio
✔ Heredar del padre
✔ Versionarlo en Git

---

# 🔐 8️⃣ Personalización con MFA

Importante:

Si activas TOTP o Required Actions:

* login.ftl debe soportarlos
* No eliminar bloques dinámicos
* No romper variables Freemarker

Ejemplo:

```html
${kcSanitize(msg.summary)?no_esc}
```

Si lo borras → errores.

---

# 🧠 9️⃣ Customización avanzada

Puedes modificar:

* Logo
* Texto legal
* Política de privacidad
* Campos adicionales
* Mensajes de error
* Layout completo

Pero sin romper:

* Form actions
* Hidden inputs
* Tokens CSRF

---

# 🔥 10️⃣ Email themes

Puedes personalizar:

* Reset password email
* Verify email
* OTP configuration mail

Ruta:

```text
themes/mytheme/email/
```

Muy importante para branding empresarial.

---

# 🚨 11️⃣ Seguridad importante

Nunca:

❌ Añadir scripts externos inseguros
❌ Exponer datos sensibles
❌ Romper CSRF tokens
❌ Eliminar campos ocultos del form

---

# 🧠 12️⃣ Modelo mental correcto

```text
Theme → Presentación
Flow → Lógica
Security → Intocable
```

Separar diseño de seguridad.

---

# 🎯 Qué debes retener

* Crear theme propio
* Heredar del base
* No romper Freemarker
* Compatible con MFA
* Versionar cambios

