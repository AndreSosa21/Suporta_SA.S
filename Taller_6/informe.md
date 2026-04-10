# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 6 – Checklist de Cumplimiento Normativo aplicado al Sistema de Gestión de Evaluaciones Post-Capacitación de Suporta S.A.S_

---

## 👥 Integrantes del equipo
- Andrea Sosa
- Juan Gomez
- Samuel Rodriguez

---

## 🧠 Descripción general del trabajo

El objetivo de este taller fue evaluar el nivel de cumplimiento normativo del sistema de gestión de evaluaciones post-capacitación de **Suporta S.A.S**, aplicando un checklist estructurado sobre los marcos de referencia de la **Ley 1581 de 2012 (Habeas Data)**, **ISO/IEC 27001** y la normativa sectorial colombiana de **SST (Decreto 1072 de 2015)**.

Suporta S.A.S es una empresa colombiana que presta servicios de capacitación virtual a aproximadamente 30 empresas cliente, con un total de ~600 empleados. Su proceso actual es 100% manual: se crea un formulario Google Forms compartido para todos los clientes, se descarga el Excel de respuestas periódicamente, se filtra por empresa y se envía un informe diario por correo al contacto de RRHH de cada cliente. Este proceso implica el tratamiento continuo de datos personales de empleados de terceras empresas, lo que genera obligaciones legales concretas que fueron objeto de este análisis.

El checklist evaluó **28 criterios** distribuidos en seis categorías normativas: Ley 1581/Habeas Data, ISO 27001 (Control de Acceso, Gestión de Activos, Seguridad Operacional, Criptografía, Gestión de Incidentes, Gestión de Proveedores, Continuidad y Auditoría) y SST/Normativa Sectorial.

---

## 🔧 Proceso de desarrollo

El equipo estructuró el trabajo en tres fases:

**Fase 1 – Contextualización normativa:** Se revisaron los marcos aplicables al tipo de empresa y datos procesados. Dado que Suporta trata datos personales de empleados de sus clientes (nombres, correos electrónicos, resultados de evaluación), se identificaron como normativas primarias la Ley 1581 de 2012 y su Decreto Reglamentario 1377 de 2013. Por operar con servicios de terceros en la nube (Google Workspace) y enviar informes con datos personales, se incorporó ISO/IEC 27001 como marco de referencia para seguridad de la información. Adicionalmente, por tratarse de capacitaciones laborales, se incluyó el Decreto 1072 de 2015 (SG-SST).

**Fase 2 – Diligenciamiento del checklist:** Se evaluaron los 28 criterios del checklist (`checklist_cliente.xlsx`) mediante revisión del sitio web de Suporta, análisis de los flujos descritos en talleres anteriores (proceso actual BPMN – Taller 1, mapa de infraestructura – Taller 5) y verificación puntual de evidencias disponibles. Para cada criterio se determinó: nivel de cumplimiento (✅ / ❌), justificación con evidencia y recomendación de acción correctiva.

**Fase 3 – Análisis de brechas y priorización:** Los criterios con incumplimiento se consolidaron en la hoja de brechas del checklist, clasificándose por prioridad (Alta / Media) según el impacto regulatorio y operativo de cada hallazgo.

---

## 🧩 Análisis del modelo propuesto

### Resultados generales del checklist

| Resultado | Cantidad | Porcentaje |
|-----------|----------|------------|
| ✅ Cumple | 13 | 46% |
| ❌ Brecha identificada | 15 | 54% |
| **Total criterios evaluados** | **28** | **100%** |

De las 15 brechas identificadas, **9 son de prioridad Alta** y **6 de prioridad Media**.

---

### Resultados por categoría

#### Ley 1581 de 2012 / Habeas Data — 4 brechas (Alta prioridad)

Esta es la categoría con mayor riesgo regulatorio. Aunque Suporta cumple con los dos criterios formales básicos —inscripción en el Registro Nacional de Bases de Datos (RNBD) ante la SIC y comunicación del propósito del formulario a los respondientes— presenta cuatro incumplimientos críticos:

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 1 | Política de Tratamiento de Datos publicada en suportasas.com | ❌ | No se encuentra publicada en el sitio web |
| 2 | Autorización expresa de los empleados en el formulario | ❌ | El formulario no incluye casilla de autorización ni aviso de privacidad |
| 3 | Cláusula de encargo de tratamiento en contratos con clientes | ❌ | Los contratos vigentes no contemplan esta cláusula |
| 4 | Canal para ejercicio de derechos de los titulares | ❌ | No existe canal ni procedimiento documentado de Habeas Data |
| 5 | Inscripción en el RNBD ante la SIC | ✅ | Verificado |
| 6 | El formulario informa el propósito del uso de datos | ✅ | Visible para el respondiente |

La ausencia de autorización expresa en el formulario implica que Suporta está recolectando datos personales de ~600 empleados de terceros sin base legal suficiente. La falta de cláusula de encargo de tratamiento en los contratos con clientes genera responsabilidad solidaria ante cualquier incidente. Ambos incumplimientos exponen a Suporta a sanciones de la SIC de hasta 2.000 SMMLV.

#### ISO 27001 – Control de Acceso — 1 brecha (Alta prioridad)

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 10 | Solo personal autorizado de Suporta accede a los datos | ✅ | Acceso restringido internamente |
| 11 | Autenticación segura (MFA) en plataforma de formularios | ❌ | MFA no activado en la cuenta administradora |
| 12 | Revocación de accesos al desvincularse un empleado | ✅ | Proceso informal existente |

La ausencia de MFA en la cuenta administradora de Google/Microsoft representa una superficie de ataque directa sobre los datos de los ~600 empleados. Una contraseña comprometida daría acceso total al historial de evaluaciones sin segunda barrera de autenticación.

#### ISO 27001 – Gestión de Activos — Cumple

| # | Criterio | Estado |
|---|----------|--------|
| 13 | Inventario de activos de información | ✅ |
| 14 | Excel clasificados como confidenciales | ✅ |

Suporta mantiene un inventario interno de sus archivos y los trata de forma restringida, aunque esta clasificación no está formalizada en una política documentada.

#### ISO 27001 – Seguridad Operacional — 1 brecha (Alta prioridad)

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 16 | Backups periódicos de listas y evaluaciones | ✅ | Se realizan periódicamente |
| 17 | El formulario evita respuestas duplicadas o suplantación | ❌ | El formulario actual permite múltiples respuestas por empleado sin verificación de identidad |

La posibilidad de respuestas duplicadas o suplantadas compromete la integridad de los resultados de evaluación, que son la razón de ser del servicio de Suporta. Dado que estos resultados se reportan a clientes como evidencia de cumplimiento del SG-SST, un dato falso puede derivar en consecuencias legales para las empresas cliente.

#### ISO 27001 – Criptografía — 2 brechas (1 Alta, 1 Media)

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 18 | Informes enviados por correo corporativo seguro (TLS) | ❌ | Se usan cuentas personales (Gmail/Hotmail) y WhatsApp sin cifrado verificado |
| 19 | Archivos Excel protegidos con contraseña antes de enviar | ❌ | Los archivos se envían sin contraseña ni cifrado |

Los informes diarios contienen nombres, correos y estados de evaluación de los empleados de cada empresa cliente. Enviarlos por correo personal o WhatsApp sin cifrado constituye transmisión de datos personales por canal no seguro, en contravención de los principios de seguridad de la Ley 1581.

#### ISO 27001 – Gestión de Incidentes — 2 brechas (Alta prioridad)

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 20 | Protocolo ante envío erróneo de informe al cliente equivocado | ❌ | No existe protocolo documentado |
| 21 | Proceso de notificación a la SIC ante fuga de datos | ❌ | No hay procedimiento formal ni responsable designado |

El envío de un informe al cliente equivocado es un incidente plausible en el proceso manual actual (30 clientes, envío diario por correo). Sin protocolo de contención y notificación, Suporta no podría responder dentro del plazo legal de 15 días hábiles que exige la Ley 1581 para notificar a la SIC.

#### ISO 27001 – Gestión de Proveedores — 2 brechas (Media prioridad)

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 22 | Compatibilidad de Google/Microsoft Forms con la Ley 1581 | ❌ | No se han revisado los términos del proveedor en este sentido |
| 23 | País de residencia de los datos del formulario | ❌ | Se desconoce en qué país se almacenan las respuestas |

Google Forms almacena las respuestas en servidores de Google LLC (EE.UU.), lo que constituye una transferencia internacional de datos personales regulada por el Art. 26 de la Ley 1581. Sin verificar ni documentar esta situación, Suporta no puede garantizar que aplica las garantías adecuadas exigidas por la norma.

#### ISO 27001 – Continuidad — 1 brecha (Media prioridad)

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 24 | Plan de contingencia si falla la plataforma o el correo | ❌ | No existe plan alternativo documentado |
| 25 | Listas de empleados respaldadas y actualizadas | ✅ | Mantenidas en archivos controlados |

#### ISO 27001 – Auditoría — 2 brechas (Media prioridad)

| # | Criterio | Estado | Hallazgo |
|---|----------|--------|----------|
| 26 | Log de envíos de informes (fecha, destinatario, empresa) | ❌ | No se lleva registro formal de los envíos |
| 27 | Revisión periódica de listas de empleados por cliente | ✅ | Realizada periódicamente |
| 28 | Política de retención de datos de evaluaciones y empleados | ❌ | No existe política definida |

La ausencia de un log de envíos impide demostrar ante una auditoría o disputa con un cliente que los informes se entregaron correctamente. La falta de política de retención puede llevar a conservar datos personales indefinidamente (incumpliendo el principio de temporalidad de la Ley 1581) o a eliminarlos antes del mínimo de 2 años exigido por el Decreto 1072 para registros del SG-SST.

#### SST / Normativa Sectorial — Cumplimiento total

| # | Criterio | Estado |
|---|----------|--------|
| 29 | Capacitaciones cumplen requisitos del SG-SST (Decreto 1072) | ✅ |
| 30 | Evaluaciones generan evidencia válida para MinTrabajo/ARL | ✅ |
| 31 | Historial de capacitaciones conservado mínimo 2 años | ✅ |
| 32 | Temas alineados con riesgos del SG-SST de cada cliente | ✅ |

El cumplimiento completo en SST refleja la madurez operativa del servicio principal de Suporta. El registro de asistencia y resultados ya funciona como evidencia válida ante inspecciones del Ministerio del Trabajo o ARL, y los temas de capacitación se definen en función de los riesgos de cada cliente.

---

### Supuestos tomados

- Se verificó el sitio web `suportasas.com` para los criterios de publicación de política de tratamiento de datos.
- Se tomó como base documental el proceso descrito en los Talleres 1 (BPMN), 5 (infraestructura) y 4 (STRIDE) para evaluar los criterios operacionales y de seguridad.
- Se asumió que Google Forms es la plataforma de formularios activa, dado el mapa de infraestructura del Taller 5.
- La inscripción en el RNBD fue verificada como un hecho declarado por Suporta.
- Los criterios marcados como ✅ con recomendación de "documentar formalmente" indican cumplimiento práctico pero sin evidencia escrita, lo que no fue penalizado en el nivel de cumplimiento pero sí señalado como área de mejora.

---

## 📋 Resumen ejecutivo de brechas

| Categoría | Brechas Alta | Brechas Media | Total Brechas |
|-----------|-------------|---------------|---------------|
| Ley 1581 / Habeas Data | 4 | 0 | 4 |
| ISO 27001 – Control de Acceso | 1 | 0 | 1 |
| ISO 27001 – Seguridad Operacional | 1 | 0 | 1 |
| ISO 27001 – Criptografía | 1 | 1 | 2 |
| ISO 27001 – Gestión de Incidentes | 2 | 0 | 2 |
| ISO 27001 – Gestión de Proveedores | 0 | 2 | 2 |
| ISO 27001 – Continuidad | 0 | 1 | 1 |
| ISO 27001 – Auditoría | 0 | 2 | 2 |
| **Total** | **9** | **6** | **15** |

---

## 🔍 Investigación complementaria

### Tema investigado: Obligaciones del encargado del tratamiento de datos bajo la Ley 1581 y su relación con empresas de servicios B2B en Colombia

### Resumen

La Ley 1581 de 2012 y su Decreto Reglamentario 1377 de 2013 establecen una distinción fundamental entre el **responsable del tratamiento** (quien decide sobre los datos) y el **encargado del tratamiento** (quien los procesa por cuenta del responsable). Esta distinción es clave para empresas de servicios como Suporta S.A.S: cuando Suporta recolecta y procesa datos de empleados de sus clientes para generar informes de evaluación, actúa como **encargada del tratamiento**, mientras que cada empresa cliente actúa como responsable. Esta relación debe estar formalizada mediante un **contrato de encargo** que delimite finalidades, medidas de seguridad, prohibición de uso para fines propios y obligación de eliminar los datos al terminar la relación contractual (Decreto 1377/2013, Art. 25) [1].

La Superintendencia de Industria y Comercio (SIC) ha publicado guías específicas para el sector servicios que indican que la base legal para el tratamiento de datos de empleados de terceros es el **consentimiento informado** del titular, salvo que exista una relación contractual directa que lo justifique. En el caso de Suporta, los empleados no tienen relación contractual directa con la empresa, por lo que la autorización debe obtenerse explícitamente al momento de responder la evaluación. La SIC ha sancionado a empresas con multas entre 100 y 2.000 SMMLV por ausencia de autorización o política de tratamiento no publicada [2]. Para contexto, 2.000 SMMLV en 2025 equivalen a aproximadamente $2.600 millones de pesos colombianos.

Desde la perspectiva de ISO/IEC 27001:2022, la gestión de proveedores (cláusula A.5.19–A.5.23) exige que las organizaciones evalúen y documenten los controles de seguridad de sus proveedores de servicios en la nube. En el caso específico de Google Workspace, Google ofrece un **Data Processing Amendment (DPA)** que puede activarse desde el panel de administración de Google Workspace Business, que incluye compromisos de cumplimiento GDPR y define la ubicación de procesamiento de datos. Si bien Colombia no ha suscrito el GDPR europeo, la SIC ha adoptado criterios equivalentes para transferencias internacionales, y la firma de un DPA con Google constituye una garantía adecuada bajo el Art. 26 de la Ley 1581 [3]. Para Suporta, esto representa una acción de remediación de bajo costo que resuelve simultáneamente dos de las brechas identificadas (criterios 22 y 23).

### Relación con el taller

El análisis del checklist confirmó que las brechas más críticas de Suporta no son técnicas sino legales: la empresa tiene prácticas operativas razonables (backups, control de acceso básico, cumplimiento SST), pero carece de la formalización jurídica que exige tratar datos personales de 600 personas de terceras empresas. La priorización debe orientarse primero a los elementos de Habeas Data (política publicada, autorización en formulario, contratos actualizados y canal de ejercicio de derechos), que son los de mayor exposición regulatoria y los que pueden implementarse sin inversión tecnológica significativa.

---

## 📚 Referencias

Ver archivo `referencias.md`.

---

_Este documento hace parte de la entrega del Taller 6 del curso AREM (Arquitectura Empresarial) – Universidad de La Sabana._
