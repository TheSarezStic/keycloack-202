# 📘 Módulo 2 – Realms

## 01 – Realm Roles vs Client Roles

---

# 🧠 1️⃣ El problema

Cuando modelas permisos en **Keycloak**, debes decidir:

* ¿Este rol es global?
* ¿O es específico de una aplicación?

Aquí aparecen:

* **Realm Roles**
* **Client Roles**

Entender esta diferencia es CLAVE para modelar bien.

---

# 🏢 2️⃣ Realm Roles

👉 Son roles globales dentro del realm.

Afectan a todo el ecosistema.

Ejemplo:

* admin
* auditor
* soporte
* empleado

Si asignas un realm role a un usuario:

* Estará disponible en todos los clientes (según mapeo)

---

## 🔎 Características

* Se definen a nivel de realm
* No pertenecen a ninguna app concreta
* Se incluyen en el token bajo:

```json
"realm_access": {
  "roles": ["admin"]
}
```

---

# 🧩 3️⃣ Client Roles

👉 Son roles específicos de una aplicación.

Ejemplo:

Para la API `facturacion-api`:

* factura.read
* factura.write
* factura.delete

Estos roles:

* Solo existen dentro del cliente
* No tienen sentido fuera de esa app

En el token aparecen bajo:

```json
"resource_access": {
  "facturacion-api": {
    "roles": ["factura.read"]
  }
}
```

---

# 🔥 4️⃣ Diferencia conceptual

| Realm Role        | Client Role            |
| ----------------- | ---------------------- |
| Global            | Específico             |
| Organizativo      | Técnico                |
| Modelo de negocio | Permisos de aplicación |
| Vive en el realm  | Vive en un cliente     |

---

# 🏗 5️⃣ Buen patrón de diseño

### 🟢 Realm roles → modelo organizativo

* admin
* empleado
* supervisor

### 🔵 Client roles → permisos técnicos

* invoice.read
* invoice.create
* user.manage

---

# 🧠 6️⃣ Error típico

Crear todos los roles como realm roles.

Problema:

* Tokens gigantes
* Difícil mantenimiento
* Acoplamiento innecesario

---

# 🎯 7️⃣ Patrón avanzado

Puedes:

* Crear realm role “admin”
* Hacerlo compuesto de varios client roles

Así:

```text
admin
 ├── facturacion.write
 ├── user.manage
 └── report.view
```

Se llaman **roles compuestos**.

---

# 🔎 8️⃣ ¿Qué ve la API?

La API:

* No le importan realm roles organizativos
* Le importan client roles técnicos

Por eso normalmente validas:

```text
resource_access.facturacion-api.roles
```

---

# 🧠 9️⃣ Modelo mental correcto

```text
Realm = Organización
Client = Aplicación
Realm Role = Cargo
Client Role = Permiso técnico
```

---

# 🎯 Qué debes retener

* Realm roles son globales
* Client roles son específicos
* No mezclar responsabilidades
* Modelar primero negocio, luego técnico
* Las APIs validan normalmente client roles
