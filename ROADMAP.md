# 🚀 ROADMAP ESTIMADO

---

# 🟢 SESIÓN 1 – Fundamentos + Primer entorno

---

## 🔹 Bloque 1 – Base conceptual IAM / OIDC

### 📘 Contenidos

* `modules/module-01-fundamentos/contenidos/01-iam-moderno.md`
* `modules/module-01-fundamentos/contenidos/02-oauth2-vs-oidc.md`
* `modules/module-01-fundamentos/contenidos/03-tokens.md`
* `modules/module-01-fundamentos/contenidos/04-flujos-autenticacion.md`
* `modules/module-01-fundamentos/contenidos/05-arquitectura-keycloak.md`

---

## 🔹 Bloque 2 – Instalación y primer flujo real

### 📘 Contenido

* `modules/module-01-fundamentos/contenidos/06-instalacion-dev.md`

### 🧪 Labs

**Lab 01 – Entorno**

* `modules/module-01-fundamentos/labs/lab-01-keycloak-dev/01-crear-docker-compose.md`
* `modules/module-01-fundamentos/labs/lab-01-keycloak-dev/02-definir-servicio-keycloak.md`
* `modules/module-01-fundamentos/labs/lab-01-keycloak-dev/03-levantar-contenedor.md`
* `modules/module-01-fundamentos/labs/lab-01-keycloak-dev/04-validar-admin-console.md`

**Lab 02 – Primer Realm**

* `modules/module-01-fundamentos/labs/lab-02-primer-realm/01-crear-realm.md`
* `modules/module-01-fundamentos/labs/lab-02-primer-realm/02-crear-usuario.md`
* `modules/module-01-fundamentos/labs/lab-02-primer-realm/03-probar-login-ui.md`

---

# 🟡 SESIÓN 2 – Cliente OIDC + Modelado RBAC

---

## 🔹 Bloque 1 – Cliente OIDC + flujo completo

**Lab 03 – Cliente**

* `modules/module-01-fundamentos/labs/lab-03-cliente-oidc/01-crear-cliente-publico.md`
* `modules/module-01-fundamentos/labs/lab-03-cliente-oidc/02-configurar-redirect-uris.md`
* `modules/module-01-fundamentos/labs/lab-03-cliente-oidc/03-revisar-endpoints-oidc.md`

**Lab 04 – Authorization Code**

* `modules/module-01-fundamentos/labs/lab-04-authorization-code-flow/01-construir-url-autorizacion.md`
* `modules/module-01-fundamentos/labs/lab-04-authorization-code-flow/02-obtener-code.md`
* `modules/module-01-fundamentos/labs/lab-04-authorization-code-flow/03-intercambiar-code-por-token.md`

**Lab 05 – Tokens**

* `modules/module-01-fundamentos/labs/lab-05-analisis-tokens/01-decodificar-jwt.md`
* `modules/module-01-fundamentos/labs/lab-05-analisis-tokens/02-analizar-claims.md`
* `modules/module-01-fundamentos/labs/lab-05-analisis-tokens/03-validar-firma-jwks.md`

---

## 🔹 Bloque 2 – Modelado correcto (Módulo 2)

### 📘 Contenidos

* `modules/module-02-realms/contenidos/01-realm-vs-client-roles.md`
* `modules/module-02-realms/contenidos/02-grupos-y-roles-compuestos.md`
* `modules/module-02-realms/contenidos/03-service-accounts.md`
* `modules/module-02-realms/contenidos/05-modelado-organizativo.md`

### 🧪 Labs

* `modules/module-02-realms/labs/lab-01-modelado-roles/01-crear-realm-roles.md`

* `modules/module-02-realms/labs/lab-01-modelado-roles/02-crear-client-roles.md`

* `modules/module-02-realms/labs/lab-01-modelado-roles/03-roles-compuestos.md`

* `modules/module-02-realms/labs/lab-02-grupos-y-asignaciones/01-crear-grupos.md`

* `modules/module-02-realms/labs/lab-02-grupos-y-asignaciones/02-asignar-roles-a-grupos.md`

* `modules/module-02-realms/labs/lab-02-grupos-y-asignaciones/03-validar-asignaciones.md`

---

# 🔵 SESIÓN 3 – Frontend Angular

---

## 🔹 Bloque 1 – Integración básica

### 📘 Contenidos

* `modules/module-03-angular/contenidos/01-keycloak-js.md`
* `modules/module-03-angular/contenidos/02-authorization-code-pkce.md`
* `modules/module-03-angular/contenidos/03-guards.md`

### 🧪 Labs

* `modules/module-03-angular/labs/lab-01-crear-angular-app/01-generar-proyecto.md`

* `modules/module-03-angular/labs/lab-01-crear-angular-app/02-configuracion-base.md`

* `modules/module-03-angular/labs/lab-01-crear-angular-app/03-ejecutar-app.md`

* `modules/module-03-angular/labs/lab-02-integrar-keycloak/01-instalar-keycloak-js.md`

* `modules/module-03-angular/labs/lab-02-integrar-keycloak/02-configurar-init.md`

* `modules/module-03-angular/labs/lab-02-integrar-keycloak/03-validar-login.md`

---

## 🔹 Bloque 2 – Ciclo de vida y control

### 📘 Contenidos

* `modules/module-03-angular/contenidos/04-token-lifecycle.md`
* `modules/module-03-angular/contenidos/05-refresh-automatico.md`
* `modules/module-03-angular/contenidos/06-logout-centralizado.md`

### 🧪 Labs

* `modules/module-03-angular/labs/lab-03-guards/01-crear-guard.md`

* `modules/module-03-angular/labs/lab-03-guards/02-proteger-rutas.md`

* `modules/module-03-angular/labs/lab-03-guards/03-validar-redireccion.md`

* `modules/module-03-angular/labs/lab-04-token-ui/01-mostrar-claims.md`

* `modules/module-03-angular/labs/lab-04-token-ui/02-simular-expiracion.md`

* `modules/module-03-angular/labs/lab-04-token-ui/03-refresh-token.md`

* `modules/module-03-angular/labs/lab-05-logout/01-logout-centralizado.md`

* `modules/module-03-angular/labs/lab-05-logout/02-validar-sesion.md`

---

# 🟣 SESIÓN 4 – Backend + MFA

---

## 🔹 Bloque 1 – Backend seguro

### 📘 Contenidos

* `modules/module-04-backend/contenidos/01-bearer-tokens.md`
* `modules/module-04-backend/contenidos/02-validacion-jwt.md`
* `modules/module-04-backend/contenidos/03-jwks.md`
* `modules/module-04-backend/contenidos/04-roles-y-scopes.md`
* `modules/module-04-backend/contenidos/05-service-to-service.md`

### 🧪 Labs

* `modules/module-04-backend/labs/lab-01-crear-api-node/01-inicializar-proyecto.md`

* `modules/module-04-backend/labs/lab-01-crear-api-node/02-crear-servidor-express.md`

* `modules/module-04-backend/labs/lab-01-crear-api-node/03-crear-endpoint.md`

* `modules/module-04-backend/labs/lab-02-validacion-jwt/01-instalar-dependencias.md`

* `modules/module-04-backend/labs/lab-02-validacion-jwt/02-crear-middleware.md`

* `modules/module-04-backend/labs/lab-02-validacion-jwt/03-validar-jwks.md`

---

## 🔹 Bloque 2 – MFA y Flujos

### 📘 Contenidos

* `modules/module-05-mfa/contenidos/01-authentication-flows.md`
* `modules/module-05-mfa/contenidos/02-required-actions.md`
* `modules/module-05-mfa/contenidos/03-mfa-totp.md`
* `modules/module-05-mfa/contenidos/04-gestion-avanzada-usuarios.md`

### 🧪 Labs

* `modules/module-05-mfa/labs/lab-01-custom-flow/01-duplicar-flow.md`

* `modules/module-05-mfa/labs/lab-01-custom-flow/02-modificar-ejecuciones.md`

* `modules/module-05-mfa/labs/lab-01-custom-flow/03-validar-flujo.md`

* `modules/module-05-mfa/labs/lab-02-mfa-obligatorio/01-configurar-totp.md`

* `modules/module-05-mfa/labs/lab-02-mfa-obligatorio/02-forzar-required-action.md`

* `modules/module-05-mfa/labs/lab-02-mfa-obligatorio/03-validar-mfa.md`

---

# 🔴 SESIÓN 5 – Federation + Producción

---

## 🔹 Bloque 1 – Federation

### 📘 Contenidos

* `modules/module-06-federation/contenidos/01-user-federation.md`
* `modules/module-06-federation/contenidos/02-ldap.md`
* `modules/module-06-federation/contenidos/03-sincronizacion.md`
* `modules/module-06-federation/contenidos/04-identity-brokering.md`
* `modules/module-06-federation/contenidos/05-arquitecturas-hibridas.md`

### 🧪 Labs

* `modules/module-06-federation/labs/lab-01-openldap/01-definir-servicio-ldap.md`
* `modules/module-06-federation/labs/lab-01-openldap/02-levantar-ldap.md`
* `modules/module-06-federation/labs/lab-01-openldap/03-validar-conexion.md`

---

## 🔹 Bloque 2 – Seguridad avanzada + HA

### 📘 Contenidos

* `modules/module-07-security/contenidos/01-pkce-deep-dive.md`

* `modules/module-07-security/contenidos/03-ssl-y-certificados.md`

* `modules/module-07-security/contenidos/06-hardening.md`

* `modules/module-08-ha/contenidos/01-migracion-postgresql.md`

* `modules/module-08-ha/contenidos/03-clustering.md`

* `modules/module-08-ha/contenidos/05-backup-recuperacion.md`

* `modules/module-08-ha/contenidos/06-health-checks.md`

* `modules/module-08-ha/contenidos/07-api-gateway-conceptual.md`