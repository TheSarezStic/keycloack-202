# 📘 Módulo 2 – Realms

## 05 – Modelado organizativo real

---

# 🧠 1️⃣ El problema real

En una empresa real tienes:

* Departamentos
* Cargos
* Aplicaciones
* Permisos técnicos
* Jerarquías

Si modelas mal en **Keycloak**:

* Tokens gigantes
* Roles duplicados
* Caos organizativo
* Difícil auditoría

Aquí vamos a hacerlo bien desde cero.

---

# 🏢 2️⃣ Escenario ejemplo

Empresa: ACME Corp

Departamentos:

* Finanzas
* RRHH
* IT
* Ventas

Aplicaciones:

* facturacion-api
* hr-api
* portal-interno

---

# 🏗 3️⃣ Paso 1 – Definir Client Roles (permisos técnicos)

En `facturacion-api`:

* invoice.read
* invoice.write
* invoice.approve

En `hr-api`:

* employee.read
* employee.update

Estos son permisos técnicos, no organizativos.

---

# 🧩 4️⃣ Paso 2 – Crear Realm Roles organizativos

Ahora creamos roles que representen cargos reales:

* empleado
* supervisor
* manager
* admin

No son técnicos.
Son del negocio.

---

# 🔁 5️⃣ Paso 3 – Convertir en roles compuestos

Ejemplo:

```text id="k3wz9n"
empleado
 └── invoice.read

supervisor
 ├── invoice.read
 └── invoice.approve

admin
 ├── invoice.read
 ├── invoice.write
 ├── invoice.approve
 └── employee.update
```

Así desacoplas negocio de técnica.

---

# 👥 6️⃣ Paso 4 – Crear grupos jerárquicos

```text id="p7r1tx"
ACME
 ├── Finanzas
 ├── RRHH
 ├── IT
 └── Ventas
```

Asignaciones:

* Grupo Finanzas → rol empleado
* Grupo Finanzas-Supervisores → rol supervisor
* Grupo IT → rol admin

Usuarios → se asignan a grupos, no roles directos.

---

# 🔐 7️⃣ Resultado final

Cuando un usuario entra en Finanzas:

```text id="t6b3wc"
Usuario
   ↓
Grupo
   ↓
Realm Role (compuesto)
   ↓
Client Roles
   ↓
Permisos técnicos en token
```

La API valida client roles.

---

# 🚨 8️⃣ Anti-patrones comunes

❌ Crear todos los roles como realm roles
❌ Asignar roles directamente a usuarios
❌ Mezclar nombre técnico con organizativo
❌ No usar grupos

---

# 🧠 9️⃣ Modelo mental limpio

```text id="z9n2qs"
Organización → Grupos
Cargos → Realm Roles
Permisos técnicos → Client Roles
APIs validan → Client Roles
```

---

# 🎯 10️⃣ Diseño escalable

Este modelo permite:

* Añadir nueva app sin romper nada
* Añadir nuevo departamento fácilmente
* Cambiar permisos sin tocar usuarios
* Auditar de forma clara
* Escalar a cientos/miles de usuarios

---

# 📌 Qué debes retener

* Primero diseña organización
* Luego define permisos técnicos
* Usa roles compuestos
* Usa grupos jerárquicos
* Nunca asignes roles masivamente a usuarios
