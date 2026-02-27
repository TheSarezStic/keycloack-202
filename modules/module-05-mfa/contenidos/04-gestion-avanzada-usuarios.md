# 📘 Módulo 5 – MFA

## 04 – Gestión avanzada de usuarios y políticas de seguridad

---

# 🎯 Objetivo

Configurar controles de seguridad empresariales en
**Keycloak** para:

* Proteger contra fuerza bruta
* Endurecer contraseñas
* Limitar sesiones
* Mejorar postura de seguridad

Aquí ya hablamos de entorno productivo.

---

# 🧠 1️⃣ Protección contra fuerza bruta

Keycloak incluye protección integrada.

Activar en:

```text
Realm Settings → Security Defenses → Brute Force Detection
```

---

## 🔐 Opciones importantes

* Permanent Lockout
* Max Login Failures
* Wait Increment
* Quick Login Check

Ejemplo recomendado:

| Parámetro          | Valor ejemplo |
| ------------------ | ------------- |
| Max Failures       | 5             |
| Wait Increment     | 60s           |
| Failure Reset Time | 12h           |

---

# 🔄 2️⃣ Qué ocurre internamente

```text
Intento fallido
   ↓
Contador incrementa
   ↓
Supera límite
   ↓
Cuenta bloqueada temporalmente
```

Protege contra ataques automatizados.

---

# 🔑 3️⃣ Política de contraseñas

Configurar en:

```text
Realm Settings → Authentication → Password Policy
```

Opciones típicas:

* Minimum length
* Digits
* Uppercase
* Special characters
* Not recently used
* Expiration

Ejemplo fuerte:

```text
length(12) and digits(1) and upperCase(1) and specialChars(1)
```

---

# 🔥 4️⃣ Expiración de contraseña

Puedes definir:

* Password expiration (ej: 90 días)
* Not recently used (ej: 5 últimas)

Cuando expira:

→ Se activa Required Action UPDATE_PASSWORD.

---

# 🧠 5️⃣ Gestión de sesiones

Configurar en:

```text
Realm Settings → Sessions
```

Parámetros clave:

* SSO Session Idle
* SSO Session Max
* Client Session Idle
* Access Token Lifespan

Ejemplo profesional:

| Parámetro     | Valor  |
| ------------- | ------ |
| Access Token  | 2 min  |
| Refresh Token | 30 min |
| SSO Idle      | 30 min |
| SSO Max       | 8 h    |

---

# 🔐 6️⃣ Cierre de sesiones activas

Puedes ver sesiones activas en:

```text
Users → {user} → Sessions
```

Permite:

* Invalidar sesión manualmente
* Forzar logout remoto

Muy útil en incidentes.

---

# 🚨 7️⃣ Protección adicional

En producción también deberías:

✔ Activar HTTPS obligatorio
✔ Configurar Proxy correctamente
✔ Activar headers seguros (X-Forwarded-*)
✔ Limitar Web Origins
✔ Configurar CORS restrictivo

---

# 🧠 8️⃣ Protección contra token replay

Buenas prácticas:

* Access tokens cortos
* Validar audience
* Validar issuer
* No usar tokens largos
* No exponer tokens en logs

---

# 🔎 9️⃣ Eventos y auditoría

Activar en:

```text
Realm Settings → Events
```

Puedes loguear:

* Login
* Logout
* Errores
* Cambios de usuario
* Actualizaciones de roles

Clave para auditoría empresarial.

---

# 🧠 10️⃣ Modelo mental de seguridad por capas

```text
Flow → Autenticación
MFA → Segundo factor
Password Policy → Fortaleza
Brute Force → Protección ataques
Sessions → Control temporal
JWT → Seguridad API
```

Defensa en profundidad.

---

# 🎯 Qué debes retener

* Activar brute force detection
* Configurar password policy fuerte
* Ajustar expiraciones correctamente
* Monitorear sesiones activas
* Activar auditoría

