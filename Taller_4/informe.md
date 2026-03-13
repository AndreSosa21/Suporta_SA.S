# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Taller 4 – Infraestructura

## 👥 Integrantes del equipo
- Andrea Sosa – AndreSosa21
- Samuel Rodriguez – sam200630
- Juan Gomez – juangomepere

## 🧠 Descripción general del trabajo

El objetivo de este taller fue elaborar el **mapa de infraestructura tecnológica actual** de la empresa Suporta S.A.S, identificando los componentes, herramientas y flujos de comunicación que soportan hoy el proceso de gestión de capacitaciones virtuales dirigidas a empresas cliente. A diferencia de los talleres anteriores (donde se modeló el proceso en BPMN y el modelo de datos), aquí el foco está en **cómo se sostiene ese proceso desde el punto de vista técnico y de infraestructura**, incluyendo dispositivos, servicios en la nube, canales de comunicación y puntos de integración entre los actores.

El punto de partida fue reconocer que Suporta S.A.S opera en su estado actual **sin ningún tipo de automatización**: los reportes se generan manualmente en Excel, los links de evaluación se distribuyen por correo y WhatsApp, y el seguimiento del cumplimiento se realiza filtrando hojas de cálculo a mano. Este diagnóstico fue la base para construir el mapa de infraestructura real y, a partir de él, identificar debilidades y cuellos de botella.

## 🔧 Proceso de desarrollo

El trabajo inició revisando el proceso descrito en el Taller 1 (BPMN) pero eliminando cualquier supuesto de automatización, para reflejar fielmente la operación actual. A partir de ahí identificamos los componentes que realmente usa cada actor:

- **Coordinadora de capacitaciones y Asesora de seguimiento**: operan desde sus equipos personales o de oficina (PC/laptop), usando Microsoft Excel como herramienta central de seguimiento, Google Forms para las evaluaciones, y correo electrónico (Gmail u Outlook) para comunicarse con los clientes.
- **Google Drive / Google Workspace**: actúa como repositorio compartido de archivos y como plataforma que aloja los formularios. No hay un servidor propio ni base de datos estructurada.
- **Clientes (empresas contratantes)**: reciben por correo el informe diario como archivo Excel adjunto; internamente lo revisan de forma manual y usan WhatsApp para recordarle a sus empleados completar la evaluación.
- **Empleados**: acceden al formulario de evaluación a través de un link enviado por correo o WhatsApp, sin ninguna plataforma ni portal dedicado.
- **Zoom / Google Meet**: herramienta externa para las sesiones de capacitación virtual, sin integración con el seguimiento.

Con este inventario definimos los nodos del diagrama, organizados en tres zonas: Suporta S.A.S, Internet/Nube Pública y Cliente/Empresa Contratante. Luego trazamos los flujos de datos entre ellos y marcamos visualmente los cuellos de botella más críticos.

## 🧩 Análisis del modelo propuesto

### Cómo se estructura el modelo entregado

El diagrama se organiza en **tres zonas diferenciadas**:

**Zona Suporta S.A.S**: contiene los dispositivos del equipo interno (PC de la Coordinadora y la Asesora), las herramientas de trabajo (Excel, Google Forms, correo electrónico, Google Drive, Zoom/Meet, WhatsApp) y los flujos entre ellas. Excel es el nodo central de esta zona porque ahí convergen la lista de empleados, los estados de cumplimiento y la generación manual de informes.

**Zona Internet / Nube Pública**: incluye Google Workspace como proveedor de los servicios de correo, Drive y Forms; Zoom/Meet como plataforma de videoconferencia; y el ISP (proveedor de internet) de banda ancha doméstica o básica sin garantías de disponibilidad. Esta zona es crítica porque toda la operación depende de estos servicios externos sin contratos de nivel de servicio (SLA) empresarial.

**Zona Cliente / Empresa Contratante**: representa el lado del cliente con el contacto de RRHH (que recibe el informe por correo), los empleados (que acceden al formulario desde sus dispositivos personales) y el uso de WhatsApp como canal de distribución informal de links.

### Cómo representa las necesidades del cliente

El mapa refleja con fidelidad el estado actual: un proceso operativo pero frágil, donde la carga recae sobre personas más que sobre sistemas. La necesidad principal del cliente (saber quién respondió y quién no, con el 100% de respuestas correctas) hoy se cubre de forma manual: la Coordinadora descarga las respuestas del Forms, las cruza con la lista de empleados en Excel filtrando por empresa, y genera un informe que envía por correo. Esto es funcional pero no escala, y depende de la disponibilidad y precisión manual de una persona.

El diagrama también muestra que no existe un portal unificado, ningún sistema de notificaciones automáticas, ni integración entre las herramientas: cada pieza opera en silos y la coordinación entre ellas es responsabilidad humana.

### Supuestos tomados

- Se asume que Suporta S.A.S no cuenta con servidor propio, data center ni VPN; toda la infraestructura es servicios de terceros en la nube.
- Se asume que las cuentas de Google Workspace son del plan gratuito o básico (sin SLA empresarial garantizado).
- Se asume que el Excel de seguimiento vive en Google Drive y es compartido solo entre el equipo interno de Suporta.
- El canal WhatsApp se identifica como informal pero real: los contactos de RRHH del cliente lo usan para redistribuir links a sus empleados porque no hay otro mecanismo.
- Se asume que los empleados acceden al formulario desde dispositivos personales (celular o computador) sin ninguna app o plataforma dedicada.

## 📈 Diagrama final entregado

> Ver archivo adjunto: `infraestructura-suporta.drawio`
>
> El diagrama contiene tres zonas (Suporta S.A.S / Internet-Nube Pública / Cliente), los nodos de infraestructura de cada actor, los flujos de datos entre ellos y los cuellos de botella señalados visualmente con advertencias ⚠️.

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento                  | Tipo                    | Descripción                                                                                                                          | Responsable                   |
|--------------------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| PC / Laptop Coordinadora             | Dispositivo (endpoint)  | Equipo desde el que se gestiona todo el proceso: carga listas, filtra Excel, genera informes y envía correos.                        | Suporta S.A.S                 |
|             |
| Microsoft Excel (libro de seguimiento) | Herramienta / Software | Archivo central del proceso: almacena la lista de empleados por empresa, registra estados (Pendiente/Aprobó/Reprobó) y genera informes. | Suporta S.A.S (Coordinadora) |
| Google Forms (evaluaciones)          | Herramienta SaaS        | Formulario en línea usado como plataforma de evaluación. Almacena respuestas en Google Sheets. Sin acceso controlado por empleado.   | Suporta S.A.S                 |
| Google Drive                         | Almacenamiento nube     | Repositorio de archivos del equipo (Excels, materiales de capacitación, formularios). Sin control de versiones ni acceso granular.   | Suporta S.A.S                 |
| Correo electrónico (Gmail / Outlook) | Canal de comunicación   | Medio para enviar links de evaluación, informes diarios y coordinar con clientes.                                                    | Suporta S.A.S / Clientes      |
| WhatsApp / Teléfono                  | Canal informal          | Usado para recordatorios urgentes y reenvío de links de evaluación a empleados. Sin trazabilidad formal.                             | Suporta S.A.S / Clientes      |
| Zoom / Google Meet                   | Herramienta SaaS        | Plataforma de videoconferencia para las sesiones virtuales de capacitación. Sin integración con el seguimiento de evaluaciones.      | Suporta S.A.S                 |
| Google Workspace (nube Google)       | Infraestructura cloud   | Proveedor de los servicios de correo, Drive y Forms. Plan básico/gratuito sin SLA empresarial.                                       | Google (externo)              |
| ISP (proveedor de internet)          | Infraestructura de red  | Conexión a internet del equipo de Suporta. Doméstica o básica, sin redundancia ni garantía de uptime.                               | ISP externo                   |
| PC / Celular del contacto RRHH       | Dispositivo (endpoint)  | Dispositivo desde el que el contacto del cliente recibe y revisa el informe diario en Excel.                                         | Cliente                       |
| Celular / PC del Empleado            | Dispositivo (endpoint)  | Dispositivo personal desde el que el empleado accede al link del Google Forms para responder la evaluación.                          | Empleado (cliente)            |
| Informe diario (Excel adjunto)       | Artefacto de datos      | Archivo Excel generado manualmente por la Coordinadora y enviado por correo al contacto RRHH del cliente.                            | Suporta S.A.S (manual)        |

## 🔍 Investigación complementaria

### Tema investigado

Buenas prácticas de arquitectura de infraestructura: modelos cloud, on-premise e híbrido aplicados a empresas de servicios de tamaño pequeño/mediano (SMB).

### Resumen

La arquitectura de infraestructura de una organización define cómo se organiza, conecta y gestiona el conjunto de componentes tecnológicos que soportan sus procesos. Para empresas de servicios como Suporta S.A.S, que operan con equipos pequeños y procesos intensivos en datos y comunicación, la elección del modelo de infraestructura tiene impacto directo en la escalabilidad, la disponibilidad y la seguridad.

El modelo **on-premise** implica que la empresa mantiene sus propios servidores físicos y gestiona toda la infraestructura internamente. Ofrece control total pero requiere inversión en hardware, personal técnico y mantenimiento, lo que lo hace poco viable para organizaciones pequeñas. El modelo **cloud puro** delega toda la infraestructura a proveedores externos (AWS, Google Cloud, Azure, etc.), reduciendo costos iniciales y permitiendo escalar bajo demanda; es especialmente adecuado para empresas sin capacidad de TI interna. El modelo **híbrido** combina ambos: datos sensibles o sistemas críticos permanecen on-premise mientras servicios de menor criticidad se trasladan a la nube, ofreciendo balance entre control y flexibilidad (Microsoft, 2023).

En el caso específico de Suporta S.A.S, la infraestructura actual es informalmente cloud (Google Workspace, Zoom) pero sin gobernanza: no hay políticas de acceso, respaldo estructurado ni SLA. Las buenas prácticas recomiendan que incluso en entornos cloud básicos se establezcan: control de accesos por roles (IAM), políticas de respaldo automatizado, uso de planes con SLA mínimo garantizado, y separación de ambientes de trabajo para evitar que un error humano afecte datos de producción (AWS Well-Architected Framework, 2023). Para una empresa que escala hacia la automatización (como se proyecta en los talleres anteriores), la migración a una arquitectura cloud estructurada —con una plataforma BPM o una base de datos real en lugar de Excel— es el camino natural y alineado con las buenas prácticas del sector.

## 📚 Referencias

- [1] Microsoft. *What is hybrid cloud?* 2023. [https://azure.microsoft.com/en-us/resources/cloud-computing-dictionary/what-is-hybrid-cloud-computing](https://azure.microsoft.com/en-us/resources/cloud-computing-dictionary/what-is-hybrid-cloud-computing)
- [2] Amazon Web Services. *AWS Well-Architected Framework*. 2023. [https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [3] Google Cloud. *Google Workspace Service Level Agreement*. s.f. [https://workspace.google.com/terms/sla.html](https://workspace.google.com/terms/sla.html)
- [4] Gartner. *Magic Quadrant for Cloud Infrastructure and Platform Services*. 2023. [https://www.gartner.com/en/documents/cloud-infrastructure-platform-services](https://www.gartner.com/en/documents/cloud-infrastructure-platform-services)

---

_Este documento hace parte de la entrega del Taller 4 del curso AREM (Arquitectura Empresarial) – Universidad de La Sabana._
