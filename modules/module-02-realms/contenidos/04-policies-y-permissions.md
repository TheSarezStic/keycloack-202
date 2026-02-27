# 📘 Módulo 2 – Realms

## 04 – Policies y Permissions

---

# 🧠 1️⃣ El problema

Hasta ahora hemos visto:

* Usuarios
* Roles
* Grupos
* Client roles

Pero a veces necesitas algo más fino que:

> “Tiene rol admin → puede todo”

Necesitas reglas como:

* Solo puede ver facturas propias
* Solo puede aprobar si es supervisor
* Solo puede acceder en horario laboral

Aquí entra la **Authorization Services** de
**Keycloak**

---

# 🏗 2️⃣ Modelo de autorización avanzada

Keycloak permite definir:

* Resources
* Scopes
* Policies
* Permissions

Es un modelo tipo **RBAC + ABAC**.

---

# 🔹 3️⃣ Resources

Representan cosas protegidas.

Ejemplos:

* `/invoices`
* `/users`
* `/reports`
* `documento-123`

---

# 🔹 4️⃣ Scopes

Definen acciones sobre recursos.

Ejemplos:

* read
* write
* delete
* approve

---

# 🔹 5️⃣ Policies

Definen reglas que deben cumplirse.

Tipos comunes:

* Role-based policy
* User-based policy
* Group-based policy
* Time-based policy
* JS-based policy

Ejemplo conceptual:

```text id="p8k3ms"
Policy: solo-supervisores
Condición: tiene rol supervisor
```

---

# 🔹 6️⃣ Permissions

Conectan:

Resource + Scope + Policy

Ejemplo:

```text id="z4t8ql"
Recurso: /invoices
Scope: approve
Policy: solo-supervisores
```

Resultado:
Solo supervisores pueden aprobar facturas.

---

# 🔥 7️⃣ Flujo conceptual

```text id="x1cz9p"
API recibe request
   ↓
Envía token a Keycloak (o valida localmente)
   ↓
Keycloak evalúa políticas
   ↓
Permitir o Denegar
```

---

# 🧠 8️⃣ RBAC vs ABAC

| Modelo   | Basado en            |
| -------- | -------------------- |
| RBAC     | Roles                |
| ABAC     | Atributos            |
| Keycloak | Puede combinar ambos |

Ejemplo ABAC:

Permitir acceso si:

```text id="n3tz8u"
usuario.department == recurso.department
```

---

# 🚨 9️⃣ Error típico

Pensar que Keycloak solo sirve para login.

No.

También puede:

* Evaluar permisos dinámicos
* Actuar como PDP (Policy Decision Point)
* Emitir tokens con autorización embebida

---

# 🎯 10️⃣ ¿Cuándo usar Authorization Services?

Usarlo cuando:

* Necesitas autorización centralizada
* Quieres evitar lógica compleja en backend
* Quieres delegar evaluación a IAM

No usarlo si:

* Solo necesitas validar roles simples

---

# 🧠 Modelo mental

```text id="q7w2le"
Autenticación = ¿Quién eres?
Autorización simple = ¿Qué rol tienes?
Autorización avanzada = ¿Cumples la política?
```

---

# 🎯 Qué debes retener

* Resources representan cosas protegidas
* Scopes representan acciones
* Policies definen reglas
* Permissions unen todo
* Keycloak puede ser motor de autorización avanzado
