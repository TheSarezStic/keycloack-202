# 📘 Módulo 6 – Federation

## 03 – Sincronización y mapeo avanzado

---

# 🎯 Objetivo

Aprender a:

* Mapear atributos LDAP → Keycloak
* Sincronizar grupos correctamente
* Controlar cómo se importan usuarios
* Diseñar un modelo limpio y escalable

Siempre usando
**Keycloak**
como orquestador de identidad.

---

# 🧠 1️⃣ El problema real

LDAP tiene atributos como:

* cn
* sn
* mail
* department
* employeeNumber
* memberOf

Pero tu aplicación necesita:

* firstName
* lastName
* email
* roles
* atributos personalizados

Aquí entran los **mappers**.

---

# 🧩 2️⃣ Qué es un Mapper

Un Mapper es:

> Una regla que transforma datos externos en atributos internos.

Ruta:

```text
User Federation → LDAP → Mappers
```

---

# 🔎 3️⃣ Mapper típico de atributos

Ejemplo:

| LDAP      | Keycloak  |
| --------- | --------- |
| mail      | email     |
| givenName | firstName |
| sn        | lastName  |

Tipo:

```text
User Attribute Mapper
```

---

# 🔐 4️⃣ Mapear atributos personalizados

Ejemplo LDAP:

```text
department=finance
```

Puedes mapearlo a:

```text
User Attribute → department
```

Luego usarlo para:

* Authorization
* Policies
* Conditional MFA

---

# 👥 5️⃣ Mapper de grupos LDAP

Tipo:

```text
group-ldap-mapper
```

Permite:

* Importar grupos LDAP
* Convertirlos en grupos Keycloak
* Heredar roles automáticamente

Modelo típico:

```text id="groups1"
LDAP Group: CN=Finance
   ↓
Keycloak Group: Finance
   ↓
Realm Role: finance-user
```

---

# 🔄 6️⃣ Sincronización

Hay tres modos:

| Tipo               | Qué hace       |
| ------------------ | -------------- |
| Full Sync          | Reimporta todo |
| Changed Users Sync | Solo cambios   |
| Periodic Sync      | Automático     |

En producción → usar sync periódica.

---

# 🧠 7️⃣ Estrategia recomendada empresarial

Modelo limpio:

```text id="model1"
LDAP
   ↓ (usuarios + grupos)
Keycloak
   ↓ (roles compuestos)
Aplicaciones
```

LDAP controla estructura organizativa.
Keycloak controla permisos técnicos.

---

# 🔥 8️⃣ Atributos para autorización avanzada

Si mapeas:

```text
department
employeeType
costCenter
```

Puedes crear políticas tipo:

```text id="policy1"
Permitir acceso si user.department == resource.department
```

Esto es ABAC (Attribute-Based Access Control).

---

# ⚠️ 9️⃣ Errores comunes

❌ No mapear email correctamente
❌ No importar grupos
❌ No activar Import Users
❌ No probar sync incremental
❌ No revisar conflictos de username

---

# 🔒 10️⃣ Seguridad importante

✔ Usar LDAPS
✔ Filtrar base DN correctamente
✔ No permitir escritura innecesaria
✔ Limitar atributos importados
✔ Revisar duplicados

---

# 🧠 11️⃣ Modelo mental correcto

```text id="mental1"
LDAP = Identidad organizativa
Keycloak = Identidad técnica
JWT = Identidad operativa
```

Separar claramente responsabilidades.

---

# 🎯 Qué debes retener

* Mappers transforman atributos
* Grupos LDAP pueden convertirse en roles
* Sync debe planificarse
* Atributos permiten ABAC
* Diseño correcto evita caos futuro

