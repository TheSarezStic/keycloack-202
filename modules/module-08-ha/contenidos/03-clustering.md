# 📘 Módulo 8 – HA & Producción

## 03 – Clustering y múltiples instancias (HA real)

---

# 🎯 Objetivo

Escalar **Keycloak** horizontalmente para:

* Alta disponibilidad
* Tolerancia a fallos
* Balanceo de carga
* Escalado bajo demanda

Aquí dejamos de pensar en “un servidor” y pasamos a arquitectura distribuida.

---

# 🧠 1️⃣ Principio fundamental

```text id="zvntn7"
Keycloak puede escalar horizontalmente
SI comparte base de datos
```

La base PostgreSQL es el punto común.

Sin DB compartida → no hay cluster.

---

# 🏗 2️⃣ Arquitectura HA básica

![Image](https://blog.elest.io/content/images/2024/06/cluster_architecture.png)

![Image](https://developers.redhat.com/sites/default/files/fkeycloak_1.png)

![Image](https://is.docs.wso2.com/en/5.9.0/assets/img/setup/component-diagram.png)

![Image](https://is.docs.wso2.com/en/5.9.0/assets/img/administer/load%20balancing/load-balancing.png)

Modelo típico:

```text id="7k5f7m"
Internet
   ↓
Load Balancer
   ↓
Keycloak Node 1
Keycloak Node 2
Keycloak Node 3
   ↓
PostgreSQL
```

---

# 🔐 3️⃣ Qué debe compartirse

✔ Base de datos
✔ Configuración
✔ Realm data
✔ Claves

No se comparten:

* Sesiones en memoria (se sincronizan vía cache)

---

# 🔄 4️⃣ Cache distribuida (Infinispan)

Keycloak usa **Infinispan** para:

* Sesiones
* Tokens
* Login state

En cluster:

```text id="y2eqbt"
Nodo 1
   ↔
Nodo 2
   ↔
Nodo 3
```

La cache se replica entre nodos.

---

# ⚙️ 5️⃣ Requisitos mínimos para cluster

1️⃣ Base PostgreSQL externa
2️⃣ Hostname estable
3️⃣ Reverse proxy o Load Balancer
4️⃣ Health checks activos
5️⃣ Red interna privada

---

# 🔥 6️⃣ Sticky sessions (¿necesarias?)

Depende del diseño.

### Con cache distribuida correcta:

→ No obligatorias.

### Sin configuración adecuada:

→ Sticky sessions recomendadas.

En producción profesional:
→ Mejor configurar cluster correctamente y evitar dependencia de stickiness.

---

# 🧩 7️⃣ Ejemplo conceptual en Docker (multi instancia)

```yaml
services:

  keycloak1:
    image: quay.io/keycloak/keycloak:24.0
    command: start

  keycloak2:
    image: quay.io/keycloak/keycloak:24.0
    command: start
```

Ambos apuntando a la misma PostgreSQL.

Balanceo externo obligatorio.

---

# 🚨 8️⃣ Problemas típicos en cluster

❌ No configurar correctamente cache
❌ Hostname mal definido
❌ No usar health checks
❌ Exponer nodos directamente
❌ No sincronizar claves

---

# 🧠 9️⃣ Flujo real en HA

```text id="2y5mwj"
Usuario login
   ↓
Load Balancer → Node 1
   ↓
Sesión creada
   ↓
Cache replicada
   ↓
Siguiente request → Node 2
   ↓
Sesión válida
```

Sin pérdida de sesión.

---

# 🔒 10️⃣ Alta disponibilidad real implica

✔ Múltiples nodos
✔ DB con alta disponibilidad (ej: RDS multi-AZ)
✔ Backup automatizado
✔ Monitoreo
✔ Rolling updates

---

# 🏢 11️⃣ En Kubernetes

Cluster típico:

```text id="3d7c8u"
Ingress
   ↓
Service
   ↓
Pods Keycloak (replicas=3)
   ↓
PostgreSQL externo
```

Aquí ya hablamos de producción cloud-native.

---

# 🧠 12️⃣ Modelo mental final

```text id="mental1"
Un nodo = riesgo
Dos nodos = disponibilidad básica
Tres nodos = resiliencia real
```

HA no es opcional en IAM crítico.

---

# 🎯 Qué debes retener

* Cluster requiere DB compartida
* Cache distribuida es clave
* Load balancer obligatorio
* Health checks necesarios
* HA real implica múltiples nodos

