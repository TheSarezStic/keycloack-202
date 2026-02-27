# 📘 Módulo 6 – Federation

## 02 – Integración LDAP (Paso a paso)

---

# 🎯 Objetivo

Conectar **Keycloak** con un servidor LDAP corporativo para:

* Autenticar usuarios contra LDAP
* Sincronizar usuarios
* Mapear grupos
* Mantener JWT emitidos por Keycloak

---

# 🧠 1️⃣ Arquitectura final

![Image](https://thalesdocs.com/sas/3.18/images/Keycloak/Load.png)

![Image](https://techcommunity.microsoft.com/t5/s/gxcuf89792/images/bS00MTc0MjM4LTU5NDE4OGlBREJENjdFODBENjU5MTc5?image-dimensions=999x999\&revision=6)

![Image](https://thalesdocs.com/sas/3.22/images/keycloak/Load.png)

![Image](https://thalesdocs.com/sas/3.22/images/keycloak/SAS%20UserU.png)

Flujo:

```text id="arch1"
Usuario
   ↓
Keycloak
   ↓
LDAP / Active Directory
   ↓
Validación password
   ↓
Keycloak emite JWT
```

LDAP valida credenciales.
Keycloak controla tokens y roles.

---

# 🛠 2️⃣ Requisitos previos

Necesitas:

* Host LDAP (ej: ldap.corp.local)
* Puerto (389 o 636 para LDAPS)
* Bind DN (usuario técnico)
* Password bind
* Base DN (ej: dc=corp,dc=local)

---

# 🔧 3️⃣ Configuración en Keycloak

Ir a:

```text
User Federation → Add provider → ldap
```

---

## 🔹 Parámetros básicos

| Campo           | Ejemplo                    |
| --------------- | -------------------------- |
| Vendor          | Active Directory           |
| Connection URL  | ldap://ldap.corp.local:389 |
| Bind DN         | cn=admin,dc=corp,dc=local  |
| Bind Credential | ********                   |
| Users DN        | ou=users,dc=corp,dc=local  |

---

# 🔎 4️⃣ Probar conexión

Botón:

```text
Test connection
Test authentication
```

Ambos deben pasar antes de continuar.

---

# 🔁 5️⃣ Configuración importante

### 🔹 Edit Mode

| Modo      | Significado             |
| --------- | ----------------------- |
| READ_ONLY | LDAP es fuente única    |
| WRITABLE  | Keycloak puede escribir |
| UNSYNCED  | Mezcla híbrida          |

En empresa → normalmente READ_ONLY.

---

# 🔐 6️⃣ Import Users

Opción:

```text
Import Users → ON
```

Significa:

* Usuario se copia a DB local
* Permite asignar roles locales
* Mejora rendimiento

Recomendado en producción.

---

# 👥 7️⃣ Mapear grupos LDAP

En:

```text
Mappers → Add mapper → group-ldap-mapper
```

Permite:

* Sincronizar grupos LDAP
* Convertirlos en grupos Keycloak
* Asignar roles automáticamente

Muy potente.

---

# 🔄 8️⃣ Sincronización manual

En el provider LDAP:

```text
Synchronize all users
```

O:

```text
Synchronize changed users
```

Permite importar usuarios masivamente.

---

# 🧠 9️⃣ Qué ocurre en login

```text
Usuario introduce password
   ↓
Keycloak envía bind a LDAP
   ↓
LDAP valida
   ↓
Keycloak crea sesión
   ↓
JWT emitido
```

Password nunca se almacena en Keycloak.

---

# 🔒 10️⃣ Seguridad en producción

✔ Usar LDAPS (636)
✔ Validar certificado LDAP
✔ Usar cuenta bind con permisos mínimos
✔ Restringir acceso por red
✔ Activar brute force en Keycloak

---

# 🚨 11️⃣ Errores comunes

❌ Base DN incorrecta
❌ Filtro LDAP mal configurado
❌ No mapear atributos obligatorios
❌ No usar LDAPS
❌ No configurar sync periódica

---

# 🎯 Qué debes retener

* LDAP sigue siendo fuente de credenciales
* Keycloak sigue siendo emisor de tokens
* Se pueden mapear grupos LDAP
* Import Users mejora gestión
* Seguridad depende de LDAPS
