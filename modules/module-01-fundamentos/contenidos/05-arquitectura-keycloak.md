# 📘 Módulo 1 – Fundamentos

## 05 – Arquitectura de Keycloak

---

# 🧠 1️⃣ ¿Qué es realmente Keycloak?

**Keycloak** es un:

* Identity Provider (IdP)
* Authorization Server (OAuth2)
* Servidor OIDC
* Servidor SAML

Centraliza identidad y control de acceso.

---

# 🏗 2️⃣ Arquitectura lógica

![Image](https://developers.redhat.com/sites/default/files/blog/2019/11/keycloak1.png)

![Image](https://www.keycloak.org/keycloak-benchmark/kubernetes-guide/latest/_images/minikube-runtime-view.dio.svg)

![Image](https://media.licdn.com/dms/image/v2/D4E12AQE8gRCjlAzsXQ/article-cover_image-shrink_720_1280/B4EZVnK6mUG0AM-/0/1741192670056?e=2147483647\&t=dAhmimIa-0QAB7MLupKHqaMNbp8qStW_fK9drKmGUhg\&v=beta)

![Image](https://miro.medium.com/1%2AVyuVPAZoaoe4XM-GtauKjw.png)

---

## 🔹 Componentes principales

### 1️⃣ Realms

Espacios aislados de identidad.

Cada realm tiene:

* Usuarios
* Roles
* Clientes
* Configuración propia

Es como un “tenant”.

---

### 2️⃣ Clientes (Clients)

Aplicaciones que confían en Keycloak.

Tipos:

* Public
* Confidential
* Bearer-only

Ejemplos:

* Angular SPA
* API Node
* Microservicio

---

### 3️⃣ Usuarios

Pueden:

* Tener roles
* Pertenecer a grupos
* Tener atributos personalizados
* Tener MFA configurado

---

### 4️⃣ Roles

Definen permisos.

Tipos:

* Realm roles
* Client roles

---

### 5️⃣ Grupos

Agrupan usuarios y heredan roles.

---

# 🔐 3️⃣ Arquitectura técnica interna

```text id="v2m81d"
Reverse Proxy (opcional)
        ↓
Keycloak (clusterable)
        ↓
Base de datos (PostgreSQL recomendado)
```

---

## 🔹 Internamente usa

* Base de datos relacional
* Infinispan (cache distribuida)
* JPA / Hibernate
* Firmas criptográficas (RSA)

---

# 🗄 4️⃣ Persistencia

Keycloak guarda en base de datos:

* Realms
* Usuarios
* Configuración
* Clientes
* Roles
* Tokens offline
* Eventos

En producción necesita PostgreSQL.

---

# 🔄 5️⃣ Flujo interno simplificado

```text id="xg6l31"
Login → Validación → Generación Token → Firma → Respuesta
```

Cuando genera un JWT:

1. Crea claims
2. Firma con clave privada
3. Expone clave pública vía JWKS

---

# 🏢 6️⃣ Arquitectura en producción

En entorno real:

```text id="p7a3bt"
Load Balancer
   ↓
KC-1   KC-2
   ↓
PostgreSQL HA
```

Permite:

* Alta disponibilidad
* Escalado horizontal
* Failover

---

# 🧠 7️⃣ Conceptos clave que debes entender

* Realm = frontera de seguridad
* Cliente = aplicación
* Usuario ≠ cliente
* Roles pueden estar en realm o cliente
* Keycloak no es solo login UI
* Es un Authorization Server completo

---

# ⚠️ 8️⃣ Error típico

Confundir:

> Realm con cliente

Un realm puede tener múltiples clientes.
Un cliente pertenece a un único realm.

---

# 🎯 9️⃣ Qué debes retener

* Keycloak centraliza identidad
* Es multi-tenant vía realms
* Necesita base de datos en producción
* Es clusterizable
* Implementa OAuth2 + OIDC + SAML
