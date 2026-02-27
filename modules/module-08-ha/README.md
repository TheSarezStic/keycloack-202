# 📘 Módulo 08 – Alta Disponibilidad y Producción

---

## 🎯 Objetivo del módulo

Diseñar y desplegar
**Keycloak**
en un entorno de producción real con:

* Base de datos externa (PostgreSQL)
* Configuración segura
* Clustering
* Backup y recuperación
* Health checks
* Integración con API Gateway

Este módulo transforma todo lo aprendido en una arquitectura empresarial robusta.

---

## 🧠 Qué aprenderás

Al finalizar este módulo podrás:

* Migrar de entorno dev a PostgreSQL
* Configurar variables de producción
* Escalar múltiples instancias
* Entender el clustering y caché distribuida
* Diseñar estrategia de backup y DR
* Configurar health checks correctamente
* Integrar con API Gateway en arquitectura moderna

Este es el módulo donde se consolida el nivel profesional.

---

## 📚 Contenidos del módulo

| Archivo                        | Tema                           |
| ------------------------------ | ------------------------------ |
| 01-migracion-postgresql.md     | Uso de base de datos externa   |
| 02-configuracion-produccion.md | Variables y buenas prácticas   |
| 03-clustering.md               | Alta disponibilidad            |
| 04-export-import-realms.md     | Migración entre entornos       |
| 05-backup-recuperacion.md      | Estrategia de DR               |
| 06-health-checks.md            | Liveness, readiness y métricas |
| 07-api-gateway-conceptual.md   | Arquitectura enterprise        |

---

# 🏗 Arquitectura objetivo

```text
Usuario
   ↓
Reverse Proxy / API Gateway
   ↓
Keycloak (2+ instancias)
   ↓
PostgreSQL
```

Componentes clave:

✔ Base de datos compartida
✔ Múltiples nodos
✔ Health checks activos
✔ Monitorización
✔ Backup probado

---

# 🗄 Migración a PostgreSQL

En producción:

* Nunca usar H2
* Usar PostgreSQL externa
* Activar migraciones automáticas
* Asegurar conexión segura

La base de datos es el corazón del sistema.

---

# ⚙️ Configuración de producción

Variables esenciales:

* `KC_DB`
* `KC_DB_URL`
* `KC_DB_USERNAME`
* `KC_DB_PASSWORD`
* `KC_HOSTNAME`
* `KC_HEALTH_ENABLED`
* `KC_METRICS_ENABLED`

Errores en hostname o proxy config pueden romper OIDC.

---

# 🔁 Clustering

Alta disponibilidad implica:

* 2 o más instancias
* Balanceador de carga
* Caché compartida (Infinispan)
* Sesiones replicadas

Modelo:

```text
LB → Nodo 1
   → Nodo 2
```

Si un nodo cae, el otro sigue operando.

---

# 💾 Backup y Recuperación

Estrategia mínima:

✔ Backup periódico PostgreSQL
✔ Export lógico de realms
✔ Restauración probada
✔ Definición de RPO y RTO

Regla fundamental:

> Backup no probado = backup inexistente.

---

# ❤️ Health Checks y Observabilidad

Endpoints clave:

* `/health/live`
* `/health/ready`
* `/metrics`

Integración con:

* Load Balancer
* Kubernetes
* Prometheus

El LB debe consultar `/health/ready`, no `/`.

---

# 🚪 Integración con API Gateway

Arquitectura moderna:

```text
Cliente
   ↓
API Gateway
   ↓
Keycloak
   ↓
Microservicios
```

El Gateway puede:

* Validar JWT
* Aplicar rate limiting
* Centralizar seguridad

Modelo Zero Trust recomendado.

---

# 🧪 Laboratorios incluidos

---

## 🔹 Lab 01 – PostgreSQL

* Definir servicio Postgres
* Configurar conexión
* Validar migración

---

## 🔹 Lab 02 – Clustering

* Levantar múltiples instancias
* Configurar caché
* Validar sesión compartida

---

## 🔹 Lab 03 – Backup

* Exportar realm
* Importar realm
* Simular restauración

---

# 🧠 Modelo mental final

```text
Alta disponibilidad = evitar caída
Disaster recovery = sobrevivir a la caída
Observabilidad = detectar problemas
```

Producción no es solo levantar contenedores.

---

# ⚠ Errores comunes

❌ Usar H2 en producción
❌ No configurar hostname correctamente
❌ No activar health endpoints
❌ No probar restauración
❌ No validar clustering

