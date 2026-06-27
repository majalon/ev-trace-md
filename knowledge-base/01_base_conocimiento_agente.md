# Base de conocimiento del EV-TRACE-MD Assistant

## 1. Finalidad y estado

Este documento consolida la información operativa del marco **EV-TRACE-MD** (Evaluation and Traceability for Regulated Medical Devices) para evaluar, aprobar y seguir herramientas software de terceros empleadas en el ciclo de vida de un SaMD o en procesos del sistema de gestión de calidad. Está preparado para usarse como corpus de conocimiento del **EV-TRACE-MD Assistant**, mientras que el fichero JSON asociado aporta la misma información con estructura reutilizable por un flujo conversacional o un sistema RAG.

**Estado:** operativo (v0.7). Pendiente: normalización definitiva de la regla de pesos.

**Estadísticas del catálogo:** 6 familias (S1-S6), con S4 dividida en dos subfamilias; 8 dimensiones de riesgo; 3 criterios transversales; 74 criterios específicos; 77 criterios en total; 39 anclajes normativos y regulatorios.


## 2. Papel y límites del futuro agente

El EV-TRACE-MD Assistant se concibe como herramienta de apoyo. Debe recopilar información, homogeneizar criterios, reducir la carga de búsqueda de información pública y generar una valoración estructurada con riesgos, medidas de mitigación y evidencias preliminares.

No debe sustituir la decisión humana ni automatizar la aprobación. La resolución final corresponde al responsable de cumplimiento. Tampoco debe presentar hechos como verificados sin una evidencia identificable ni emitir una conclusión sobre la viabilidad técnica de implementación, cuestión que queda fuera del alcance previsto.

### Reglas de comportamiento del EV-TRACE-MD Assistant

- Solicitar la información que falte antes de evaluar criterios que dependan de ella.
- Distinguir siempre entre evidencia del proveedor y medida que debe ejecutar la organización.
- Cuando la evidencia sea insuficiente, marcar el criterio como `no-verif`; no inferir su cumplimiento.
- Referenciar cada evidencia mediante URL o ubicación interna, fecha de consulta, localizador y breve hecho verificable.
- Elaborar una **propuesta de resolución**, no una resolución definitiva.
- Si el campo «Proveedor» está vacío, buscar en fuentes públicas la empresa o autor del producto antes de evaluar criterios.
- Para la vía técnica, aceptar un formulario reducido orientado a dependencias y componentes open source.
- Rellenar automáticamente el campo «Acción requerida» con la propuesta de medida organizativa del catálogo (campo `ov`).
- Ofrecer dos modos de informe: resumido (tabla compacta) y completo (con razonamiento, fuente consultada y acción requerida por criterio).

## 3. Alcance y activación del proceso

### 3.1 Herramientas dentro del alcance

Se consideran las herramientas software de terceros que intervienen en el diseño, desarrollo, fabricación, verificación, validación, mantenimiento o soporte del SaMD, así como las que contribuyen a demostrar el cumplimiento aplicable.

### 3.2 Vías de activación

**Activa (activa).** Un empleado o departamento identifica la necesidad y presenta una solicitud formal.
- Información mínima: Uso previsto, proceso afectado, usuarios y tratamiento de datos personales o confidenciales.
**Técnica (tecnica).** Nueva dependencia, librería o complemento incorporado al código del SaMD o a infraestructura CI/CD mediante una PR.
- Información mínima: Detalles de la dependencia o complemento, versión y cambios en el manifiesto; no necesariamente el formulario completo.
- Control asociado: Regla de protección de rama que impida fusionar la PR hasta superar la validación.
**Reactiva (reactiva).** Auditoría, revisión del inventario de activos, revisión de SBOM u otra detección de una herramienta no registrada.
- Información mínima: Identificación de la herramienta descubierta, contexto de uso y motivo de la detección.
- Control asociado: El responsable de cumplimiento inicia la validación para subsanar la falta de registro.

### 3.3 Árbol de decisión de alcance (Nivel 1)

1. ¿Podrían los fallos del software afectar a la seguridad o a la calidad del SaMD?
   - Sí: continuar a validación.
   - No: pregunta 2.
2. ¿Podría verse afectada la conformidad del SaMD si el software no funciona correctamente?
   - Sí: continuar a validación.
   - No: pregunta 3.
3. ¿El software automatiza o ejecuta una actividad exigida por los requisitos regulatorios?
   - Sí: continuar a validación.
   - No: fuera del alcance obligatorio.

**Resultado dentro del alcance:** Continuar a clasificación y evaluación.

**Resultado fuera del alcance obligatorio:** No es necesaria la validación del software. Valorar si accede, procesa o almacena información confidencial para su registro en el inventario de activos.

## 4. Clasificación de la herramienta

La clasificación determina la tabla de criterios específicos. S5 y S6 tienen tratamiento adicional: una herramienta de IA se evalúa también con S5 y un complemento o extensión se evalúa también con S6, además de la familia huésped correspondiente.

### S1 — Dependencias y componentes de terceros
**Función:** Incorporación de código externo al producto SaMD
**Ejemplos:** Librerías npm, PyPI; imágenes de contenedor (Docker); frameworks de desarrollo (Angular, Symfony)
**Aplicación en el assistant:** Aplicar CT.1–CT.3 y la tabla S1. En la vía técnica, bloquear la fusión de la PR hasta completar la validación.

### S2 — Repositorios de código y herramientas CI/CD
**Función:** Custodia del código y producción automatizada de artefactos
**Ejemplos:** GitHub, GitLab, BitBucket (control de versiones); Jenkins, GitHub Actions (CI/CD); JFrog Artifactory, Nexus (gestores de artefactos)
**Aplicación en el assistant:** Aplicar CT.1–CT.3 y la tabla S2 para repositorios, CI/CD y gestores de artefactos.

### S3 — Herramientas instalables
**Función:** Uso local de software de terceros
**Ejemplos:** Visual Studio Code, PHPStorm, Jupyter Notebook (IDEs); DBeaver, MySQL Workbench (gestores de bases de datos); Postman (clientes de API)
**Aplicación en el assistant:** Aplicar CT.1–CT.3 y la tabla S3. Los complementos se evalúan por separado con S6.

### S4.1 — Servicios SaaS de soporte interno
**Función:** Gestión del ciclo de vida del proyecto, colaboración, calidad y observabilidad
**Ejemplos:** Jira, Notion, Slack, Grafana, Google Workspace
**Aplicación en el assistant:** Aplicar CT.1–CT.3 y la tabla S4. Distinguir S4.1 (soporte interno) y S4.2 (integrado en el producto); aplicar las filas etiquetadas específicamente para cada subfamilia.

### S4.2 — Servicios SaaS integrados en el producto
**Función:** Funcionalidad de terceros incorporada al comportamiento del SaMD en tiempo de ejecución
**Ejemplos:** Intercom (atención al cliente); HubSpot (gestor de clientes); Segment (analíticas de uso); Stripe (pagos integrados)
**Aplicación en el assistant:** Aplicar CT.1–CT.3 y la tabla S4. Distinguir S4.1 (soporte interno) y S4.2 (integrado en el producto); aplicar las filas etiquetadas específicamente para cada subfamilia.

### S5 — Herramientas con IA
**Función:** Asistencia, generación y automatización mediante modelos de IA en cualquier fase
**Ejemplos:** GitHub Copilot, ChatGPT, Claude, Gemini
**Aplicación en el assistant:** Aplicar CT.1–CT.3 y S5 como módulo adicional cuando la herramienta con IA pertenezca también a otra familia.

### S6 — Complementos y extensiones
**Función:** Extensión del entorno de ejecución de otras herramientas
**Ejemplos:** Extensiones de Visual Studio Code; extensiones de navegador; plugins de Jira; add-ons de herramientas de ofimática
**Aplicación en el assistant:** Aplicar CT.1–CT.3 y S6 de forma adicional a la familia huésped S2, S3 o S4; el expediente debe referenciarse en el expediente de la herramienta huésped.

## 5. Nivel de criticidad

El nivel de criticidad modula la consecuencia del incumplimiento de los criterios. Se valoran conjuntamente la criticidad del proceso soportado y el tipo de datos manejados, en particular datos de pacientes o usuarios del SaMD, código fuente y credenciales.

1. ¿Un fallo o interrupción en el servicio podría causar un impacto severo en la operativa?
   - Sí: Critico.
   - No: pregunta 2.
2. ¿Un fallo o interrupción en el servicio podría causar un incumplimiento legal?
   - Sí: Critico.
   - No: pregunta 3.
3. ¿El proveedor tratará datos sensibles, reservados o confidenciales?
   - Sí: Importante.
   - No: pregunta 4.
4. ¿Existen proveedores alternativos que ofrezcan el mismo servicio?
   - Sí: Pregunta_5.
   - No: critico.
5. ¿El proveedor tratará datos de carácter personal sin llegar a ser sensibles?
   - Sí: Importante.
   - No: pregunta 6.
6. ¿Un fallo o interrupción en el servicio podría influir en la operativa pero no afectar al servicio diario?
   - Sí: Importante.
   - No: bajo.

**Niveles resultantes:**
- **Critico:** La herramienta interviene en procesos de alto impacto.
- **Importante:** La herramienta tiene un impacto moderado.
- **Bajo:** La herramienta tiene impacto reducido.

## 6. Dimensiones de riesgo

### A1 — Identidad, autenticación y control de acceso
Riesgos derivados de una gestión insuficiente de identidades digitales y mecanismos de autenticación en herramientas de terceros: cuentas compartidas, permisos excesivos, credenciales en texto plano o bajas de acceso no formalizadas.

**Subcategorías:**
- A1.1 — Autenticación, MFA y políticas de acceso
- A1.2 — Roles, permisos, mínimo privilegio y segregación de funciones
- A1.3 — Gestión de cuentas, altas/bajas y accesos del proveedor
- A1.4 — Gestión de secretos y credenciales

### A2 — Confidencialidad
Riesgos de exposición de información restringida cuando la herramienta transmite, almacena o procesa datos fuera de un sistema controlado; incluye fuga de código, secretos, datos personales y telemetría.

**Subcategorías:**
- A2.1 — Datos tratados: transmisión, almacenamiento y exposición
- A2.2 — Cifrado y protección de ficheros
- A2.3 — Telemetría y uso de datos por el proveedor (no confundir con IA)
- A2.4 — Privacidad, gobierno del dato y ciclo de vida (retención, borrado y exportación de datos) (no confundir con cumplimiento normativo)

### A3 — Integridad
Modificaciones no autorizadas o no intencionadas en código fuente, artefactos de compilación, resultados de pruebas, evidencias o configuraciones.

**Subcategorías:**
- A3.1 — Integridad del código y artefactos
- A3.2 — Integridad de pipelines CI/CD
- A3.3 — Integridad de evidencias y registros

### A4 — Trazabilidad
Incapacidad de demostrar qué ocurrió, cuándo, quién realizó una acción o qué evidencia respalda una liberación; incluye registros insuficientes o no auditables.

**Subcategorías:**
- A4.1 — Registro de cambios y autoría
- A4.2 — Trazabilidad de requisitos y casos de prueba
- A4.3 — Logs de actividad y auditoría
- A4.4 — Retención y exportación de registros (no confundir con gobierno del dato)

### A5 — Disponibilidad
Interrupciones por caída del proveedor, discontinuidad del servicio o revocación de licencias, con impacto operativo en el fabricante.

**Subcategorías:**
- A5.1 — Disponibilidad del servicio y SLA
- A5.2 — Continuidad operativa y plan de contingencia
- A5.3 — Copias de seguridad y reversibilidad
- A5.4 — Portabilidad de datos y salida del proveedor

### A6 — Cadena de suministro
Amenazas derivadas de componentes, servicios o herramientas de terceros y sus dependencias: versiones vulnerables, obsolescencia, procedencia, actualizaciones, plugins o componentes transitivos.

**Subcategorías:**
- A6.1 — Actividad y mantenibilidad del componente
- A6.2 — Vulnerabilidades conocidas (SCA)
- A6.3 — Procedencia, firma e integridad de paquetes (relacionada con integridad pero del componentes externo incorporado)
- A6.4 — SBOM y política de actualización
- A6.5 — Plugins, extensiones y Marketplace
- A6.6 — Dependencias transitivas y subcomponentes

### A7 — Cumplimiento y garantías
Riesgos de incumplimiento y falta de garantías del proveedor: certificaciones, incidentes, licencias, DPA/SLA, transferencias, subproveedores y seguimiento.

**Subcategorías:**
- A7.1 — Certificaciones, auditorías y políticas de seguridad del proveedor
- A7.2 — Gestión de vulnerabilidades, incidentes y subproveedores
- A7.3 — Licencias de software y compatibilidad
- A7.4 — Condiciones contractuales (DPA, SLA y propiedad de resultados)
- A7.5 — Localización del dato y transferencias internacionales
- A7.6 — Monitorización del proveedor

### A8 — Gobernanza de la IA
Riesgos específicos de IA: transmisión de datos mediante prompts, reutilización para entrenamiento, salidas incorrectas o sesgadas, propiedad intelectual, falta de trazabilidad y ausencia de revisión humana.

**Subcategorías:**
- A8.1 — Clasificación regulatoria y rol de la organización
- A8.2 — Condiciones de uso de datos e inputs (entrenamiento)
- A8.3 — Supervisión humana y riesgo de outputs incorrectos
- A8.4 — Registro del uso, modelo, versión y finalidad
- A8.5 — Componentes de IA de terceros (SBOM for AI, procedencia del modelo)
- A8.6 — Normas internas del uso de IA

## 7. Matriz entre familias y dimensiones

Leyenda: **■** amenaza predominante; **○** amenaza secundaria; **—** no aplica de forma significativa.

|Familia|A1|A2|A3|A4|A5|A6|A7|A8|
|---|---|---|---|---|---|---|---|---|
|S1 – Dependencias|—|○|■|○|○|■|○|—|
|S2 – Repos y CI/CD|■|■|■|■|■|○|○|—|
|S3 – Instalables|—|■|■|○|○|—|○|—|
|S4.1 – SaaS soporte|■|■|○|■|■|—|■|—|
|S4.2 – SaaS integrado|■|■|—|○|■|○|■|—|
|S5 – IA|○|■|■|■|○|○|■|■|
|S6 – Complementos|■|■|■|○|—|■|○|—|

## 8. Roles en la validación

### Solicitante (obligatorio)
**Intervención:** Vía activa. Puede no existir en vía reactiva.
**Responsabilidades:**
- Detectar la necesidad de incorporar una nueva herramienta y activar el proceso mediante el formulario de solicitud.
- Definir con precisión el uso previsto: proceso afectado, tareas que realizará o automatizará, usuarios y tratamiento de datos personales o confidenciales.

### Responsable de cumplimiento (obligatorio)
**Intervención:** Todas las evaluaciones.
**Responsabilidades:**
- Recibir la solicitud, aplicar el árbol de decisiones y coordinar los roles de consulta.
- Supervisar la ejecución de las listas de verificación y emitir la resolución final justificada.
- Custodiar el inventario de herramientas validadas y los informes.
- Coordinar la evaluación de impacto y revalidación ante cambios relevantes.

### Responsable de la implementación (obligatorio)
**Intervención:** Durante el despliegue y cuando se requiera conocimiento técnico de integración.
**Responsabilidades:**
- Aportar conocimiento sobre la integración, los accesos y los controles técnicos viables.
- Desplegar y configurar la herramienta tras la aprobación.
- Comunicar al responsable de cumplimiento los cambios que puedan afectar al estado validado.

### Responsable de calidad (QA/RA) (consulta)
**Intervención:** Cuando la herramienta afecte directamente a procesos del sistema de gestión de calidad.
**Responsabilidades:**
- Participar en la valoración de la herramienta respecto de procesos del SGC.

### Responsable de seguridad de la información (consulta)
**Intervención:** Cuando la herramienta acceda, procese o almacene información confidencial o existan riesgos de confidencialidad, integridad o disponibilidad.
**Responsabilidades:**
- Verificar la adecuación de los controles del proveedor.
- Valorar riesgos de cadena de suministro.
- Proponer medidas de seguridad.

### Delegado de protección de datos (DPO) (consulta)
**Intervención:** Siempre que la herramienta trate datos personales y el proveedor actúe como encargado del tratamiento.
**Responsabilidades:**
- Supervisar los requisitos de protección de datos aplicables.
- Proponer medidas privacy by design y by default.

## 9. Requisitos de diseño del marco

### R1 — Diferenciación por tipología
El proceso debe modularse según la naturaleza de la herramienta, los vectores de riesgo, las obligaciones aplicables y los controles verificables.

### R2 — Suficiencia documental
La evaluación debe documentar criterios, fuentes de evidencia, riesgos, mitigaciones y resolución final.

### R3 — Proporcionalidad al nivel de criticidad
El esfuerzo y el umbral de exigencia deben modularse por impacto del fallo, uso indebido o compromiso sobre seguridad, evidencias y datos.

### R4 — Responsabilidad compartida
Cada criterio diferencia los controles nativos esperados del proveedor y las medidas complementarias de la organización.

### R5 — Cobertura del ciclo de vida del estado validado
La validación debe mantenerse y revisarse ante cambios que puedan comprometer el estado validado.

## 10. Secuencia de evaluación

1. Aplicar criterios transversales CT.1–CT.3.
2. Aplicar la tabla específica de la familia principal.
3. Aplicar S5 adicionalmente cuando haya capacidades de IA.
4. Aplicar S6 adicionalmente cuando se evalúe un complemento o extensión.
5. Calcular o asignar el peso efectivo conforme al nivel de criticidad.
6. Documentar evidencia, brechas, mitigaciones y propuesta de resolución.

### 10.1 Pesos y regla pendiente de normalización

El borrador contiene **dos lecturas que deben unificarse antes de programar una recomendación automática**:

1. **Tabla 6.** Convierte los pesos simples en función de la criticidad.
2. **Apartado 6.2.4 y leyenda del Anexo D.** Indican que los criterios con peso simple son fijos e independientes del nivel de riesgo; solo `B/C` y `C/O` son pesos duales.

| Peso bruto | Lectura según apartado 6.2.4 y Anexo D | Lectura de la Tabla 6: Crítico | Importante | Bajo |
|---|---|---|---|---|
| B | B fijo | B | C | C |
| C | C fijo | C | O | O |
| O | O fijo | B | C | O |
| B/C | B para Crítico; C para Importante o Bajo | Derivado de B | Derivado de B | Derivado de B |
| C/O | C para Crítico; O para Importante o Bajo | Derivado de C | Derivado de C | Derivado de C |

> **Regla operativa provisional para el agente:** mostrar siempre el peso bruto del criterio; no calcular el peso efectivo ni proponer una resolución automática hasta que la regla definitiva sea aprobada y versionada por la autora del marco.

### 10.2 Criterios transversales

#### CT.1 — Adecuación al uso previsto declarado (peso: B)
**Verificación del proveedor:**
- La documentación técnica del proveedor describe con claridad el uso previsto de la herramienta.
- Las funcionalidades ofrecidas cubren los requisitos que motivaron la solicitud.
**Verificación de la organización y evidencia:**
- Definir y documentar el uso previsto de la herramienta dentro del proceso. Se recibe a través del formulario de solicitud.
- Comparar el uso previsto declarado por el proveedor con el que se hará internamente.

#### CT.2 — Verificación de requisitos funcionales mínimos (peso: B)
**Verificación del proveedor:**
- La herramienta produce los resultados esperados bajo las condiciones de uso definidas.
- Existen casos de prueba o evidencias de comportamiento documentadas.
**Verificación de la organización y evidencia:**
- Diseñar y ejecutar pruebas de aceptación específicas. LAS DISEÑA LA PERSONA/ÁREA QUE DEBE USAR LA HERRAMIENTA

#### CT.3 — Reproducibilidad y estabilidad del comportamiento (peso: B)
**Verificación del proveedor:**
- La herramienta produce resultados consistentes ante los mismos datos de entrada.
- El proveedor documenta el comportamiento esperado y las limitaciones conocidas.
**Verificación de la organización y evidencia:**
- Verificar la reproducibilidad como parte de las pruebas de aceptación.
- Documentar las limitaciones conocidas de la herramienta.

### 10.3 Criterios específicos por familia

#### S1 – Dependencias y componentes de terceros — Criterios de evaluación
**Aplicación:** Aplica a librerías, paquetes, frameworks, imágenes de contenedor y cualquier componente de código incorporado directamente al producto SaMD.

##### Cadena de suministro y procedencia (A6)

###### CS1.1 — Procedencia y canal de distribución oficial (amenaza: A6.3; peso: B)
**Verificación del proveedor:**
- Verificar que el paquete se distribuye desde el repositorio oficial del proyecto (npm, PyPI, Docker Hub oficial).
- Comprobar que la URL del repositorio en los metadatos es accesible y coincide con el proyecto declarado.
- Constatar la existencia de firma criptográfica del artefacto (hash SHA-256 o firma GPG/Sigstore) verificable de forma independiente.
**Verificación de la organización:**
- Registrar en el expediente el canal de descarga utilizado.
- Si se usa un repositorio interno, documentar que el paquete fue importado desde la fuente oficial y no modificado.

###### CS1.2 — Actividad y mantenibilidad del componente (amenaza: A6.1; peso: B/C)
**Verificación del proveedor:**
- Consultar el historial de cambios del repositorio fuente: el componente debe haber tenido actividad de mantenimiento en los últimos 12 meses.
- Verificar si existe equipo o fundación que respalde el proyecto.
- Comprobar si el componente ha sido declarado obsoleto, retirato o sin soporte.
**Verificación de la organización:**
- Para componentes sin actividad reciente, evaluar si existe un fork activo mantenido o si la funcionalidad puede sustituirse.
- Documentar la decisión en el expediente.

###### CS1.3 — Vulnerabilidades conocidas (SCA) (amenaza: A6.2; peso: B)
**Verificación del proveedor:**
- Obtener el identificador exacto del paquete y su versión.
- Consultar la base de datos NVD/CVE y los avisos de seguridad del registro del paquete para verificar ausencia de vulnerabilidades conocidas no parcheadas.
**Verificación de la organización:**
- Ejecutar análisis SCA sobre el árbol de dependencias completo (Dependabot, OWASP Dependency-Check, Snyk u otros).
- Documentar el resultado con fecha y versión de la herramienta SCA utilizada.
- Si existen vulnerabilidades, registrar el CVSS y la decisión adoptada.

###### CS1.4 — Dependencias transitivas y subcomponentes (amenaza: A6.6; peso: C/O)
**Verificación del proveedor:**
- Generar el árbol de dependencias completo (npm list --all, pip-tree, mvn dependency:tree).
- Identificar todos los subcomponentes introducidos de forma transitiva.
**Verificación de la organización:**
- Aplicar los criterios CS1.1–CS1.3 a las dependencias transitivas de mayor criticidad.
- Documentar el alcance del análisis y las dependencias revisadas.

###### CS1.5 — SBOM y política de actualización (amenaza: A6.4; peso: C/O)
**Verificación del proveedor:**
- Verificar si el proyecto publica un SBOM.
- Consultar si existe política de publicación de actualizaciones de seguridad.
**Verificación de la organización:**
- Generar o mantener internamente el SBOM del producto que incluya este componente.
- Establecer proceso de revisión periódica y aplicación de parches.

##### Integridad del componente (A3)

###### CS1.6 — Integridad del artefacto descargado (amenaza: A3.1; peso: B)
**Verificación del proveedor:**
- Verificar el hash publicado del artefacto (SHA-256 o equivalente) contra el artefacto descargado antes de su incorporación.
- Comprobar si el paquete incluye firma de procedencia (Sigstore/cosign, npm provenance).
**Verificación de la organización:**
- Incorporar la verificación de integridad al pipeline CI/CD como paso obligatorio previo a la instalación.
- Registrar el hash verificado en el expediente.

##### Licencias y compatibilidad (A7)

###### CS1.7 — Licencia de software y compatibilidad (amenaza: A7.3; peso: B)
**Verificación del proveedor:**
- Identificar la licencia del componente (SPDX identifier).
- Clasificarla: permisiva (MIT, Apache 2.0, BSD), débilmente copyleft (LGPL, MPL) o fuertemente copyleft (GPL, AGPL).
- Verificar la compatibilidad con la licencia del producto SaMD.
**Verificación de la organización:**
- Mantener un inventario de licencias de todas las dependencias.
- Para licencias copyleft, evaluar el impacto sobre la propiedad intelectual del SaMD con criterio jurídico.
- Documentar la decisión.

##### Trazabilidad (A4)

###### CS1.8 — Fijación de versión y control de cambios (pinning) (amenaza: A4.1; peso: B)
**Verificación del proveedor:**
- Verificar que la dependencia está fijada a una versión concreta en el fichero de definición de dependencias (package-lock.json, poetry.lock, requirements.txt con versión exacta).
- Comprobar que no se usan rangos semánticos amplios que permitan actualizaciones automáticas no controladas.
**Verificación de la organización:**
- Establecer política de fijación de versión de dependencias.
- Documentar la versión aprobada en el expediente.
- Cualquier cambio de versión debe reiniciar el proceso de evaluación.

##### Disponibilidad y continuidad (A5)

###### CS1.9 — Riesgo de discontinuidad del componente (amenaza: A5.2; peso: C/O)
**Verificación del proveedor:**
- Evaluar indicadores de riesgo de abandono: número de mantenedores activos, existencia de patrocinadores, dependencia de un único autor, antigüedad sin releases.
- *Se diferencia de la CS1.2 sobre el plan de mantenimiento futuro.
**Verificación de la organización:**
- Para dependencias críticas con alto riesgo de abandono, evaluar alternativas o la viabilidad de mantener un fork interno.
- Documentar el plan de contingencia.

##### Confidencialidad (A2)

###### CS1.10 — Comportamiento en tiempo de ejecución y acceso a datos del entorno (amenaza: A2.1, A2.3; peso: C/O)
**Verificación del proveedor:**
- Revisar si la dependencia accede en tiempo de ejecución a variables de entorno, ficheros de configuración, credenciales del SO o datos del proceso.
- Consultar el historial de incidentes del proyecto para detectar casos previos de exfiltración de datos.
- Si el repositorio fuente es público, revisar si el código realiza llamadas de red en contextos no esperados.
**Verificación de la organización:**
- Para dependencias con acceso potencial a datos sensibles del entorno, restringir mediante configuración del entorno de ejecución: limitación de variables de entorno accesibles, aislamiento del componente y reducción de permisos.
- Documentar la evaluación.

#### S2 – Repositorios de código y herramientas CI/CD — Criterios de evaluación
**Aplicación:** Aplica a plataformas de control de versiones (GitHub, GitLab, Bitbucket), sistemas CI/CD (Jenkins, GitHub Actions) y gestores de artefactos (Artifactory, Nexus). Concentran acceso privilegiado al código fuente y a los artefactos distribuidos del SaMD.

##### Identidad, autenticación y control de acceso (A1)

###### CS2.1 — Autenticación multifactor (MFA) (amenaza: A1.1; peso: B)
**Verificación del proveedor:**
- Verificar que la plataforma soporta MFA para todos los roles y permite hacerlo obligatorio a nivel de organización.
- Obtener captura de la configuración de MFA activa en la organización.
**Verificación de la organización:**
- Activar la obligatoriedad de MFA antes del primer uso productivo.
- Revisar periódicamente (mínimo semestral) cuentas activas sin MFA.
- Registrar la política interna aprobada.

###### CS2.2 — Control de acceso y principio de mínimo privilegio (amenaza: A1.2; peso: B)
**Verificación del proveedor:**
- Verificar que la plataforma permite gestión de roles y permisos granulares por repositorio (lectura, escritura, administración) y que es posible restringir acceso a colaboradores externos.
**Verificación de la organización:**
- Documentar los roles asignados a cada miembro del equipo.
- Mantener proceso formal de baja de accesos con registro de auditoría cuando un colaborador abandona el proyecto.

###### CS2.3 — Gestión de secretos y credenciales (amenaza: A1.4; peso: B)
**Verificación del proveedor:**
- Verificar que la plataforma dispone de gestión de secretos (GitHub Secrets, GitLab CI/CD Variables protegidas) que impide su exposición en logs y en el historial de commits.
- Comprobar que el escaneo de secretos está activado.
**Verificación de la organización:**
- Establecer política que prohíba credenciales en texto plano en el repositorio.
- Verificar periódicamente el historial de commits en busca de secretos expuestos.
- Registrar el resultado de la verificación.

###### CS2.4 — Gestión de accesos del proveedor a los repositorios (amenaza: A1.3; peso: B/C)
**Verificación del proveedor:**
- Revisar las condiciones de servicio respecto al acceso del personal del proveedor al código fuente alojado.
- Verificar si el proveedor ofrece auditoría de acceso interno.
**Verificación de la organización:**
- Documentar qué información sensible reside en los repositorios.
- Verificar el uso de repositorios privados o solución on-premise para código crítico.

##### Integridad del código y los artefactos (A3)

###### CS2.5 — Protección de ramas y control de cambios (amenaza: A3.1; peso: B)
**Verificación del proveedor:**
- Verificar que la plataforma permite configurar reglas de protección de ramas que exijan:
- Revisión de código por al menos un aprobador.
- Superación de checks de CI obligatorios.
- Prohibición de push directo a la rama principal.
**Verificación de la organización:**
- Activar y documentar las reglas de protección de ramas para todas las ramas de producción y release.
- Registrar capturas de la configuración activa como evidencia.

###### CS2.6 — Integridad del pipeline CI/CD y de los artefactos producidos (amenaza: A3.2; peso: B/C)
**Verificación del proveedor:**
- Verificar si la plataforma soporta la firma de artefactos generados en el pipeline.
- Comprobar si los runners son gestionados por la plataforma o son self-hosted y, en este caso, si están sujetos a hardening documentado.
**Verificación de la organización:**
- Configurar el pipeline para que genere hashes de los artefactos publicados.
- Implementar verificación de integridad como paso obligatorio antes del despliegue.
- Mantener los runners actualizados.

###### CS2.7 — Trazabilidad de commits y autoría (amenaza: A3.3, A4.1; peso: C/O)
**Verificación del proveedor:**
- Verificar que la plataforma registra la autoría de todos los commits con identidad verificada (signed commits con GPG o SSH).
- Comprobar que el historial de commits es inmutable.
**Verificación de la organización:**
- Establecer política de firma de commits para el equipo de desarrollo.
- Documentar la política y registrar evidencia de su activación en los repositorios.

##### Trazabilidad y auditoría (A4)

###### CS2.8 — Logs de actividad y exportación de auditoría (amenaza: A4.3; peso: B/C)
**Verificación del proveedor:**
- Verificar que la plataforma genera logs de auditoría de actividad de usuarios (accesos, modificaciones de permisos, eliminaciones).
- Comprobar el periodo de retención y si son exportables.
**Verificación de la organización:**
- Establecer proceso de revisión periódica de logs de auditoría.
- Si el periodo de retención del proveedor es insuficiente, configurar exportación periódica a almacenamiento propio.

###### CS2.9 — Trazabilidad de requisitos y casos de prueba (amenaza: A4.2; peso: C/O)
**Verificación del proveedor:**
- Evaluar si la plataforma permite vincular commits, pull requests o issues a requisitos o casos de prueba (integración con herramientas de ALM).
**Verificación de la organización:**
- Establecer una convención de etiquetado de commits que referencie el identificador de requisito o tarea correspondiente.
- Documentar la convención y verificar su aplicación.

##### Confidencialidad (A2)

###### CS2.10 — Visibilidad de repositorios y exposición de información sensible (amenaza: A2.1; peso: B)
**Verificación del proveedor:**
- Verificar la visibilidad de todos los repositorios (privados vs. públicos).
- Comprobar si la plataforma dispone de escaneo de información sensible en el código (secret scanning, code scanning).
**Verificación de la organización:**
- Auditar periódicamente la visibilidad de los repositorios.
- Activar el escaneo automático de secretos y configurar alertas.
- Verificar que los repositorios son privados salvo decisión deliberada y documentada.

##### Disponibilidad y continuidad (A5)

###### CS2.11 — SLA, continuidad y plan de contingencia (amenaza: A5.1, A5.2; peso: B/C)
**Verificación del proveedor:**
- Revisar el SLA publicado: disponibilidad mínima garantizada, tiempo de respuesta ante incidencias.
- Verificar si el proveedor ofrece histórico de disponibilidad (status page).
- Comprobar si el contrato incluye compensaciones por indisponibilidad.
**Verificación de la organización:**
- Evaluar el impacto de una indisponibilidad del CI/CD sobre los compromisos de validación del SaMD.
- Establecer plan de contingencia con capacidad de compilación local de emergencia.

###### CS2.12 — Portabilidad del código y salida del proveedor (amenaza: A5.4; peso: C/O)
**Verificación del proveedor:**
- Verificar que el código fuente puede exportarse en formatos estándar (git bundle).
- Comprobar que los metadatos relevantes (issues, pull requests, wiki) pueden exportarse o migrarse.
**Verificación de la organización:**
- Documentar el procedimiento de migración o exportación del repositorio.
- Realizar y registrar al menos una prueba de exportación completa.

##### Cumplimiento y garantías del proveedor (A7)

###### CS2.13 — Certificaciones y políticas de seguridad del proveedor (amenaza: A7.1; peso: B/C)
**Verificación del proveedor:**
- Verificar la existencia de certificaciones del proveedor (ISO/IEC 27001, SOC 2 Type II, ENS).
- Consultar si el proveedor publica un programa de gestión de vulnerabilidades y proceso de divulgación responsable.
**Verificación de la organización:**
- Registrar las certificaciones verificadas con fecha de emisión y vencimiento.
- Establecer proceso de revisión anual del estado de certificación.

###### CS2.14 — Condiciones contractuales y localización de datos (amenaza: A7.4, A7.5; peso: B)
**Verificación del proveedor:**
- Revisar las condiciones de servicio: propiedad del código alojado, jurisdicción aplicable, localización de centros de datos, transferencias internacionales y política de retención tras la cancelación.
**Verificación de la organización:**
- Si el repositorio contiene código fuente del SaMD o datos de pacientes, verificar que la jurisdicción y localización de datos son compatibles con el RGPD y el MDR.
- Documentar la revisión.

###### CS2.15 — Monitorización continua y gestión de incidencias del proveedor (amenaza: A7.2, A7.6; peso: C/O)
**Verificación del proveedor:**
- Verificar si el proveedor dispone de un canal oficial de comunicación de incidencias de seguridad (status page).
- * Similar a CS2.11, distintas finalidades.
- Comprobar si el proveedor publica un proceso formal de gestión de vulnerabilidades y notificación a clientes ante incidencias.
**Verificación de la organización:**
- Establecer proceso de seguimiento activo del proveedor: suscripción a la página de estado del servicio, suscripción al canal de avisos de seguridad, revisión anual de las condiciones de servicio y del DPA.
- Registrar las revisiones realizadas con fecha.

##### Cadena de suministro del pipeline (A6)

###### CS2.16 — Procedencia e integridad de los componentes del pipeline (acciones, runners e imágenes base) (amenaza: A6.3, A6.5; peso: B/C)
**Verificación del proveedor:**
- Verificar si el marketplace del CI/CD ofrece verificación del editor.
- Comprobar si las acciones de terceros utilizadas en el pipeline tienen repositorio fuente accesible, actividad de mantenimiento reciente y firma o acreditación de procedencia verificable.
**Verificación de la organización:**
- Fijar todas las acciones de terceros a un commit hash específico en lugar de a una etiqueta de versión mutable (p. ej. uses: actions/checkout@abc1234 en lugar de @v3).
- Mantener un inventario de las acciones y runners de terceros utilizados en el pipeline.
- Revisar periódicamente su estado de mantenimiento.

#### S3 – Herramientas de desarrollo instalables — Criterios de evaluación
**Aplicación:** Aplica a software instalado localmente: IDEs, gestores de bases de datos, clientes de API.
Los complementos y extensiones se evalúan de forma separada en la tabla de verificación S6.

##### Procedencia e integridad del instalador (A6 / A3)

###### CS3.1 — Descarga desde canal oficial y verificación de integridad (amenaza: A6.3, A3.1; peso: B)
**Verificación del proveedor:**
- Verificar que el software se descarga desde el sitio oficial del proveedor o canal de distribución reconocido.
- Comprobar que el instalador dispone de firma digital del fabricante (Authenticode en Windows, notarization en macOS) verificable antes de la ejecución.
**Verificación de la organización:**
- Documentar el canal de descarga y la versión instalada.
- Verificar la firma digital del instalador antes de su ejecución.
- Registrar el hash del instalador en el expediente.

###### CS3.2 — Actividad de mantenimiento y soporte del proveedor (amenaza: A6.1; peso: B/C)
**Verificación del proveedor:**
- Verificar que el software tiene versión estable con soporte activo del fabricante.
- Consultar la política de fin de vida (EOL) y las versiones con soporte de seguridad vigente.
**Verificación de la organización:**
- Establecer proceso de seguimiento de versiones instaladas.
- No utilizar versiones EOL en entornos de producción vinculados al SaMD.

###### CS3.3 — Vulnerabilidades conocidas en la versión instalada (amenaza: A6.2; peso: B)
**Verificación del proveedor:**
- Consultar la NVD/CVE para la versión específica del software instalado.
- Verificar si el proveedor publica un canal de seguridad.
**Verificación de la organización:**
- Mantener un inventario de software instalado con versiones.
- Aplicar actualizaciones de seguridad en un plazo máximo definido.
- Documentar la gestión de vulnerabilidades.

##### Integridad del software instalado (A3)

###### CS3.4 — Integridad del software instalado y ausencia de modificaciones no autorizadas (amenaza: A3.1; peso: B/C)
**Verificación del proveedor:**
- Verificar tras la instalación que el software no ha sido modificado respecto al instalador original, mediante comparación de hashes o herramientas de verificación de integridad del sistema de archivos.
- Comprobar si el proveedor publica hashes de los binarios instalados para verificación post-instalación.
**Verificación de la organización:**
- Establecer un proceso de verificación de integridad del software instalado, especialmente tras actualizaciones.
- Documentar el resultado de la verificación como evidencia.

##### Identidad y control de acceso al equipo (A1)

###### CS3.5 — Control de instalación en equipos corporativos (amenaza: A1.2; peso: B/C)
**Verificación del proveedor:**
- Verificar si el software requiere permisos de administrador para su instalación y si existen mecanismos de distribución centralizada (MDM, SCCM, políticas de grupo).
**Verificación de la organización:**
- Establecer política de software aprobado para los equipos de desarrollo.
- La instalación de herramientas vinculadas al SaMD debe pasar por el proceso de evaluación.
- Documentar los equipos en los que está instalado el software aprobado.

##### Confidencialidad y telemetría (A2)

###### CS3.6 — Telemetría y envío de datos al proveedor (amenaza: A2.3; peso: B/C)
**Verificación del proveedor:**
- Revisar la política de privacidad del proveedor para identificar qué datos recoge la herramienta durante su uso (telemetría, código fuente analizado, diagnósticos).
- Verificar si existe la posibilidad de desactivar la telemetría de forma total o parcial.
**Verificación de la organización:**
- Evaluar si los datos recogidos incluyen información sensible.
- Si es así, desactivar la telemetría o garantizar que los datos transmitidos están dentro del alcance del DPA aplicable.
- Registrar la decisión.

###### CS3.7 — Cifrado de datos en reposo y en tránsito (amenaza: A2.2; peso: C/O)
**Verificación del proveedor:**
- Verificar si la herramienta almacena datos localmente (cachés, tokens de sesión, fragmentos de código) y si dicho almacenamiento está cifrado.
- Comprobar que las comunicaciones con servicios externos se realizan sobre TLS.
**Verificación de la organización:**
- Asegurarse de que los equipos aplican cifrado de disco completo.
- Revisar si la herramienta puede configurarse para minimizar el almacenamiento local de datos sensibles.

##### Trazabilidad y auditoría (A4)

###### CS3.8 — Registro de versiones instaladas y actualizaciones (amenaza: A4.1; peso: C/O)
**Verificación del proveedor:**
- Verificar si el proveedor documenta el historial de versiones y los cambios de seguridad en notas de versión públicas.
**Verificación de la organización:**
- Mantener registro del software instalado en los equipos de desarrollo: nombre, versión y fecha de instalación.
- Registrar las actualizaciones aplicadas y su fecha.

##### Disponibilidad y continuidad (A5)

###### CS3.9 — Dependencia de licencia y activación online (amenaza: A5.1, A5.2; peso: C/O)
**Verificación del proveedor:**
- Verificar si la herramienta requiere activación online periódica o acceso continuado a un servidor de licencias del proveedor.
- Comprobar las condiciones de uso en caso de indisponibilidad del servidor de licencias.
**Verificación de la organización:**
- Para herramientas críticas, evaluar si la dependencia de activación online representa un riesgo inaceptable.
- Considerar licencias offline o soluciones alternativas.

##### Cumplimiento y licencia (A7)

###### CS3.10 — Licencia de uso y condiciones contractuales (amenaza: A7.3, A7.4; peso: B/C)
**Verificación del proveedor:**
- Revisar los términos de la licencia: número de instalaciones permitidas, uso comercial, restricciones de uso en entornos de producción de dispositivos médicos y derechos de auditoría del proveedor sobre el uso.
**Verificación de la organización:**
- Verificar que el número de licencias adquiridas cubre las instalaciones activas.
- Registrar las licencias en el sistema de gestión de activos.
- Revisar las condiciones de uso ante cambios de versión o de plan de licencia.

#### S4 – Servicios SaaS — Criterios de evaluación
**Aplicación:** Aplica a servicios en la nube (software residente en infraestructura del proveedor).
S4.1: soporte interno (Jira, Slack, Notion).
S4.2: integrado en el SaMD (Stripe, Segment, Intercom).
Las filas aplicables exclusivamente a una subfamilia se indican entre corchetes en la columna de verificación.

##### Identidad, autenticación y control de acceso (A1)

###### CS4.1 — Autenticación y control de acceso (MFA, SSO, roles) (amenaza: A1.1, A1.2; peso: B)
**Verificación del proveedor:**
- Verificar que el servicio soporta MFA y permite hacerlo obligatorio.
- Comprobar si el servicio es integrable con el proveedor de identidad corporativo (SSO/SAML/OIDC).
- Revisar la granularidad de roles y permisos disponibles.
**Verificación de la organización:**
- Activar MFA o integrar con SSO corporativo antes del primer uso.
- Documentar la matriz de roles y permisos asignados a cada usuario o grupo.

###### CS4.2 — Gestión de altas, bajas y accesos del proveedor (amenaza: A1.3; peso: B)
**Verificación del proveedor:**
- Revisar en los términos de servicio o DPA qué personal del proveedor puede acceder a los datos de la organización y en qué condiciones.
- Verificar si existe proceso de notificación al cliente.
**Verificación de la organización:**
- Establecer procedimiento formal de alta y baja de usuarios que incluya la revocación del acceso al servicio.
- Documentar el proceso y registrar las bajas realizadas.

##### Confidencialidad y privacidad (A2)

###### CS4.3 — Cifrado en tránsito y en reposo (amenaza: A2.2; peso: B)
**Verificación del proveedor:**
- Verificar que el servicio utiliza TLS 1.2 o superior para todas las comunicaciones.
- Comprobar si los datos en reposo están cifrados y con qué estándar.
- Obtener la información de la documentación técnica o página de seguridad del proveedor.
**Verificación de la organización:**
- Registrar en el expediente el estándar de cifrado declarado por el proveedor y la fecha de verificación.

###### CS4.4 — Tratamiento de datos personales y DPA (amenaza: A2.4; peso: B)
**Verificación del proveedor:**
- Verificar si el servicio procesa datos personales en nombre de la organización.
- En caso afirmativo, comprobar la existencia de un DPA disponible y aceptado.
- Revisar condiciones del DPA: subencargados, localización, transferencias internacionales.
**Verificación de la organización:**
- Firmar o aceptar el DPA antes de la puesta en producción si el servicio procesa datos personales.
- Registrar el DPA firmado en el expediente.

###### CS4.5 — Telemetría, uso de datos y propiedad de los datos (amenaza: A2.3; peso: B/C)
**Verificación del proveedor:**
- Revisar los términos de servicio para identificar si el proveedor puede utilizar los datos de la organización para sus propios fines.
- Verificar si el contrato establece que los datos y los resultados generados pertenecen a la organización.
**Verificación de la organización:**
- Documentar la revisión de los términos.
- Si el proveedor utiliza los datos para sus propios fines, evaluar si dicho uso es compatible con las obligaciones del fabricante.

##### Integridad de los datos y trazabilidad (A3 / A4)

###### CS4.6 — Integridad, trazabilidad y retención de los registros generados (amenaza: A3.3, A4.3, A4.4; peso: B/C)
**Verificación del proveedor:**
- [S4.1] Verificar si el servicio genera logs de actividad de usuario exportables.
- Comprobar el periodo de retención de dichos logs y si son exportables a sistemas propios.
- Verificar que los registros exportados no pueden alterarse retroactivamente.
**Verificación de la organización:**
- Establecer proceso de exportación periódica de registros relevantes para el expediente de conformidad del SaMD.
- Si el periodo de retención del proveedor es inferior al requerido por el marco regulatorio, configurar la exportación periódica.

##### Disponibilidad y continuidad (A5)

###### CS4.7 — SLA y disponibilidad garantizada (amenaza: A5.1; peso: B/C)
**Verificación del proveedor:**
- Revisar el SLA del proveedor: disponibilidad mínima comprometida, ventanas de mantenimiento, proceso de notificación de incidencias y compensaciones por incumplimiento.
- Verificar el historial de disponibilidad publicado (status page).
**Verificación de la organización:**
- Evaluar el impacto de una indisponibilidad sobre el proceso del SaMD que utiliza el servicio.
- Para servicios críticos, establecer plan de contingencia.

###### CS4.8 — Portabilidad de datos y política de salida del proveedor (amenaza: A5.4; peso: B/C)
**Verificación del proveedor:**
- Verificar si el servicio permite la exportación completa de los datos en un formato estándar o portable.
- Comprobar las condiciones de acceso a los datos tras la cancelación del contrato (plazo, formato, coste).
**Verificación de la organización:**
- Documentar el procedimiento de exportación de datos.
- Realizar y registrar al menos una prueba de exportación antes de la puesta en producción para servicios críticos.

###### CS4.9 — Copias de seguridad gestionadas por el proveedor (amenaza: A5.3; peso: C/O)
**Verificación del proveedor:**
- Verificar la política de copias de seguridad del proveedor: frecuencia, retención, cifrado de las copias, procedimiento de restauración y RTO/RPO declarados.
**Verificación de la organización:**
- Para datos críticos evaluar si la política de copias del proveedor es suficiente o si se requiere una copia adicional propia.

##### Cadena de suministro del servicio (A6) — S4.2

###### CS4.10 — Subencargados y cadena de suministro del proveedor SaaS (amenaza: A6.5; peso: C/O)
**Verificación del proveedor:**
- [S4.2] Verificar si el servicio utiliza subproveedores para la prestación de la funcionalidad contratada.
- Comprobar si el DPA o los términos listan los subencargados y si el proveedor notifica cambios en la cadena.
**Verificación de la organización:**
- Evaluar el riesgo introducido por cada subencargado relevante.
- Documentar la revisión.

###### CS4.11 — Viabilidad y continuidad del proveedor SaaS (amenaza: A6.1; peso: C/O)
**Verificación del proveedor:**
- Evaluar la viabilidad del proveedor: antigüedad, modelo de negocio, financiación, historial de adquisiciones o cambios de titularidad.
- Verificar si el proveedor ha comunicado cambios relevantes en su modelo de servicio en los últimos 12 meses.
**Verificación de la organización:**
- Para servicios críticos documentar la evaluación de viabilidad del proveedor.
- Establecer criterios de alerta ante cambios relevantes (adquisición, cambio de modelo de negocio, fin del servicio).

##### Cumplimiento y garantías del proveedor (A7)

###### CS4.12 — Certificaciones de seguridad del proveedor (amenaza: A7.1; peso: B/C)
**Verificación del proveedor:**
- Verificar la existencia y vigencia de certificaciones del proveedor (ISO/IEC 27001, SOC 2 Type II, CSA STAR, ENS).
- Comprobar si el proveedor publica informes de auditoría de terceros compartibles bajo NDA.
**Verificación de la organización:**
- Registrar las certificaciones verificadas con fecha de emisión y vencimiento.
- Revisar anualmente.

###### CS4.13 — Localización de datos y transferencias internacionales (amenaza: A7.5; peso: B)
**Verificación del proveedor:**
- Identificar los países en los que el proveedor almacena y procesa los datos.
- Verificar si las transferencias fuera del EEE están amparadas por mecanismos adecuados (cláusulas contractuales tipo, decisión de adecuación).
**Verificación de la organización:**
- Si el SaMD procesa datos de pacientes europeos, verificar que la localización y transferencia de datos es compatible con el RGPD.
- Documentar la revisión.

###### CS4.14 — Monitorización continua del proveedor (amenaza: A7.6; peso: C/O)
**Verificación del proveedor:**
- Verificar si el proveedor dispone de un canal de comunicación de incidencias de seguridad y de cambios relevantes en los términos del servicio.
**Verificación de la organización:**
- Establecer proceso de seguimiento del proveedor: suscripción a la página de estado, revisión anual del DPA y los términos, gestión de notificaciones de incidencias.

#### S5 – Herramientas con inteligencia artificial — Criterios de evaluación
**Aplicación:** Aplica a asistentes de codificación, LLMs, copilotos de IDE y herramientas de análisis con ML (ChatGPT, GitHub Copilot, Claude, Gemini).
Para herramientas que combinen IA con otra familia (p. ej. GitHub Copilot = S2+S5), esta tabla se aplica en su totalidad como módulo complementario a la tabla de la familia principal.

##### Clasificación regulatoria y gobernanza (A8)

###### CS5.1 — Clasificación de riesgo conforme al Reglamento de IA de la UE (amenaza: A8.1; peso: B)
**Verificación del proveedor:**
- Identificar la clasificación de riesgo de la herramienta IA según el AI Act (inaceptable, alto riesgo, riesgo limitado, riesgo mínimo).
- Para herramientas de alto riesgo aplicables al sector sanitario, verificar si el proveedor ha realizado la evaluación de conformidad y si el sistema está inscrito en la base de datos de la UE.
**Verificación de la organización:**
- Documentar la clasificación de riesgo determinada y su fundamento.
- Si la herramienta se clasifica como de alto riesgo, escalar la decisión de aprobación al nivel directivo correspondiente.

###### CS5.2 — Rol de la organización como desplegador de IA (amenaza: A8.1; peso: B/C)
**Verificación del proveedor:**
- Verificar si los términos de uso del proveedor reconocen el rol de desplegador de la organización conforme al AI Act y si el proveedor facilita la información necesaria para cumplir las obligaciones del desplegador.
**Verificación de la organización:**
- Registrar formalmente el rol de la organización como desplegador.
- Implementar las medidas de supervisión humana requeridas y documentarlas.

###### CS5.3 — Normas internas de uso de IA aprobadas (amenaza: A8.6; peso: B)
**Verificación del proveedor:**
- No aplica (criterio organizativo).
**Verificación de la organización:**
- Verificar que la organización dispone de política interna de uso de herramientas IA aprobada que cubra: usos permitidos y prohibidos, tipos de datos admisibles, requisito de revisión crítica de las salidas y proceso de registro del uso.

##### Condiciones de uso de datos y entrenamiento del modelo (A8 / A2)

###### CS5.4 — Política de uso de datos de entrada para entrenamiento del modelo (amenaza: A8.2, A2.3; peso: B)
**Verificación del proveedor:**
- Revisar los términos de uso para determinar si los datos introducidos (prompts, código, documentos) son utilizados para el entrenamiento del modelo.
- Verificar si existe posibilidad de deshabilitar el uso de datos para entrenamiento (opt-out) o si dicha garantía está incluida en planes empresariales.
**Verificación de la organización:**
- Seleccionar un plan o configuración que excluya el uso de datos para entrenamiento cuando se introduzca información sensible.
- Documentar la configuración activa como evidencia.

###### CS5.5 — Datos permitidos para introducir en la herramienta (amenaza: A8.2, A2.1; peso: B)
**Verificación del proveedor:**
- Revisar si los términos establecen restricciones sobre el tipo de datos que pueden introducirse (datos personales, información de salud, secretos comerciales, código fuente propietario).
**Verificación de la organización:**
- Establecer en la política interna de uso de IA una clasificación de los datos que pueden y no pueden introducirse en herramientas IA externas.
- Implementar proceso de revisión previa antes de introducir datos del SaMD o de pacientes.

##### Supervisión humana y calidad de las salidas (A8)

###### CS5.6 — Supervisión humana de los resultados generados (amenaza: A8.3; peso: B)
**Verificación del proveedor:**
- Verificar si el proveedor documenta las limitaciones conocidas del modelo, los tipos de errores frecuentes (alucinaciones, sesgos) y las recomendaciones de supervisión humana para los casos de uso declarados.
**Verificación de la organización:**
- Establecer en la política interna que ninguna salida de una herramienta IA puede utilizarse directamente en un proceso del SaMD sin revisión crítica por personal cualificado.
- Documentar el proceso de revisión y los criterios de aceptación.

##### Integridad de los artefactos generados (A3)

###### CS5.7 — Integridad de los artefactos generados e incorporados al SaMD (amenaza: A3.1, A3.3; peso: B/C)
**Verificación del proveedor:**
- Verificar si el proveedor documenta el proceso de generación de salidas y si existen mecanismos de trazabilidad entre la salida generada y el modelo o versión que la produjo.
- Comprobar si el proveedor garantiza que las salidas no son alteradas durante la transmisión.
**Verificación de la organización:**
- Establecer un proceso de verificación de las salidas de la herramienta IA antes de su incorporación al código, documentación técnica o evidencias de validación del SaMD.
- Registrar qué salidas fueron revisadas, por quién y con qué resultado.
- Guardar una copia de la salida original junto con el registro de revisión.

##### Supervisión humana y calidad de las salidas (A8)

###### CS5.8 — Propiedad intelectual de las salidas generadas (amenaza: A8.3, A7.4; peso: B/C)
**Verificación del proveedor:**
- Revisar los términos del proveedor respecto a la titularidad del contenido generado: si la organización es propietaria de las salidas, si existen restricciones de uso comercial y si el proveedor puede utilizar las salidas generadas.
**Verificación de la organización:**
- Documentar la revisión de las condiciones de propiedad intelectual.
- Si las salidas se incorporan a la documentación técnica o al código del SaMD, verificar que la organización ostenta los derechos necesarios.

##### Registro y trazabilidad del uso (A8 / A4)

###### CS5.9 — Registro del uso: modelo, versión y finalidad (amenaza: A8.4, A4.4; peso: B/C)
**Verificación del proveedor:**
- Verificar si el proveedor identifica de forma inequívoca el modelo y la versión utilizada en cada interacción.
- Comprobar si el servicio ofrece historial de conversaciones exportable o API de auditoría.
**Verificación de la organización:**
- Establecer proceso de registro interno del uso de herramientas IA: nombre y versión del modelo, fecha y hora, finalidad, usuario y decisión adoptada a partir de la salida.
- Este registro forma parte del expediente de conformidad del SaMD.

##### Componentes de IA de terceros y cadena de suministro del modelo (A8 / A6)

###### CS5.10 — Procedencia del modelo y SBOM for AI (amenaza: A8.5, A6.4; peso: C/O)
**Verificación del proveedor:**
- Verificar si el proveedor publica información sobre la procedencia del modelo (datos de entrenamiento, arquitectura, versión, fecha de corte del conocimiento).
- Para modelos construidos sobre bases de terceros, comprobar si se declaran los componentes base.
- Verificar si el proveedor publica SBOM for AI.
**Verificación de la organización:**
- Registrar la información disponible sobre procedencia del modelo en el expediente.
- Evaluar si la opacidad del proveedor sobre la composición del modelo es aceptable para el nivel de riesgo de la herramienta.

###### CS5.11 — Mantenibilidad y soporte activo del modelo o versión utilizada (amenaza: A6.1, A6.2; peso: C/O)
**Verificación del proveedor:**
- Verificar si el modelo o versión utilizada sigue teniendo soporte activo del proveedor.
- Comprobar si el proveedor notifica con antelación suficiente las deprecaciones de modelos o versiones.
- Consultar si existen vulnerabilidades conocidas asociadas a los componentes del modelo.
**Verificación de la organización:**
- Establecer un proceso de seguimiento del ciclo de vida del modelo utilizado.
- Documentar la versión activa y revisar el estado de soporte de forma periódica.
- Ante una deprecación, evaluar la migración a la versión sucesora y su impacto sobre los procesos que dependen de la herramienta.

###### CS5.12 — Controles de seguridad del modelo (amenaza: A8.5, A1.4; peso: B/C)
**Verificación del proveedor:**
- Verificar si el proveedor implementa controles contra ataques de prompt injection y si existen filtros de entrada y salida activos.
- Comprobar si el proveedor publica evaluaciones de seguridad del modelo.
**Verificación de la organización:**
- Evaluar si los controles del proveedor son suficientes para el caso de uso previsto.
- Si la herramienta procesa inputs de usuarios no confiables en un flujo automatizado, implementar controles adicionales de validación de entrada.

##### Confidencialidad y privacidad (A2)

###### CS5.13 — Cifrado y aislamiento de datos entre clientes (amenaza: A2.2; peso: B)
**Verificación del proveedor:**
- Verificar que el servicio garantiza el aislamiento de datos entre diferentes clientes (tenant isolation).
- Comprobar que los datos en tránsito y en reposo están cifrados.
- Verificar si los planes empresariales ofrecen garantías adicionales de aislamiento.
**Verificación de la organización:**
- Para herramientas IA que manejan información confidencial, seleccionar planes que garanticen el aislamiento de datos.
- Documentar la configuración y las garantías contractuales.

##### Disponibilidad (A5)

###### CS5.14 — SLA y continuidad del servicio IA (amenaza: A5.1, A5.2; peso: C/O)
**Verificación del proveedor:**
- Revisar el SLA del proveedor para el plan contratado.
- Verificar el historial de disponibilidad (status page).
- Comprobar si el proveedor notifica cambios en el modelo o deprecaciones con antelación suficiente.
**Verificación de la organización:**
- Evaluar el impacto de una indisponibilidad o cambio de comportamiento del modelo sobre los procesos del SaMD.
- Establecer procedimientos alternativos para los procesos críticos que dependen de la herramienta IA.

##### Cumplimiento y condiciones contractuales (A7)

###### CS5.15 — DPA, condiciones de uso y certificaciones del proveedor IA (amenaza: A7.1, A7.4; peso: B)
**Verificación del proveedor:**
- Verificar la existencia de DPA si el servicio procesa datos personales.
- Comprobar si el proveedor dispone de certificaciones de seguridad (ISO 27001, SOC 2).
- Revisar las condiciones de uso respecto a usos prohibidos relevantes para el contexto médico.
**Verificación de la organización:**
- Firmar o aceptar el DPA.
- Registrar las certificaciones.
- Verificar que el uso previsto no está incluido en la lista de usos prohibidos del proveedor.

#### S6 – Complementos y extensiones — Criterios de evaluación
**Aplicación:** Aplica a extensiones de navegador, plugins de IDE, add-ons de herramientas SaaS.
Se evalúa siempre de forma adicional a la familia huésped (S2, S3 o S4). El expediente de evaluación de S6 debe quedar referenciado en el expediente de la herramienta huésped.

##### Procedencia e identidad del complemento (A6 / A1)

###### CS6.1 — Procedencia desde marketplace oficial y verificación del editor (amenaza: A6.3, A1.3; peso: B)
**Verificación del proveedor:**
- Verificar que el complemento se instala desde el marketplace oficial de la plataforma huésped (VS Code Marketplace, JetBrains Marketplace, Chrome Web Store, Jira Marketplace).
- Comprobar que el editor o publisher está verificado por el marketplace.
- Revisar el historial de publicaciones del editor para descartar typosquatting.
**Verificación de la organización:**
- Documentar el nombre exacto del complemento, el editor verificado, la versión instalada y el marketplace de origen.
- Prohibir la instalación de complementos desde fuera de los marketplaces oficiales.

###### CS6.2 — Actividad de mantenimiento y número de instalaciones (amenaza: A6.1; peso: B/C)
**Verificación del proveedor:**
- Verificar en el marketplace el número de instalaciones activas, la valoración de la comunidad, la fecha de la última actualización y el historial de versiones.
**Verificación de la organización:**
- Establecer umbrales mínimos de aceptabilidad (p. ej. número mínimo de instalaciones activas, última actualización no superior a 12 meses) como criterios orientativos de la evaluación.

###### CS6.3 — Repositorio fuente accesible y licencia (amenaza: A6.3, A7.3; peso: C/O)
**Verificación del proveedor:**
- Verificar si el complemento tiene repositorio fuente público accesible.
- Si es así, comprobar la licencia del software y la actividad del repositorio.
- Para complementos de código cerrado, revisar las condiciones de uso publicadas en el marketplace.
**Verificación de la organización:**
- Documentar la licencia aplicable y cualquier restricción relevante.

##### Permisos de ejecución y acceso al entorno (A1 / A2 / A3)

###### CS6.4 — Revisión de permisos declarados por el complemento (amenaza: A1.2, A2.1; peso: B)
**Verificación del proveedor:**
- Revisar la lista de permisos que solicita el complemento durante la instalación o que declara en su manifiesto (acceso al sistema de archivos, red, portapapeles, otras extensiones, ventanas del navegador).
- Evaluar si los permisos son proporcionales a la funcionalidad declarada.
**Verificación de la organización:**
- Rechazar complementos que soliciten permisos de acceso a red o al sistema de archivos cuando su función no lo requiere.
- Documentar la justificación de los permisos aceptados.

###### CS6.5 — Acceso al código fuente y al portapapeles (amenaza: A2.1, A3.1; peso: B)
**Verificación del proveedor:**
- Identificar si el complemento puede acceder al código fuente abierto en el IDE o al portapapeles del sistema.
- Evaluar si este acceso es necesario para la función declarada y si el complemento puede transmitir dicho contenido a servicios externos.
**Verificación de la organización:**
- Para complementos con acceso al código fuente del SaMD o al portapapeles, evaluar si la transmisión de datos al proveedor es aceptable.
- Si no lo es, rechazar el complemento o configurar el entorno para restringir el acceso de red del proceso.

##### Integridad (A3)

###### CS6.6 — Integridad del código fuente y evidencias accesibles desde el complemento (amenaza: A3.1, A3.3; peso: B)
**Verificación del proveedor:**
- Evaluar si el complemento tiene capacidad técnica de modificar ficheros del proyecto o del workspace activo (no solo leer, sino escribir o eliminar).
- Verificar si el complemento accede a registros, logs o evidencias almacenadas en el entorno de trabajo y si podría manipularlos.
- Si el repositorio fuente está disponible, revisar las funciones de escritura de ficheros del complemento.
**Verificación de la organización:**
- Para complementos con permisos de escritura sobre el sistema de archivos o el workspace, exigir revisión de código de las modificaciones realizadas por el complemento antes de su confirmación en el repositorio.
- Establecer alertas de monitorización sobre cambios inesperados en ficheros críticos del proyecto.

##### Permisos de ejecución y acceso al entorno (A1 / A2 / A3)

###### CS6.7 — Comunicaciones de red del complemento (amenaza: A2.1, A2.3; peso: B/C)
**Verificación del proveedor:**
- Verificar si el complemento realiza comunicaciones de red y con qué destinos.
- Esta información puede obtenerse de la documentación del complemento, del análisis del código fuente si está disponible, o de herramientas de monitorización de red.
**Verificación de la organización:**
- Para complementos que realizan comunicaciones de red, verificar que los destinos son conocidos y esperados.
- Evaluar si dichas comunicaciones implican la transmisión de datos del SaMD.

##### Trazabilidad y gestión del inventario (A4)

###### CS6.8 — Inventario de complementos instalados por equipo (amenaza: A4.1; peso: B/C)
**Verificación del proveedor:**
- No aplica (criterio organizativo).
**Verificación de la organización:**
- Mantener un inventario actualizado de los complementos instalados en los equipos, con nombre, versión, editor, plataforma huésped y fecha de aprobación.
- Verificar que ningún complemento instalado es ajeno a la lista aprobada.

##### Cumplimiento y condiciones de uso (A7)

###### CS6.9 — Política de privacidad y tratamiento de datos del complemento (amenaza: A7.4, A2.4; peso: B/C)
**Verificación del proveedor:**
- Verificar si el complemento dispone de política de privacidad publicada y si declara qué datos recoge y cómo los utiliza.
- Para complementos que acceden al código fuente o al entorno de trabajo, revisar si se aplica un DPA.
**Verificación de la organización:**
- Para complementos que recogen datos relevantes del proceso de desarrollo, registrar la revisión de la política de privacidad.
- Si los datos recogidos incluyen datos personales, verificar la base jurídica del tratamiento.

## 11. Propuesta de resolución e informe

### Aprobado
Todos los criterios aplicables se cumplen y no hay incumplimientos bloqueantes ni condicionantes pendientes de resolución.

### Aprobado condicionado
Existen incumplimientos que no impiden la aprobación, pero requieren medidas de mitigación específicas antes del uso.

### Rechazado
Existe uno o más incumplimientos bloqueantes.

**Autoridad:** la resolución final corresponde al responsable de cumplimiento.

### Contenido mínimo del informe de validación

- Identificación de la herramienta: denominación, versión o plan, familia, nivel de riesgo y uso previsto.
- Vía de activación y fechas de inicio y final de la evaluación.
- Criterios aplicados, resultado de cada criterio y fuentes o evidencias consultadas.
- Medidas de mitigación acordadas.
- Resolución final, fecha y firma del responsable de cumplimiento.
- Frecuencia de revisión periódica asignada.

**Inventario:** toda herramienta se registra, también si ha sido rechazada, con el fin de conservar el historial de la evaluación.

## 12. Seguimiento y revalidación

### Revisión periódica programada

- **Critico:** Periodicidad máxima semestral.
- **Importante:** Periodicidad máxima anual.
- **Bajo:** Periodicidad máxima trienal.

### Revalidación anticipada por evento

- Nueva versión mayor con cambios de arquitectura, funcionalidades significativas, modelo de datos o integraciones; los cambios menores pueden requerir análisis de impacto.
- Modificación de condiciones de servicio, política de privacidad o DPA, especialmente por localización de datos, cesión a terceros, subencargados o derechos sobre los datos generados.
- Vulnerabilidad CVSS alta o crítica que afecte a la herramienta o sus componentes sin parche disponible.
- Incidente de seguridad, brecha de datos o no conformidad relevante de la herramienta, interna o públicamente notificada por el proveedor.
- Cambio del uso previsto, por ampliación a nuevos procesos o datos.

## 13. Entrada de información para el agente

### 13.1 Campos ya previstos en el formulario de solicitud

**Departamento solicitante.** Indicar el área, departamento o equipo que solicita la incorporación de la herramienta. Ejemplo: Área de Sistemas.

**Nombre de la herramienta.** Indicar el área comercial o técnico de la herramienta solicitada. Ejemplo: Bitwarden

**Tipo de software.** Seleccionar la categoría que mejor describa la herramienta solicitada: Repositorio de código y herramientas CI/CD Software instalable SaaS Herramienta IA Complementos y extensiones Ejemplo: Proveedor SaaS

**Describe la herramienta.** Describir brevemente la finalidad de la herramienta, sus principales funcionalidades, el modelo de uso y el enlace oficial del proveedor o repositorio. Ejemplo: Bitwarden es una plataforma de gestión de credenciales basada en la nube (SaaS), diseñada para el almacenamiento, intercambio y generación de contraseñas. El software garantiza que los datos sólo sean accesibles para los usuarios autorizados mediante una clave maestra. Permite la gestión centralizada de identidades, la implementación de políticas de seguridad empresarial y la sincronización multiplataforma bajo un modelo de actualización continua por parte del proveedor. https://bitwarden.com/

**Tipo de licencia.** Indicar el tipo de licencia, plan o modalidad de contratación necesaria para el uso previsto. Ejemplo: Teams Aclaración: en herramientas SaaS, las funcionalidades de seguridad, administración, auditoría, integración o cumplimiento pueden variar significativamente según el plan contratado.

**Procesos en los que se usará.** Indicar los procesos internos que se verán afectados por el uso de la herramienta. Ejemplo: Procesos de mantenimiento de infraestructuras, equipamiento y herramientas software.

**Tareas para las que se hará uso de la herramienta o que serán automatizadas.** Describir el uso previsto de la herramienta dentro del flujo de trabajo de la organización. Ejemplos: - Eliminará el riesgo de almacenar contraseñas en texto plano. - Automatizar la detección de vulnerabilidades en el código. - Registrar y realizar seguimiento de incidencias con clientes. - Facilitar la planificación de las actividades del equipo. - Automatizar el envío de correos o comunicaciones internas.

**Datos personales tratados por la herramienta.** Indicar si la herramienta tratará datos personales en el contexto de uso previsto. De acuerdo con el Reglamento General de Protección de Datos, se entiende por dato personal toda información sobre una persona física identificada o identificable. Esto incluye, entre otros, nombre, apellidos, correo electrónico, identificadores en línea, datos de localización, información profesional o cualquier otro dato que permita identificar directa o indirectamente a una persona. Ejemplos: - Una herramienta de control horario tratará datos de empleados. - Un CRM tratará datos de clientes, contactos profesionales o potenciales clientes. Ejemplo de respuesta: La herramienta tratará nombre, apellidos y correo electrónico corporativo de empleados.

**Datos confidenciales tratados por la herramienta.** Indicar si la herramienta almacenará, procesará o permitirá acceder a información confidencial de la organización. Ejemplo: La herramienta contendrá información reservada, incluyendo credenciales de acceso a otros sistemas corporativos.

**Prioridad de uso.** Indicar la prioridad de incorporación de la herramienta en una escala del 1 al 5: 1: Baja: Mejora deseable, pero no impide el flujo de trabajo actual. 5: Crítica: La falta de esta herramienta detiene la operación actual.

### 13.2 Campos propuestos para digitalizar la evaluación

Los siguientes campos no modifican el marco, pero permiten que el agente aplique el árbol de decisión, seleccione los criterios y mantenga trazabilidad del expediente.

- **via_activacion:** Determina el origen del expediente y la información mínima aplicable. (Propuesta de modelado para la implementación; derivada de la Etapa 1.)
- **version_o_plan_evaluado:** El informe debe identificar versión o plan evaluado; los criterios dependen de la versión concreta. (Propuesta de modelado para la implementación; derivada de los apartados 6.2.3 y 6.2.5.)
- **familia_principal_y_modulos_adicionales:** Permite seleccionar la tabla principal y añadir S5 o S6 cuando proceda. (Propuesta de modelado para la implementación; derivada de la taxonomía y Anexo D.)
- **respuestas_al_arbol_de_alcance:** Registra las respuestas que determinan la necesidad de validación. (Propuesta de modelado para la implementación; derivada de la Figura 4.)
- **respuestas_al_arbol_de_criticidad:** Sustenta el nivel Crítico, Importante o Bajo. (Propuesta de modelado para la implementación; derivada de la Figura 5.)
- **evidencias:** Cada criterio debe conservar fuente, URL o documento, fecha de consulta, localizador y resultado de la verificación. (Propuesta de modelado para la implementación; derivada de los requisitos de suficiencia documental e informe de validación.)

### 13.3 Estructura mínima de resultado por criterio

Para cada CT o CS, conservar: identificador; estado (`cumple`, `parcial`, `no-cumple`, `no-verif` o `no-aplica`); peso efectivo; evidencias; brecha o riesgo; acción organizativa; responsable y fecha objetivo, cuando proceda.

### Reglas de asignación de estado (lógica del EV-TRACE-MD Assistant v0.7)

- **cumple**: el proveedor cumple el criterio según la evidencia pública encontrada. Que existan medidas organizativas adicionales como capa extra de control NO convierte un cumplimiento del proveedor en parcial. Si el proveedor ofrece la funcionalidad, la documenta o la garantiza, el estado es «cumple» aunque la organización deba además activarla o configurarla.
- **parcial**: existen dudas reales sobre el cumplimiento del proveedor — la evidencia encontrada es ambigua, incompleta o sugiere que el proveedor solo cubre parte del criterio, pero existe una medida organizativa razonable que puede mitigar esa desviación específica.
- **no-cumple**: evidencia pública clara de una carencia grave del proveedor que NO puede compensarse con medidas organizativas. Reservado para incumplimientos severos sin mitigación posible.
- **no-verif**: no se encontró evidencia pública suficiente, el criterio es organizativo o requiere información no pública.
- **no-aplica**: el criterio no es relevante para la herramienta evaluada en el contexto de uso declarado.

**Ejemplos orientativos:**
- El proveedor ofrece MFA pero la organización debe activarlo → **cumple** (el proveedor lo ofrece).
- El proveedor menciona cifrado pero no especifica el estándar → **parcial** (duda del proveedor).
- El proveedor no ofrece exportación de datos en ningún formato → **no-cumple** (carencia grave).
- No se encuentra información sobre el tema → **no-verif**.

## 14. Anclajes normativos y regulatorios

Los siguientes elementos son las referencias de apoyo seleccionadas en el marco. El assistant debe tratarlas como anclajes de contexto y no como sustituto de la consulta de la fuente primaria vigente.

| Sección | Referencia | Título abreviado | Ámbito | Tipo | Relevancia para la validación de terceros |
|---|---|---|---|---|---|
| SaMD | MDR | Reglamento (UE) 2017/745 sobre productos sanitarios | EU | Reg | Marco central. Obliga al fabricante a mantener control sobre procesos externalizados, componentes integrados y servicios de terceros que puedan afectar a la seguridad o conformidad del producto. |
| SaMD | RD 192/2023 | Real Decreto 192/2023 sobre productos sanitarios | ES | RD | Desarrollo español del MDR. Articula las obligaciones de los operadores económicos y refuerza el control sobre actores externos en comercialización y seguimiento. |
| SaMD | Reg. Ejec. (UE) 2021/2226 | Reglamento de Ejecución sobre instrucciones electrónicas de uso | EU | RegEj | Relevancia acotada: aplica cuando se emplean herramientas externas para suministrar instrucciones electrónicas de uso, debiendo garantizarse accesibilidad, seguridad e integridad. |
| SaMD | RGPD | Reglamento (UE) 2016/679. Reglamento general de protección de datos | EU | Reg | Aplicable cuando herramientas de terceros tratan datos personales. Obliga a delimitar responsabilidades, regular contractualmente al proveedor y evitar riesgos para los derechos de los interesados. |
| SaMD | LOPDGDD | LO 3/2018 de Protección de Datos Personales y garantía de derechos digitales | ES | LO | Complemento español del RGPD. Refuerza las obligaciones sobre terceros que accedan o traten información personal en nombre de la organización. |
| SaMD | ISO 13485 | EN ISO 13485:2016. SGC para dispositivos médicos | INT | Norm | Obliga a integrar en el SGC los procesos, herramientas y servicios externos que puedan influir en la conformidad del producto sanitario. |
| SaMD | ISO 14971 | EN ISO 14971:2019. Gestión de riesgos para dispositivos médicos | INT | Norm | Base para evaluar cómo una herramienta o librería de terceros puede introducir peligros o modificar el perfil de riesgo del producto. |
| SaMD | IEC 62304 | IEC 62304:2006. Procesos del ciclo de vida del software sanitario | INT | Norm | Todo elemento software externo que forme parte del producto o de su desarrollo debe quedar sujeto a control, trazabilidad y verificación. |
| SaMD | ISO 15223-1 | EN ISO 15223-1:2021. Símbolos para etiquetado de productos sanitarios | INT | Norm | Relevancia residual: incide únicamente cuando una herramienta externa interviene en la generación o presentación de información de etiquetado. |
| SaMD | ISO 20417 | UNE-EN ISO 20417:2021. Información a suministrar por el fabricante | INT | Norm | Relevancia indirecta: puede ser de apoyo cuando terceros intervienen en la gestión o distribución de la información del producto. |
| SaMD | EN 82304-1 | UNE-EN 82304-1:2017. Software sanitario. Requisitos generales de seguridad | INT | Norm | Permite fundamentar la comprobación de requisitos de seguridad, fiabilidad y ciclo de vida de software sanitario suministrado o mantenido por un tercero. |
| SaMD | ISO/TR 80002-2 | ISO/TR 80002-2. Validación de software en SGC de dispositivos médicos | INT | IT | Referencia especialmente valiosa: establece que la validación debe extenderse al software usado en diseño, pruebas, aceptación, fabricación, etiquetado y gestión de reclamaciones dentro del SGC. |
| SaMD | EN 62366-1 | UNE-EN 62366-1:2015/A1:2020. Ingeniería de usabilidad en productos sanitarios | INT | Norm | Relevancia indirecta: aplica cuando una herramienta de terceros afecta a la usabilidad del sistema y, por tanto, a su uso seguro. |
| Guías | MDCG 2020-1 | Guidance on clinical evaluation of medical device software | EU | Guía | Útil cuando módulos o evidencias clínicas proceden de elementos externos que deben considerarse en la evaluación global del producto. |
| Guías | MDCG 2020-13 | Clinical evaluation assessment report template | EU | Guía | Relevancia limitada: puede servir de apoyo cuando la evidencia clínica depende parcialmente de componentes o desarrollos externos. |
| Guías | MDCG 2019-16 rev.1 | Guidance on cybersecurity for medical devices | EU | Guía | Una de las referencias más relevantes: aborda expresamente el impacto de componentes hardware y software de terceros desde la perspectiva de la ciberseguridad del producto. |
| Guías | MDCG 2018-5 | UDI Assignment to Medical Device Software | EU | Guía | Relevancia marginal: solo aplica cuando terceros intervienen en la configuración o distribución del software identificado por UDI. |
| Guías | MDCG 2019-11 rev.1 | Qualification and classification of software (MDR/IVDR) | EU | Guía | Clave para determinar cuándo una solución de terceros debe considerarse producto sanitario o accesorio, condicionando el nivel de control y validación exigible. |
| Guías | MDCG 2020-7 | Guidance on PMCF plan template | EU | Guía | Relevancia secundaria: aplica cuando el plan de seguimiento poscomercialización debe contemplar incidencias asociadas a componentes externos. |
| Guías | MDCG 2020-8 | Guidance on PMCF evaluation report template | EU | Guía | Relevancia secundaria: puede incluir efectos de herramientas de terceros si repercuten en el comportamiento del producto en condiciones reales. |
| Guías | MDCG 2022-21 | Guidance on Periodic Safety Update Report (PSUR) | EU | Guía | Relevancia indirecta: el PSUR puede verse afectado por incidentes o vulnerabilidades derivados de componentes externos. |
| Guías | MDCG 2023-3 rev.2 | Q&A on vigilance terms and concepts (MDR/IVDR) | EU | Guía | Contribuye a interpretar adecuadamente incidentes en los que intervengan herramientas o servicios de terceros. |
| Guías | IMDRF/SaMD WG/N41FINAL:2017 | SaMD: Clinical Evaluation | INT | DT | El origen externo de un componente software no exime de demostrar su seguridad, rendimiento y adecuación clínica. |
| GRC | ENS | RD 311/2022. Esquema Nacional de Seguridad | ES | RD | Exige considerar la cadena de suministro en el análisis de riesgos y extender las cautelas a contratistas y prestadores de servicios tecnológicos. |
| GRC | ISO/IEC 27001 | ISO/IEC 27001:2022. SGSI — Requisitos | INT | Norm | Marco de gestión para identificar, evaluar y tratar los riesgos asociados a proveedores, servicios externalizados y herramientas que afecten a la seguridad de la información. |
| GRC | ISO/IEC 27002 | ISO/IEC 27002:2022. Controles de seguridad de la información | INT | Norm | Controles operativos directamente aplicables a la gestión de proveedores, cadena de suministro TIC y servicios cloud (controles 5.19–5.23 y 8.26). |
| GRC | ISO/IEC 27701 | ISO/IEC 27701:2025. Sistema de gestión de la información de privacidad | INT | Norm | Aplicable cuando terceros o subencargados intervienen en operaciones sobre datos personales; obliga a documentar responsabilidades y garantías. |
| GRC | ISO/IEC 42001 | ISO/IEC 42001:2023. Sistema de gestión de IA | INT | Norm | Pertinente cuando se valoran herramientas de IA de terceros: exige gobernanza, gestión del riesgo y control organizativo sobre sistemas de IA externos. |
| GRC | NIST AI RMF 1.0 | AI Risk Management Framework (AI RMF 1.0) | EE. UU. | Marco | Referencia técnica de uso voluntario. Aporta estructura para evaluar riesgos de IA de terceros en aspectos como opacidad, sesgos, dependencia y cadena de suministro del modelo. |
| GRC | AI Act | Reglamento (UE) 2024/1689. Normas armonizadas en materia de IA | EU | Reg | Establece obligaciones específicas para operadores que incorporen sistemas de IA de terceros: análisis de finalidad, nivel de riesgo, documentación y responsabilidades del proveedor. |
| GRC | eIDAS | Reglamento (UE) 910/2014 sobre identificación electrónica y servicios de confianza | EU | Reg | Relevante cuando se externalizan servicios de firma electrónica, sellado temporal o certificación: la dependencia de terceros queda sujeta a requisitos regulatorios de confianza reforzada. |
| GRC | Ley 6/2020 | Ley 6/2020 de servicios electrónicos de confianza | ES | Ley | Complementa eIDAS en España. Aplica cuando se externalizan servicios electrónicos de confianza a proveedores críticos. |
| GRC | RDL 12/2018 | RDL 12/2018 de seguridad de redes y sistemas de información | ES | RDL | Extiende obligaciones de seguridad a proveedores de servicios digitales; la externalización no elimina la responsabilidad de la entidad sobre la seguridad de sus sistemas. |
| GRC | RD 43/2021 | RD 43/2021. Desarrollo del RDL 12/2018 | ES | RD | Consolida la supervisión sobre operadores y proveedores digitales; refuerza que la externalización no traslada la responsabilidad de seguridad. |
| GRC | NIS 2 | Directiva (UE) 2022/2555 sobre ciberseguridad (NIS2) | EU | Dir | Incorpora expresamente los riesgos de la cadena de suministro y de las relaciones con suministradores dentro de las medidas de gestión de ciberseguridad exigidas. |
| GRC | CRA | Reglamento (UE) 2024/2847. Requisitos de ciberseguridad para productos con elementos digitales | EU | Reg | Refuerza el control sobre componentes digitales externos y dependencias a lo largo del ciclo de vida. Aunque excluye los productos cubiertos por el MDR, contextualiza la evolución regulatoria en curso. |
| PI | Ley de Marcas | Ley 17/2001, de 7 de diciembre, de Marcas | ES | Ley | Relevancia indirecta: aplica cuando el uso de una solución externa implica explotación de signos distintivos o riesgos de confusión comercial. |
| PI | LPI | RDL 1/1996. Texto Refundido de la Ley de Propiedad Intelectual | ES | RDL | El uso de software, librerías o contenidos de terceros obliga a considerar aspectos de licenciamiento, titularidad y límites de uso que condicionan la viabilidad de determinadas herramientas externas. |
| Pagos | PCI DSS | Payment Card Industry Data Security Standard (PCI DSS) | INT | Norm | Aplicable cuando intervienen proveedores de pago o terceros que almacenen, procesen o transmitan datos de tarjeta; permite documentar la responsabilidad compartida en entornos de pago. |

## 15. Trazabilidad de los archivos del paquete

- `01_base_conocimiento_agente.md`: corpus legible del marco EV-TRACE-MD, utilizable como documento de conocimiento del assistant.
- `02_catalogo_estructurado_marco_validacion.json`: datos normalizados del catálogo de 77 criterios para el EV-TRACE-MD Assistant.
- `03_informe_validacion_herramienta.md`: plantilla del informe de validación generado por el assistant.
- `03_catalogo_editable_marco_validacion.xlsx`: versión editable de las tablas maestras para revisión y mantenimiento.
- `ev_trace_md_v07.html`: aplicación standalone del EV-TRACE-MD Assistant (v0.7).

## 16. Salvaguardas de calidad

- No añadir criterios, umbrales o referencias normativas sin revisar antes la fuente de origen.
- Diferenciar los campos extraídos del TFM de los campos propuestos solo para digitalizar el flujo.
- Exigir siempre evidencia concreta y conservar la fecha de consulta.
- Mantener la aprobación, el rechazo y la aceptación condicionada bajo supervisión humana.
- Actualizar el catálogo cuando cambie la metodología, el marco regulatorio o las condiciones relevantes de una herramienta.