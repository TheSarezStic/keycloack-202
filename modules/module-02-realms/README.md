# 📘 Módulo 02 – Realms, Roles y Modelo Organizativo

---

## 🎯 Objetivo del módulo

Aprender a modelar correctamente:

* Realms
* Roles
* Grupos
* Service Accounts
* Authorization Policies

Dentro de **Keycloak**.

Este módulo es donde dejamos de “hacer login” y empezamos a diseñar identidad empresarial real.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Entender qué es un Realm y cuándo usar múltiples
* Diseñar roles correctamente (realm vs client roles)
* Usar roles compuestos
* Modelar grupos organizativos
* Configurar Service Accounts (machine-to-machine)
* Aplicar Authorization Services
* Diseñar un modelo limpio y escalable

---

## 📚 Contenidos del módulo

| Archivo                         | Tema                            |
| ------------------------------- | ------------------------------- |
| 01-realm-vs-client-roles.md     | Diferencia entre tipos de roles |
| 02-grupos-y-roles-compuestos.md | Modelado organizativo           |
| 03-service-accounts.md          | Autenticación máquina a máquina |
| 04-policies-y-permissions.md    | Authorization Services          |
| 05-modelado-organizativo.md     | Diseño empresarial correcto     |

---

# 🏗 Conceptos clave

---

## 🌍 Realm

Un Realm es:

> Un espacio aislado de identidad.

Cada realm tiene:

* Usuarios propios
* Roles propios
* Clientes propios
* Configuración propia

Modelo típico:

```text
Empresa A → Realm A
Empresa B → Realm B
```

Nunca mezclar organizaciones en un mismo realm si no deben compartir identidad.

---

## 🔑 Realm Roles vs Client Roles

| Tipo        | Uso                                          |
| ----------- | -------------------------------------------- |
| Realm Role  | Permiso organizativo general                 |
| Client Role | Permiso técnico específico de una aplicación |

Ejemplo correcto:

```text
Realm Role → empleado
Client Role → invoice.read
```

Separación clara entre negocio y técnica.

---

## 👥 Grupos

Los grupos:

* Contienen usuarios
* Pueden ser jerárquicos
* Pueden heredar roles

Modelo recomendado:

```text
Usuario → Grupo → Rol compuesto → Permisos técnicos
```

Nunca asignar roles directamente al usuario en producción.

---

## 🧩 Roles Compuestos

Permiten agrupar permisos.

Ejemplo:

```text
admin
  ├── user.manage
  ├── invoice.read
  └── invoice.write
```

Escalable y mantenible.

---

## 🤖 Service Accounts

Permiten:

* Autenticación máquina a máquina
* Uso del flujo Client Credentials
* Integración backend ↔ backend

No hay usuario humano.

Muy común en microservicios.

---

## 🛡 Authorization Services

Permiten:

* Políticas basadas en roles
* Políticas basadas en atributos
* Control de acceso a recursos

Modelo:

```text
Usuario → Policy → Permission → Resource
```

Es RBAC + ABAC combinados.

---

# 🧪 Laboratorios incluidos

---

## 🔹 Lab 01 – Modelado de Roles

* Crear realm roles
* Crear client roles
* Crear roles compuestos

---

## 🔹 Lab 02 – Grupos y Asignaciones

* Crear grupos
* Asignar roles a grupos
* Validar herencia

---

## 🔹 Lab 03 – Client Credentials

* Crear cliente confidencial
* Activar Service Account
* Obtener token máquina a máquina

---

## 🔹 Lab 04 – Multi-Realm

* Crear segundo realm
* Configurar cliente independiente
* Validar aislamiento

---

# 🧠 Modelo mental que debes adquirir

```text
Identidad organizativa
   ↓
Roles de negocio
   ↓
Permisos técnicos
   ↓
Aplicaciones
```

Separar:

* Organización
* Permisos
* Tecnología

---

# ⚠ Errores comunes

❌ Asignar roles directamente a usuarios
❌ Mezclar roles técnicos con organizativos
❌ No usar roles compuestos
❌ No separar realms correctamente
❌ Usar un único cliente para todo

---

# 🚀 Resultado esperado

Al terminar este módulo deberías poder:

✔ Diseñar modelo RBAC limpio
✔ Explicar diferencia realm/client roles
✔ Implementar client credentials
✔ Modelar jerarquías organizativas
✔ Separar identidad entre entornos
