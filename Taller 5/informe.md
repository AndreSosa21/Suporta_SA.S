# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 4 – Análisis de Seguridad STRIDE aplicado al Sistema de Gestión de Evaluaciones Post-Capacitación de Suporta S.A.S_

---

## 👥 Integrantes del equipo
- Andrea Sosa
- Juan Gomez
- Samuel Rodriguez

---

## 🧠 Descripción general del trabajo

El objetivo de este taller fue aplicar el marco de análisis de amenazas STRIDE al sistema de gestión de evaluaciones post-capacitación de **Suporta S.A.S**, empresa colombiana que ofrece capacitaciones virtuales a aproximadamente 30 empresas cliente, cada una con un promedio de 20 empleados (~600 empleados en total).

El proceso actual de Suporta es 100% manual: se crea un único Google Forms para todos los clientes, se descarga el Excel de respuestas periódicamente y un miembro del equipo filtra empresa por empresa, empleado por empleado, para identificar quiénes no han respondido y enviar un informe diario por correo a cada cliente. Esta dinámica genera un cuello de botella operativo significativo y expone el sistema a múltiples vectores de ataque.

El trabajo se estructuró en tres fases: **(1)** comprensión del proceso actual y modelado del sistema propuesto, **(2)** selección del flujo crítico a analizar, y **(3)** aplicación sistemática de la metodología STRIDE para identificar 12 amenazas sobre los componentes del sistema digital de evaluaciones.

---

## 🔧 Proceso de desarrollo

### Fase 1 – Levantamiento del proceso actual
El equipo comenzó documentando el mapa de infraestructura actual de Suporta (`mapa_final.drawio`), identificando actores, herramientas y flujos de información del proceso manual:

1. Coordinadora crea evaluación en Google Forms
2. Envía el mismo formulario a todos los clientes por correo/WhatsApp
3. Descarga manualmente el Excel de respuestas a diario
4. Filtra por empresa y compara empleado por empleado con la lista manual
5. Genera un informe Excel adjunto y lo envía por correo (×30 empresas/día)
6. Envía recordatorios urgentes por WhatsApp a quienes no han respondido

Se identificaron los cuellos de botella principales: el filtrado manual diario y la ausencia total de trazabilidad automática sobre quién ha contestado.

### Fase 2 – Diseño del sistema propuesto
Con base en el proceso actual, se diseñó el sistema objetivo en tres artefactos:

- **`diagrama-contexto-final.drawio`** — Diagrama de Contexto con actores externos (Contacto RRHH, Empleado) e internos (Coordinadora, Asesora de Seguimiento) y sus interacciones con el sistema.
- **`modelo-final-er.drawio`** — Modelo Entidad-Relación con las entidades clave: CLIENTE, EMPLEADO, CAPACITACIÓN, EVALUACIÓN, RESPUESTA_EMPLEADO, INFORME_DIARIO.
- **`modelo-final.drawio`** — Modelo de proceso con el flujo automatizado completo: desde la carga de lista de empleados hasta la generación y envío del informe diario.

Las decisiones clave tomadas fueron: autenticación por enlace único por empleado, calificación automática con umbral del 100%, generación de reportes por job programado diario, y aislamiento de datos por cliente (`cliente_id`) en todas las consultas.

### Fase 3 – Análisis STRIDE
Se seleccionó como flujo crítico el ciclo completo: **Carga de lista → Invitación con enlace único → Respuesta → Validación automática → Generación y envío de informe diario**. Este flujo concentra la mayor densidad de activos sensibles y fue analizado con STRIDE para identificar 12 amenazas distribuidas en las 6 categorías.

---

## 🧩 Análisis del modelo propuesto

### Cómo se estructura el modelo
El sistema propuesto se organiza en cuatro capas funcionales claramente separadas:

1. **Capa de presentación**: Plataforma de Evaluación (Forms/Quiz) accesible desde cualquier dispositivo mediante enlace único por empleado.
2. **Capa de lógica de negocio**: Sistema de Validación, Motor de Calificación Automática y Generador de Reportes Diarios.
3. **Capa de comunicación**: Motor de Notificaciones (Email/SMS) para invitaciones iniciales y recordatorios automáticos a empleados pendientes.
4. **Capa de datos**: BD de Empleados y Respuestas que almacena toda la información transaccional del sistema con aislamiento por tenant.

### Cómo representa las necesidades del cliente
El modelo responde directamente a los tres problemas core identificados:

- **Elimina el filtrado manual** reemplazándolo con consultas automáticas sobre la BD con aislamiento por `cliente_id`, generando el informe diario sin intervención humana.
- **Automatiza los recordatorios** enviando notificaciones diarias a empleados pendientes sin que la coordinadora deba revisar el Excel y enviar WhatsApps uno a uno.
- **Genera informes diarios automáticos** para cada empresa con estado consolidado (aprobados / reprobados / pendientes), enviados directamente al contacto RRHH de cada cliente.

### Supuestos tomados
- Cada empleado tiene un correo electrónico único y verificado en la lista cargada por el cliente.
- La evaluación requiere un puntaje del 100% para aprobar; no hay notas parciales ni reintentos ilimitados sin autorización.
- El sistema es **multi-tenant**: cada cliente ve únicamente sus propios datos mediante filtro obligatorio de `cliente_id`.
- Los informes se generan una vez al día mediante un job programado (cron).
- El sistema propuesto reemplaza completamente el flujo manual actual de Google Forms + Excel.

---

## 📈 Diagrama final entregado

> Los diagramas están disponibles como archivos adjuntos al taller (tabla stride cliente)
---

## 📋 Tabla de actores, entidades y componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Coordinadora de Capacitaciones | Actor interno (Suporta) | Configura evaluaciones, monitorea avance, gestiona casos especiales y supervisa el sistema. | Suporta S.A.S |
| Asesora de Seguimiento | Actor interno (Suporta) | Gestiona casos especiales, solicita corrección de listas de empleados, coordina acciones internas. | Suporta S.A.S |
| Contacto RRHH / Líder | Actor externo (cliente) | Carga y actualiza la lista de empleados por empresa, consulta estados de avance, recibe informe diario. | Empresa cliente |
| Empleado (Participante) | Actor externo (cliente) | Recibe invitación con enlace único, responde la evaluación, obtiene resultado inmediato. | Empresa cliente |
| Plataforma de Evaluación | Sistema | Presenta el formulario de preguntas, registra respuestas y las envía al motor de validación. | Suporta S.A.S |
| Sistema de Validación de Evaluaciones | Sistema | Califica automáticamente las respuestas, determina aprobación (100%) o reprobación y actualiza la BD. | Suporta S.A.S |
| Motor de Notificaciones (Email/SMS) | Sistema | Envía invitaciones iniciales y recordatorios automáticos diarios a empleados pendientes. | Suporta S.A.S |
| Generador de Reportes Diarios | Sistema | Consolida el estado por empresa cada día y envía informe a cada contacto RRHH. | Suporta S.A.S |
| BD de Empleados y Respuestas | Almacenamiento | Almacena listas de empleados por empresa, respuestas individuales, puntajes y estados de avance. | Suporta S.A.S |
| Motor de Calificación Automática | Sistema | Evalúa cada respuesta contra el criterio del 100% de preguntas correctas. | Suporta S.A.S |

---

## 🔍 Investigación complementaria

### Tema investigado: Marco STRIDE para análisis de amenazas en sistemas multi-tenant

### Resumen

STRIDE es una metodología de modelado de amenazas desarrollada por Microsoft que clasifica los riesgos de seguridad en seis categorías: **S**poofing (suplantación de identidad), **T**ampering (manipulación de datos), **R**epudiation (repudio de acciones), **I**nformation Disclosure (divulgación de información), **D**enial of Service (denegación de servicio) y **E**levation of Privilege (escalada de privilegios). Fue introducida por Loren Kohnfelder y Praerit Garg en 1999 y sigue siendo el estándar de referencia para análisis de amenazas en arquitecturas de software empresarial [1].

En sistemas **multi-tenant** como el propuesto para Suporta, la aplicación de STRIDE adquiere especial importancia porque una sola instancia del sistema procesa datos de múltiples organizaciones cliente. Las amenazas de **Information Disclosure** (I-01) y **Elevation of Privilege** (E-02) son particularmente críticas porque un fallo de aislamiento puede exponer datos de todos los clientes simultáneamente. La OWASP Top 10 2021 identifica el Broken Access Control como el riesgo número uno en aplicaciones web, y en contextos multi-tenant esto se manifiesta como IDOR (Insecure Direct Object Reference) y falta de validación de `tenant_id` [2].

La investigación también examinó las mejores prácticas del NIST SP 800-154 (*Guide to Data-Centric System Threat Modeling*) para el modelado de amenazas centrado en datos, particularmente relevante dado que el activo más valioso del sistema de Suporta son los datos de evaluación de empleados y los resultados de capacitación por empresa. Se recomienda que el sistema resultante implemente los controles de mitigación en orden de prioridad: primero las amenazas **Críticas** (S-01, I-01, E-01), luego las Altas, y finalmente las Medias, integrando revisiones de seguridad en el pipeline CI/CD desde las primeras etapas del desarrollo [3].

### Relación con el taller
La metodología STRIDE permitió estructurar el análisis de seguridad de forma sistemática sobre el flujo crítico del sistema de evaluaciones, garantizando que se cubrieran todas las categorías de amenazas relevantes para un sistema que maneja datos personales de ~600 empleados de 30 empresas distintas, con implicaciones legales directas bajo la Ley 1581 de 2012 de Colombia [5].

---

## 📚 Referencias

- [1] Kohnfelder, L. y Garg, P. *The Threats to Our Products*. Microsoft Interface. 1999. https://adam.shostack.org/microsoft/The-Threats-To-Our-Products.docx
- [2] OWASP. *OWASP Top 10 – 2021: A01 Broken Access Control*. 2021. https://owasp.org/Top10/A01_2021-Broken_Access_Control/
- [3] NIST. *SP 800-154: Guide to Data-Centric System Threat Modeling*. 2016. https://csrc.nist.gov/publications/detail/sp/800-154/draft
- [4] Microsoft. *Threat Modeling Process*. Microsoft Security Development Lifecycle. 2022. https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling
- [5] República de Colombia. *Ley 1581 de 2012 – Régimen General de Protección de Datos Personales*. 2012. https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=49981

---

_Este documento hace parte de la entrega del Taller 4 del curso AREM (Arquitectura Empresarial) – Universidad de La Sabana._
