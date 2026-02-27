# 📘 Módulo 7 – Security

## 06 – Hardening completo en producción (Checklist empresarial)

---

# 🎯 Objetivo

Aplicar endurecimiento real a
**Keycloak**
para entornos:

* Empresariales
* Regulados (sanidad, banca)
* SaaS multi-tenant
* Internet público

Aquí hablamos de postura de seguridad completa.

---

# 🧠 1️⃣ Principio base

```text
IAM comprometido = Sistema completo comprometido
```

Keycloak es el punto central de identidad.
Debe tratarse como infraestructura crítica.

---

# 🔐 2️⃣ Capa 1 – Transporte (TLS)

✔ HTTPS obligatorio
✔ TLS 1.2+
✔ Certificados válidos
✔ HSTS habilitado
✔ No exponer puerto 8080

Revisado en módulos anteriores.

---

# 🏗 3️⃣ Capa 2 – Red

Arquitectura recomendada:

![Image](https://miro.medium.com/1%2AlWQNQdw8U4olam3ZZb3STw.png)

![Image](https://www.techtarget.com/rms/onlineImages/IAM_architecture_figure1_mobile.jpg)

![Image](https://substackcdn.com/image/fetch/%24s_%21-vu2%21%2Cw_1200%2Ch_675%2Cc_fill%2Cf_jpg%2Cq_auto%3Agood%2Cfl_progressive%3Asteep%2Cg_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F005673b3-62cb-48a8-adc6-19fc69156a17_887x477.png)

![Image](https://global.discourse-cdn.com/free1/uploads/netfoundry/optimized/2X/f/f6c10c47685b29a8dd6437aa69f3a2aa09ea3a74_2_485x500.png)

Modelo:

```text
Internet
   ↓
Reverse Proxy / WAF
   ↓
Red privada
   ↓
Keycloak
   ↓
Base de datos privada
```

✔ Base de datos nunca pública
✔ Solo proxy accede a Keycloak
✔ Security Groups restrictivos

---

# 🔑 4️⃣ Capa 3 – Tokens

✔ Access tokens cortos (2–5 min)
✔ Refresh tokens limitados
✔ SSO Idle ≤ 30 min
✔ SSO Max ≤ 8h
✔ Revocación activa

Nunca tokens de 1 hora en producción pública.

---

# 🔐 5️⃣ Capa 4 – Autenticación

✔ PKCE obligatorio
✔ MFA obligatorio en perfiles críticos
✔ Brute force detection activado
✔ Password policy fuerte
✔ Verify email activo

---

# 🧩 6️⃣ Capa 5 – Clientes

✔ No usar “*” en redirect URIs
✔ No usar “*” en Web Origins
✔ Limitar CORS estrictamente
✔ Revisar client scopes innecesarios
✔ Eliminar clientes no usados

---

# 🔎 7️⃣ Capa 6 – Roles y permisos

✔ No asignar roles directamente a usuarios
✔ Usar grupos
✔ Usar roles compuestos
✔ Separar realm roles de client roles
✔ Revisar permisos administrativos

---

# 🔒 8️⃣ Capa 7 – Admin Console

✔ Acceso restringido por IP
✔ MFA obligatorio para admins
✔ No exponer admin en internet si es posible
✔ Usar realm separado para administración

---

# 📊 9️⃣ Capa 8 – Logging y auditoría

Activar:

```text
Realm Settings → Events
```

✔ Log de login
✔ Log de errores
✔ Log de cambios de roles
✔ Integración con SIEM si aplica

IAM sin auditoría = sin trazabilidad.

---

# 🔄 10️⃣ Capa 9 – Backup y resiliencia

✔ Backup de base de datos
✔ Export periódico de realms
✔ Probar restauración
✔ Monitoreo de salud (/health endpoint)

---

# 🧠 11️⃣ Capa 10 – Minimización

Eliminar:

❌ Realms de prueba
❌ Usuarios demo
❌ Clientes de laboratorio
❌ Themes no usados
❌ Providers innecesarios

Reducir superficie de ataque.

---

# 🚨 12️⃣ Errores graves frecuentes

❌ Exponer Keycloak directo a internet
❌ No usar HTTPS
❌ Dejar PKCE opcional
❌ No activar brute force detection
❌ No revisar redirect URIs
❌ Usar tokens largos

---

# 🏢 13️⃣ Checklist final resumido

```text
[ ] TLS correcto
[ ] Proxy configurado
[ ] PKCE obligatorio
[ ] MFA activo
[ ] Tokens cortos
[ ] Brute force activo
[ ] Redirect URIs restringidas
[ ] Admin protegido
[ ] Logs activados
[ ] Backups probados
```

Si uno falla → superficie de ataque aumenta.

---

# 🧠 Modelo mental final de seguridad

```text
Autenticación segura
   +
Transporte seguro
   +
Tokens cortos
   +
Red privada
   +
Auditoría
   =
IAM robusto
```

---

# 🎯 Qué debes retener

* Keycloak es infraestructura crítica
* Seguridad es multicapa
* Tokens cortos reducen riesgo
* Proxy bien configurado es obligatorio
* Hardening no es opcional en producción