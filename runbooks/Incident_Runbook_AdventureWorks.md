# Runbook de Incidentes – AdventureWorks
Versión: 1.0  
Última actualización: 27 Nov 2025  
Responsable: Ricardo López – DevOps Engineer  
Aprobado por: Carlos M. Fajardo – SRE Manager

---

## 1. Propósito
Este documento define los procedimientos para atender, diagnosticar, recuperar y documentar incidentes que afecten al sistema de facturación AdventureWorks. Su misión es reducir el tiempo medio de recuperación (MTTR) y asegurar continuidad operativa.

---

## 2. Clasificación de Incidentes

CRITICAL (SEV-1): Caída total del servicio o impacto severo.  
HIGH (SEV-2): Degradación significativa con servicio afectado.  
MEDIUM (SEV-3): Problemas moderados o intermitentes.  
LOW (SEV-4): Incidencias menores sin impacto mayor.

Ejemplos:
• API sin respuesta  
• Errores 5xx elevados  
• Latencia extrema  
• Warnings o fallas esporádicas  

---

## 3. Procedimiento ante Incidente CRITICAL

### 3.1 Notificación Automática
Cuando ocurre un incidente crítico, el sistema de monitoreo genera:
• Notificación automática al canal Slack: alertas-devops  
• Correo corporativo al equipo SRE/DevOps  
• Registro del evento mediante Loki  

Ejemplo de alerta:  
CRITICAL – Servicio Caído – Instancia app-staging-01 (up == 0)

---

### 3.2 Acciones Inmediatas

1. Verificar disponibilidad del servicio consultando la URL de salud.  
2. Revisar en Grafana indicadores como disponibilidad, latencia, errores 5xx, CPU y RAM.  
3. Analizar logs en Loki usando búsquedas por palabras clave: error, panic, timeout, 503, ECONNRESET.

---

## 4. Diagnóstico Detallado

4.1 Validar el estado del servicio en el servidor correspondiente.  
4.2 Revisar consumo de recursos del sistema operativo: CPU, memoria y disco.  
4.3 Confirmar que dependencias (Redis, base de datos, APIs externas) respondan correctamente.

---

## 5. Procedimiento de Recuperación

• Reiniciar el servicio involucrado.  
• Reiniciar contenedores asociados si es necesario.  
• Limpiar archivos temporales que puedan afectar la operación.  
• Verificar que la API responda correctamente y que las métricas regresen a valores normales.  
• Evaluar escalamiento si se detecta saturación.

El incidente se considera estabilizado cuando:
• No existen alertas activas  
• Los errores 5xx disminuyen a cero  
• La latencia es estable  
• El sistema opera correctamente durante al menos 5 minutos  

---

## 6. Procedimiento de Rollback

### Rollback Automático (IA)
Se ejecuta si se detectan condiciones anómalas como:
• Latencia > 2000 ms  
• Errores 5xx continuos  
• Falta de respuesta tras reinicios  

### Rollback Manual
• Identificar la versión estable previa.  
• Desplegar nuevamente esa versión.  
• Validar comportamiento posterior al rollback.  
• Registrar el rollback en los Release Notes.

---

## 7. Comunicación del Incidente

Canales utilizados:
• Slack  
• Correo corporativo  
• Registro de incidentes interno  

Plantilla utilizada para comunicaciones:

Incidente: Servicio Caído – AdventureWorks  
Severidad: CRITICAL (SEV-1)  
Estado: Resuelto  
Duración: 7 minutos  
Causa raíz: Falla en el servicio app.service  
Acciones tomadas: Reinicio y limpieza de temporales  
Acciones preventivas: Ajustar límites de CPU  
Responsable: Ricardo López

---

## 8. Validación Post-Incidente

Checklist:
• Validar métricas en Grafana  
• Confirmar estabilidad por al menos 15 minutos  
• Registrar el incidente en documentación interna  
• Actualizar Runbook si fue necesario  
• Crear ticket preventivo para evitar recurrencia  

---

## 9. Medidas Preventivas

• Implementar auto-healing en contenedores  
• Revisar umbrales de alerta periódicamente  
• Ejecutar pruebas de estrés y carga con mayor frecuencia  
• Optimizar consumo de recursos en microservicios críticos  
• Revisar patrones de logs para detectar anomalías tempranas  

---

## 10. Cierre del Incidente

El incidente se considera cerrado cuando:
• No quedan alertas activas  
• Se confirma la estabilidad del sistema  
• La documentación está actualizada  
• Las acciones preventivas fueron registradas  

---

## 11. Registro de Incidentes (Ejemplo)

ID: 2025-11-27-01  
Severidad: CRITICAL  
Fecha: 27/11/2025  
Duración: 7 minutos  
Responsable: Ricardo López  
Estado: Cerrado  
Descripción: Servicio Caído – app-staging-01  

---

Fin del documento.  
Runbook oficial de respuesta a incidentes del sistema AdventureWorks.
