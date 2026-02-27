# 📘 Módulo 6 – Federation

## 01 – User Federation

---

# 🎯 Objetivo

Entender cómo conectar
**Keycloak**
con sistemas externos de identidad sin migrar usuarios.

Aquí hablamos de:

* LDAP
* Active Directory
* Bases de datos externas
* User Storage SPI

---

# 🧠 1️⃣ El problema real

En una empresa grande ya existen:

* Active Directory
* LDAP corporativo
* Sistemas legacy con usuarios

No puedes:

❌ Crear usuarios manualmente en Keycloak
❌ Migrar todo sin plan
❌ Romper identidad corporativa

Necesitas federación.

---

# 🏗 2️⃣ ¿Qué es User Federation?

User Federation significa:

> Keycloak no almacena el usuario.
> Lo consulta en un sistema externo.

Pero:

* Gestiona autenticación
* Emite tokens
* Aplica roles
* Controla MFA

---

# 🔄 3️⃣ Modelo conceptual

```text id="fed1"
Usuario
   ↓
Keycloak
   ↓
LDAP / AD
   ↓
Validación credenciales
   ↓
Keycloak emite JWT
```

---

# 🔐 4️⃣ ¿Dónde se almacenan los datos?

Depende del modo:

| Modo         | Qué ocurre               |
| ------------ | ------------------------ |
| Import Users | Copia usuario localmente |
| Read-only    | Solo consulta LDAP       |
| Unsynced     | Mezcla atributos         |

---

# 🔎 5️⃣ Ventajas de federación

✔ Mantienes AD corporativo
✔ No duplicas credenciales
✔ Permite MFA encima de LDAP
✔ Permite roles locales adicionales
✔ No rompes sistemas existentes

---

# 🧠 6️⃣ ¿Quién autentica realmente?

Si usas LDAP:

```text id="auth1"
Password → Validada por LDAP
JWT → Emitido por Keycloak
```

Keycloak delega autenticación pero mantiene control del token.

---

# 🧩 7️⃣ Sincronización de usuarios

Keycloak permite:

* Sync manual
* Sync periódica
* Sync incremental

Esto permite:

* Mapear grupos LDAP
* Mapear atributos
* Mapear roles

---

# ⚙️ 8️⃣ Componentes clave

En:

```text
User Federation → Add provider
```

Opciones comunes:

* ldap
* Kerberos
* Custom user storage (SPI)

---

# 🔥 9️⃣ Diferencia con Identity Brokering

| User Federation           | Identity Brokering          |
| ------------------------- | --------------------------- |
| Consulta directorio       | Delegar login completo      |
| Password validado en LDAP | Redirige a otro IdP         |
| Usuario existe en LDAP    | Usuario vive en IdP externo |

Federation = backend directory
Brokering = login externo

---

# 🧠 10️⃣ Modelo mental correcto

```text id="mental1"
Keycloak = Orquestador
LDAP = Fuente de verdad
JWT = Producto final
```

Keycloak siempre emite el token.

---

# 🚨 11️⃣ Errores comunes

❌ No probar conexión LDAP correctamente
❌ No mapear grupos
❌ No definir base DN correctamente
❌ No usar LDAPS en producción
❌ No configurar sync periódica

---

# 🎯 Qué debes retener

* Federation conecta con directorios externos
* LDAP sigue siendo fuente de verdad
* Keycloak sigue siendo IdP
* Se pueden combinar roles locales + LDAP
* Es esencial en entornos corporativos
