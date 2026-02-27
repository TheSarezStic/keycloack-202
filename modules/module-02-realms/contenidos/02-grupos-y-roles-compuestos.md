# 📘 Módulo 2 – Realms

## 02 – Grupos y Roles Compuestos

---

# 🧠 1️⃣ El problema real

En una organización real:

* No asignas roles uno a uno manualmente.
* Los usuarios pertenecen a áreas.
* Las áreas tienen permisos.

Aquí entran:

* **Grupos**
* **Roles compuestos**

En **Keycloak** esto es fundamental para escalar.

---

# 👥 2️⃣ Grupos

Un grupo es un contenedor de usuarios.

Ejemplo organizativo:

```text id="a1x9pl"
Empresa
 ├── Finanzas
 ├── RRHH
 ├── IT
 └── Ventas
```

Si asignas un rol al grupo:

→ Todos los usuarios del grupo lo heredan.

---

## 🔎 Ventajas

* Gestión masiva
* Modelo jerárquico
* Permite herencia

---

# 🧩 3️⃣ Roles Compuestos

Un rol compuesto es un rol que contiene otros roles.

Ejemplo:

```text id="h72kxp"
admin
 ├── invoice.read
 ├── invoice.write
 └── user.manage
```

Cuando asignas “admin”:
→ El usuario hereda todos los roles internos.

---

# 🔥 4️⃣ Grupos + Roles Compuestos = Modelo limpio

Ejemplo práctico:

```text id="m9t7rq"
Grupo: Finanzas
   ↓
Realm Role: finance-user
   ↓
Client Roles:
   ├── invoice.read
   └── report.view
```

Usuario entra al grupo:
→ Hereda finance-user
→ Hereda roles técnicos

Sin tocar usuario manualmente.

---

# 🏗 5️⃣ Modelo recomendado

### Paso 1: Crear client roles técnicos

* invoice.read
* invoice.write
* report.view

### Paso 2: Crear realm roles organizativos

* empleado
* supervisor
* admin

### Paso 3: Convertirlos en roles compuestos

### Paso 4: Asignarlos a grupos

---

# 🧠 6️⃣ Diferencia conceptual

| Grupo                | Rol Compuesto          |
| -------------------- | ---------------------- |
| Contiene usuarios    | Contiene roles         |
| Modelo organizativo  | Modelo lógico          |
| Puede ser jerárquico | Puede agrupar permisos |

---

# 🔁 7️⃣ Herencia en grupos

Keycloak permite:

```text id="3d0ltx"
Empresa
 └── IT
     └── DevOps
```

Si asignas rol en IT:
→ DevOps lo hereda.

Esto permite estructuras muy reales.

---

# 🚨 8️⃣ Error típico

Asignar roles directamente a usuarios.

Problema:

* No escala
* Difícil auditoría
* Difícil mantenimiento

Modelo correcto:

```text id="y6p7fs"
Usuario → Grupo → Rol compuesto → Client roles
```

---

# 🎯 9️⃣ Qué debes retener

* Los grupos contienen usuarios
* Los roles compuestos contienen roles
* Los permisos técnicos viven en client roles
* El modelo debe reflejar la organización
* Evita asignaciones directas al usuario
