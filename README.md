# 🛡️ Proyecto: Respuesta a Incidentes y Hardening Estructural (SGSI)

## 🎯 Objetivo del Proyecto
Documentar y analizar la respuesta a un **incidente de seguridad crítico** ocurrido en un servidor web Debian expuesto. El objetivo principal fue migrar la postura de seguridad de la organización de ser **reactiva a ser proactiva y resiliente** mediante la implementación de soluciones técnicas y la formalización de políticas basadas en el marco **ISO 27001 (SGSI)**.

## 💥 Resumen del Incidente y Compromiso
Se obtuvo **control total (Root)** de un servidor Debian debido a una conjunción de fallas:
1. **Falla Lógica:** Credenciales de autenticación débiles para el acceso SSH.
2. **Falla Estructural:** Ausencia de un *firewall* perimetral, exponiendo el Sistema Operativo directamente a Internet.

## 🛠️ Metodología Aplicada

El proyecto se ejecutó en dos fases principales, complementadas por la formalización estratégica (Fase 3).

### FASE 1: Análisis Forense y Contención (Diagnóstico)
Se utilizaron herramientas forenses para confirmar el vector de ataque y la persistencia del atacante.

| Hallazgo | Comando Clave | Propósito del Comando |
| :--- | :--- | :--- |
| **Vector de Entrada** | `journalctl -u sshd` | **Confirma** la IP de origen y el acceso con `Accepted password` para el usuario `root`. |
| **Persistencia (C2)** | `ss -tulnap` | **Detecta** una conexión **UDP ESTAB saliente** (Canal de Mando y Control o C2) activa. |
| **Artefacto Malicioso** | `rkhunter --checkall` | **Identifica** el *backdoor* troyanizado que reemplazó un binario legítimo (`/usr/bin/lwp-request`). |

### FASE 2: Pentesting, Explotación y Hardening (Solución Técnica)
Se realizaron pruebas de concepto para documentar la vulnerabilidad y se aplicó el *hardening* a nivel de *host*.

| Vulnerabilidad Explotada | Hardening Aplicado | Comando Clave |
| :--- | :--- | :--- |
| **Escalada de Privilegios (LPE)** | Principio de Mínimo Privilegio. | `gpasswd -d debian sudo` (Eliminar usuario del grupo `sudo`). |
| **Exposición Web (Apache)** | Denegar listado de directorios. | Crear `.htaccess` con `Options -Indexes`. |
| **Exposición Total de Puertos** | **Denegación Explícita** por *firewall*. | `iptables -P INPUT DROP` y reglas específicas de `ACCEPT`. |

## 📐 Solución Estructural y Marco Estratégico (Fase 3)

### 1. Hardening Arquitectónico (DMZ)
* **Acción:** El servidor web fue reubicado y aislado en una **Zona Desmilitarizada (DMZ)**, detrás de un *firewall* perimetral.
* **Impacto:** Mitiga el **Riesgo Crítico de Compromiso de LAN**, asegurando que un fallo externo no pueda propagarse a la red interna administrativa. 

[Image of DMZ Network Architecture]


### 2. Formalización de la Seguridad (SGSI y NIST)
* **ISO 27001 (SGSI):** Se formalizaron las políticas de seguridad (ej. contraseñas de 12 caracteres, autenticación por claves asimétricas SSH) como controles para mitigar el riesgo de Acceso no Autorizado a Root.
* **NIST SP 800-61:** Se adoptó este marco para establecer un **Plan de Respuesta a Incidentes** formalizado, con procedimientos claros para Contención, Erradicación y Recuperación.

## 📂 Contenido del Repositorio
* `DEFENSA.pdf`: Presentación final utilizada para la defensa del proyecto (Diapositivas y Guion).
* `Plan de Respuesta a Incidentes y Marco de SGSI (ISO 27001).pdf`: Documento detallado que formaliza las políticas de seguridad y el plan de acción (incluyendo la migración a DMZ y el monitoreo SIEM).
* `Informe de Pentesting - Fase 2_ Detección, Explotación y Hardening.pdf`: Documento técnico que detalla la explotación de LPE, la falla de Apache y la implementación de las reglas de `iptables`.
* [Otras imágenes relevantes]

## 💡 Próximos Pasos Estratégicos
El siguiente paso de madurez en la organización es la implementación de un **Sistema SIEM/LOGS** para lograr el **Monitoreo Activo** y la **Detección Temprana** de anomalías y ataques persistentes.
