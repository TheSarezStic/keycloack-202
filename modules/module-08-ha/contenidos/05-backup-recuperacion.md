# 📘 Módulo 8 – HA & Producción

## 05 – Backup y Recuperación (Disaster Recovery)

---

# 🎯 Objetivo

Diseñar estrategia de recuperación ante desastre para
**Keycloak** en producción:

* Backup de base de datos
* Backup lógico de realms
* Restauración controlada
* RPO / RTO claros
* Pruebas reales de recuperación

IAM sin DR probado = riesgo crítico.

---

# 🧠 1️⃣ Principio fundamental

```text id="axv51k"
Si no has probado la restauración,
no tienes backup.
```

Muchos equipos solo prueban el backup.
Pocos prueban el restore.

---

# 🏗 2️⃣ Qué debes respaldar realmente

Arquitectura:

![Image](https://www.keycloak.org/resources/images/guides/high-availability/active-active-sync.dio.svg)

![Image](https://documentation.cloud-iam.com/assets/architecture-overview.yeiwpRPE.png)

![Image](https://miro.medium.com/1%2Ansoy5asS-bbS2pVhVm22fw.png)

![Image](https://techdocs.broadcom.com/content/dam/broadcom/techdocs/us/en/assets/docops/cminderpim14/high_availability.png)

Componentes críticos:

✔ Base PostgreSQL
✔ Export lógico de realms
✔ Certificados TLS
✔ Variables de entorno
✔ Configuración proxy

No necesitas respaldar:

❌ Cache
❌ Contenedor
❌ Sesiones activas

---

# 🔐 3️⃣ Backup de PostgreSQL (obligatorio)

Ejemplo:

```bash id="h3z5vt"
pg_dump -U keycloak keycloak > backup.sql
```

En cloud (ej: RDS):

✔ Snapshots automáticos
✔ Multi-AZ
✔ Retención configurada

Frecuencia recomendada:

* Diario mínimo
* Más frecuente en entornos críticos

---

# 🔄 4️⃣ Backup lógico de realm

Export programado:

```bash id="n9r17m"
bin/kc.sh export --dir /backup --realm mi-realm
```

Útil para:

* Recuperación selectiva
* Migración entre entornos
* Versionado en Git

---

# 🔥 5️⃣ Estrategia RPO / RTO

## RPO (Recovery Point Objective)

¿Cuánto dato puedes perder?

Ejemplo:

* RPO 24h → backup diario
* RPO 1h → snapshot cada hora

---

## RTO (Recovery Time Objective)

¿Cuánto puedes tardar en restaurar?

IAM crítico suele requerir:

* RTO < 30 minutos
* Infraestructura automatizada

---

# 🧩 6️⃣ Procedimiento de recuperación real

Escenario: caída total.

Pasos:

```text id="7iv9av"
1. Restaurar PostgreSQL
2. Levantar Keycloak apuntando a DB restaurada
3. Validar /health
4. Validar login
5. Validar emisión JWT
6. Reintegrar al Load Balancer
```

---

# 🚨 7️⃣ Error común crítico

❌ Restaurar base pero olvidar certificados
❌ Restaurar realm pero cambiar hostname
❌ Cambiar claves criptográficas
❌ No probar restore antes de producción

---

# 🔒 8️⃣ Claves criptográficas y DR

Muy importante:

Las claves del realm viven en DB.

Si restauras DB:

✔ Las claves siguen siendo válidas
✔ Tokens antiguos siguen validando

Si creas realm nuevo:

❌ Claves nuevas
❌ JWT antiguos inválidos

---

# 🏢 9️⃣ Disaster Recovery avanzado (multi-región)

Arquitectura más robusta:

```text id="mental2"
Región primaria
   ↓ replicación
Región secundaria
```

Componentes:

✔ Replica DB
✔ Infra definida como código
✔ Backup cruzado entre regiones

IAM crítico suele requerir esto.

---

# 🧠 10️⃣ Prueba de DR obligatoria

Simulación real:

* Parar cluster
* Restaurar desde backup
* Medir tiempo
* Documentar pasos

Si no se prueba → no es válido.

---

# 📊 11️⃣ Checklist de DR

```text id="check1"
[ ] Backup automático PostgreSQL
[ ] Export lógico periódico
[ ] Restauración probada
[ ] Documentación de recuperación
[ ] Certificados respaldados
[ ] Infra reproducible (IaC)
```

---

# 🧠 12️⃣ Modelo mental final de resiliencia

```text id="mental3"
Alta disponibilidad = evitar caída
Disaster Recovery = sobrevivir a la caída
```

Son cosas distintas.

---

# 🎯 Qué debes retener

* DB es el activo crítico
* Backup sin restore probado no sirve
* Claves viven en DB
* Definir RPO / RTO
* DR debe estar documentado y probado

---

Con esto cerramos:

✔ PostgreSQL
✔ Producción real
✔ Clustering
✔ Migración
✔ Backup & DR
