# PI Lamberti - Muñoz

## 📜 Descripción

Este repositorio contiene el código y la configuración del **Proyecto Integrador de Ingeniería en Computación** desarrollado por **Germán Lamberti Pecile** y **Saúl Muñoz**, como parte de la tesis de grado.  
El proyecto implementa un **sistema de control de acceso multinivel** para servidores de una infraestructura distribuida, basado en **Infraestructura de Clave Pública (PKI)**.

La solución integra autenticación mediante certificados digitales X.509, autorización basada en roles (RBAC) y auditoría centralizada de operaciones, permitiendo un control seguro y trazable del acceso a recursos críticos en un entorno universitario.

---

## 🎯 Objetivos

- **Autenticación**: Validar el acceso de usuarios técnicos a servidores mediante certificados digitales emitidos por **EJBCA**.
- **Autorización**: Restringir acciones en los servidores según el **rol** contenido en el certificado (p.ej., `sysadmin`, `databases`, `devops`).
- **Auditoría**: Registrar y centralizar eventos de ejecución de comandos en **Elastic Stack** para su análisis y trazabilidad.
- **Gestión centralizada**: Configurar roles y permisos de forma remota y unificada.

---

## 🏗 Arquitectura

El sistema está compuesto por:

1. **Servidor PKI (EJBCA)**  
   - Emite certificados X.509 para usuarios técnicos.  
   - Define perfiles de certificados (`ENDUSER`) y puntos de validación OCSP.  

2. **Servidor Bastión (Gateway)**  
   - Controla el acceso SSH usando **PAM** y un script Python que valida el certificado contra el servicio RBAC.  

3. **Servidor RBAC**  
   - Servicio en Python (FastAPI) desplegado en Docker.  
   - Recibe el `SERIAL_ID` del certificado, valida su vigencia y rol.  

4. **Servidores de destino** (`destino1`, `destino2`, etc.)  
   - Ejecutan comandos solo si el usuario tiene permisos según su rol.  
   - Reglas gestionadas con **Ansible**.

5. **Servidor ARCenter**  
   - Recolecta y almacena logs con **Filebeat**, **Elasticsearch** y **Kibana**.  
   - Permite visualizar intentos de acceso, comandos ejecutados y métricas.

---

## 🔐 Flujo de funcionamiento

1. El usuario técnico intenta acceder a un servidor vía SSH usando su **certificado digital**.
2. El servidor bastión solicita al usuario el **SERIAL_ID** de su certificado.
3. Un módulo PAM ejecuta un script Python que envía el `SERIAL_ID` al servicio RBAC.
4. El servicio RBAC valida:
   - Que el certificado sea válido y no revocado.
   - Que el rol asociado tenga permisos para acceder al destino.
5. Si la validación es exitosa, se permite el acceso y se aplican las reglas de comandos según el rol.
6. Todos los eventos se registran y centralizan para su análisis.

---

## ⚙️ Tecnologías utilizadas

- **Infraestructura de Clave Pública (PKI)**: [EJBCA](https://www.ejbca.org/)
- **Autenticación SSH + PAM**
- **FastAPI** para el servicio RBAC
- **Ansible** para gestión de roles y permisos
- **Filebeat, Elasticsearch, Kibana** para auditoría
- **Docker & Docker Compose**
- **VMware** para la virtualización de servidores

---
## 🖥 Topología de red

[ Usuario Técnico ] -> [ Servidor Bastión / Gateway ] -> [ Servicio RBAC ] <--> [ EJBCA / OCSP ] --> [ Servidores de destino ] --> [ Servidor ARCenter (Elastic Stack) ] 

## 🚀 Instalación

> ⚠️ Este proyecto está diseñado para un entorno de laboratorio controlado. No se recomienda su despliegue en producción sin adaptaciones de seguridad adicionales.

### 1. Clonar el repositorio
```bash
git clone https://github.com/gerlamberti/PI-lamberti-munoz.git
cd PI-lamberti-munoz


