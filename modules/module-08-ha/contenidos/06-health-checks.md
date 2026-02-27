# 📘 Módulo 8 – HA & Producción

## 06 – Health Checks y Observabilidad

---

# 🎯 Objetivo

Configurar correctamente:

* Health endpoints
* Readiness / Liveness
* Métricas
* Integración con Load Balancer / Kubernetes

En **Keycloak** en producción real.

IAM sin monitorización = punto ciego crítico.

---

# 🧠 1️⃣ Por qué los Health Checks son críticos

En HA:

```text id="t6m8zl"
Nodo cae
   ↓
Load Balancer debe detectarlo
   ↓
Retirarlo automáticamente
```

Si no:

* Errores 500
* Login fallidos
* Impacto masivo

---

# 🔐 2️⃣ Activar Health en Keycloak

Variables necesarias:

```bash
KC_HEALTH_ENABLED=true
KC_METRICS_ENABLED=true
```

Sin esto → endpoints no existen.

---

# 🔎 3️⃣ Endpoints disponibles

Con health activado:

```text id="v4u3hs"
/health
/health/live
/health/ready
/metrics
```

---

## 🔹 /health/live

Indica:

> El proceso está vivo.

No valida DB.

---

## 🔹 /health/ready

Indica:

> El nodo está listo para recibir tráfico.

Verifica:

* Base de datos accesible
* Migraciones completas
* Servicios internos activos

Este es el endpoint correcto para Load Balancer.

---

# 🏗 4️⃣ Arquitectura con Load Balancer

![Image](https://blog.ippon.tech/hubfs/Imported_Blog_Media/feedback-keycloak-high-availability-in-cloud-environment-aws-part-1-img2-2.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1170/1%2Aoqt8GlUYvm-OrO7gNBJjNQ.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A2000/1%2Aehr1WZvusXVGas8SJ3tZuw.png)

![Image](https://d1tcczg8b21j1t.cloudfront.net/strapi-assets/High_availability_1_41e9fa1096.png)

Modelo:

```text id="yb8fwo"
Load Balancer
   ↓
Consulta /health/ready
   ↓
Nodo responde OK
   ↓
Se mantiene en pool
```

Si responde error → se elimina del pool.

---

# ⚙️ 5️⃣ Configuración típica en Load Balancer

Ejemplo conceptual:

```text id="qz0cph"
Health check path: /health/ready
Interval: 10s
Timeout: 5s
Unhealthy threshold: 3
```

Nunca usar `/` como health check.

---

# 🔥 6️⃣ Rolling Updates seguros

En cluster:

```text id="r0snmp"
Nodo 1 → draining
Nodo 2 → activo
Nodo 3 → activo
```

Cuando `/health/ready` devuelve DOWN:

→ LB deja de enviar tráfico.

Permite:

✔ Actualizaciones sin downtime
✔ Escalado dinámico

---

# 📊 7️⃣ Métricas (Prometheus)

Con:

```bash
KC_METRICS_ENABLED=true
```

Disponible:

```text id="2i9jpk"
/metrics
```

Incluye:

* Requests
* Errores
* Tiempo de respuesta
* Estado DB
* Cache stats

Integrable con:

* Prometheus
* Grafana
* Datadog

---

# 🧠 8️⃣ Qué monitorear en producción

Crítico:

✔ Tasa de login fallido
✔ Latencia de token endpoint
✔ Errores 500
✔ Conexiones DB activas
✔ Uso de CPU / memoria

IAM es punto caliente de carga.

---

# 🚨 9️⃣ Error típico grave

❌ No activar health endpoints
❌ Usar / como health check
❌ No monitorear DB
❌ No revisar métricas tras upgrade

---

# 🔒 10️⃣ Monitoreo de base de datos

Porque:

```text id="0uawui"
Si DB cae
   ↓
Todo IAM cae
```

Monitorizar:

* Conexiones
* Locks
* Replicación (si aplica)
* Espacio en disco

---

# 🧩 11️⃣ Observabilidad madura

Modelo profesional:

```text id="mental1"
Health → disponibilidad
Metrics → rendimiento
Logs → auditoría
Alerts → acción
```

Las 4 capas son necesarias.

---

# 🎯 Qué debes retener

* /health/ready es el endpoint correcto
* Health debe integrarse con LB
* Métricas permiten detectar degradación
* Sin monitorización no hay HA real
* IAM requiere vigilancia continua
