# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
Modelado de Proceso del Cliente con BPMN

## 👥 Integrantes del equipo
- Juan Andres Gomez 
- Samuel Andres Rodriguez
- Andrea Julieth Sosa Rodriguez

## 🧠 Descripción general del trabajo
El objetivo del taller fue **modelar en BPMN** el proceso de evaluación posterior a las capacitaciones virtuales dirigidas a **empresas cliente**, garantizando que **todos los empleados** de cada organización **participen y alcancen el 100% de respuestas correctas**. Además, el modelo contempla un **mecanismo de seguimiento diario**, con **reportes por cliente (empresa)** que permiten monitorear el avance y detectar pendientes de forma oportuna.

## 🔧 Proceso de desarrollo
Empezamos entendiendo el proceso actual (Forms → Excel → filtrar por empresa → comparar con listas) y decidimos modelarlo en BPMN para separar claramente responsabilidades y automatización. Primero definimos actores y carriles (Asesora, Plataforma, RRHH del cliente y Empleado) y los datos clave (lista de empleados, respuestas, estado e informe). Luego modelamos el flujo objetivo priorizando lo que reduce trabajo: enlaces únicos por empleado, calificación automática con regla de 100% y reintentos, y un evento diario para generar y enviar el informe por empresa. Finalmente, fuimos ajustando el diagrama agregando compuertas y excepciones comunes (pendientes, reprobados, cambios en listas) hasta dejar un proceso consistente y automatizable.

## 🧩 Análisis del modelo propuesto
Cómo se estructura el modelo entregado
El modelo se organiza en un Pool con swimlanes por rol (Coordinadora de capacitaciones, Sistema de Validación de Evaluaciones, Cliente/Empresa y Empleado) para mostrar responsabilidades. El flujo se construye con eventos (inicio/fin y temporizadores), actividades (tareas de usuario y de servicio), gateways para decisiones (por ejemplo, validación de lista y validación de nota) y conectores que marcan el orden del proceso.

**Cómo representa las necesidades del cliente**
El proceso refleja la necesidad principal: reducir la carga manual de filtrar Excel y comparar empleado por empleado, reemplazándolo por un sistema que verifica automáticamente quién respondió y quién alcanzó el 100%. Además incorpora el envío diario de informe mediante un evento de tiempo (para reportar pendientes/reprobados/aprobados), y contempla el caso común de corrección de lista cuando la data inicial no está completa o tiene errores, evitando que el seguimiento se vuelva inconsistente. Este enfoque usa BPMN justamente para identificar pasos repetitivos, puntos de automatización y apoyar implementación de herramientas BPM/RPA.

**Qué supuestos se tomaron**

- Cada empleado tiene un identificador único (correo/ID) para cruzar respuestas con la lista sin ambigüedades.

- La evaluación permite reintentos hasta lograr 100% (lo que se modela como loop/iteración del intento).

- Existe un contacto de RRHH por empresa que recibe el informe diario y gestiona internamente a los pendientes.

- Se modeló Cliente y Empleado como lanes dentro del mismo Pool para mantener claridad; si se quisiera máxima “pureza” BPMN, podrían representarse como Pools separados y usar message flows entre participantes (en vez de solo sequence flows).

- El “informe diario” se envía en una hora definida (p.ej., fin de jornada) y el sistema tiene permisos para almacenar/consultar estados.

## 📈 Diagrama final entregado

![Diagrama BPMN](./modelo-final.png)


## 📋 Tabla de actores, entidades o componentes (si aplica)

| Nombre del elemento                     | Tipo                 | Descripción                                                                                                                                          | Responsable                                          |
| --------------------------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| Coordinadora de capacitaciones          | Actor (Humano)       | Agenda la capacitación, carga/actualiza listas de empleados y supervisa el proceso.                                                                  | Empresa capacitadora                                 |
| Asesora de seguimiento                  | Actor (Humano)       | Da soporte operativo, atiende casos especiales y realiza escalamiento con el cliente si hay bloqueos.                                                | Empresa capacitadora                                 |
| Sistema de verificación de evaluaciones | **Actor (Sistema)**  | Ejecuta acciones automáticas: genera enlaces/códigos, envía invitaciones/recordatorios, califica, actualiza estados y genera/envía reportes diarios. | Empresa capacitadora (TI/Operación)                  |
| Cliente / Empresa                       | Actor (Organización) | Recibe reportes diarios y gestiona internamente la participación de sus empleados.                                                                   | Cliente                                              |
| Contacto RRHH / Líder del cliente       | Actor (Humano)       | Recibe el informe diario y coordina acciones internas para completar pendientes.                                                                     | Cliente                                              |
| Empleado (participante)                 | Actor (Humano)       | Responde la evaluación y reintenta hasta obtener 100% de respuestas correctas.                                                                       | Cliente                                              |
| Capacitación                            | Entidad (Proceso)    | Sesión virtual asociada a una evaluación y un grupo de empresas/empleados.                                                                           | Empresa capacitadora                                 |
| Evaluación                              | Entidad              | Conjunto de preguntas y reglas (100% requerido, fecha límite, intentos).                                                                             | Empresa capacitadora                                 |
| Lista de empleados por empresa          | Entidad (Datos)      | Base de referencia para verificar cumplimiento (incluye identificador y empresa).                                                                    | Cliente (provee) / Empresa capacitadora (administra) |
| Respuestas / Intentos                   | Entidad (Datos)      | Registro de intentos del empleado (fecha/hora, puntaje, resultado).                                                                                  | Sistema                                              |
| Estado de cumplimiento                  | Entidad (Datos)      | Estado por empleado: Pendiente / Reprobó / Aprobó (100%).                                                                                            | Sistema                                              |
| Informe diario por empresa              | Entidad (Documento)  | Reporte automático con aprobados, reprobados y pendientes, enviado al cliente.                                                                       | Sistema                                              |
| Notificaciones y recordatorios          | Componente           | Mensajes automáticos de invitación/seguimiento según reglas definidas.                                                                               | Sistema                                              |
| Evento temporizado (reporte diario)     | Componente (BPMN)    | Dispara la generación y envío del informe a una hora definida cada día.                                                                              | Sistema                                              |



## 🔍 Investigación complementaria
### Tema investigado:


Decidimos investigar sobre las buenas prácticas de BPMN para asegurarnos de que el modelo estuviera correctamente estructurado, fuera fácil de entender y siguiera los lineamientos del estándar.

### Resumen:
BPMN es un estándar de modelado que busca representar procesos de negocio con una notación “tipo diagrama de flujo”, entendible por distintos actores y lo suficientemente precisa para soportar análisis y, en algunos contextos, traducción a componentes de proceso. En particular, la especificación BPMN 2.0.2 del Object Management Group (OMG) formaliza esa notación y su propósito como estándar para diagramar procesos de negocio (Object Management Group, 2014).

Para que un diagrama BPMN sea legible y útil, varias guías coinciden en prácticas como: mantener una secuencia lógica, usar el estándar BPMN, aplicar etiquetado estricto (nombres claros en actividades/eventos) y simplificar el diagrama evitando cruces innecesarios (Bizagi, s. f.). Además, se recomienda “modelar explícitamente” las decisiones usando gateways (en lugar de flujos condicionales implícitos) para que el lector identifique fácilmente dónde se divide/une el flujo (Camunda, s. f.). 

En el taller, estas prácticas se reflejan directamente: el modelo separa responsabilidades por roles (lanes), hace explícitos los puntos de control (p. ej., “¿lista correcta?” y “¿logró 100%?”) mediante compuertas, y usa un evento de tiempo para automatizar el reporte diario, reduciendo el trabajo manual repetitivo. Este enfoque también se alinea con la idea de “método y estilo” en BPMN: no basta con conocer los símbolos, sino aplicar convenciones para que la lógica del proceso sea inequívoca (Silver, 2011).

## 📚 Referencias
* [1] Object Management Group. *Business Process Model and Notation (BPMN), Version 2.0.2*. 2014. [https://www.omg.org/spec/BPMN/2.0.2/](https://www.omg.org/spec/BPMN/2.0.2/)
* [2] Bizagi. *Best practices in process modeling*. s. f. [https://docs.bizagi.app/docs/bizagi%20studio/Process%20wizard/Model%20Process/Best%20Practices%20in%20process%20modeling/](https://docs.bizagi.app/docs/bizagi%20studio/Process%20wizard/Model%20Process/Best%20Practices%20in%20process%20modeling/)
* [3] Camunda. *Creating readable process models*. s. f. [https://docs.camunda.io/docs/components/best-practices/modeling/creating-readable-process-models/](https://docs.camunda.io/docs/components/best-practices/modeling/creating-readable-process-models/)
* [4] Silver, B. *BPMN Method and Style* (2nd ed.). 2011. [https://books.google.com/books?id=mLDYygAACAAJ](https://books.google.com/books?id=mLDYygAACAAJ)

---

_Este documento hace parte de la entrega del taller X del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
