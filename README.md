# EV-TRACE-MD

**Evidence-based Validation of Third-party Tools: Risk Assessment, Classification and Evaluation for Medical Devices**

> Artefacto del Trabajo de Fin de Máster en Ciberseguridad · UNIR, 2026  
> Autora: María del Pilar Jalón Pascual  
> Estado: prototipo de investigación · v0.9

---

## Descripción

EV-TRACE-MD es un marco metodológico y una herramienta de asistencia para la
evaluación, aprobación y seguimiento de herramientas software de terceros empleadas
en el ciclo de vida de un producto sanitario basado en software (SaMD). Está diseñado
para fabricantes de SaMD que operan bajo el Reglamento (UE) 2017/745 (MDR),
ISO 13485, IEC 62304, el Reglamento General de Protección de Datos (RGPD) y el
Reglamento (UE) 2024/1689 (EU AI Act).

El marco cubre seis familias de herramientas (dependencias, repositorios/CI-CD,
software instalable, SaaS, herramientas de IA y complementos), ocho dimensiones de
amenaza (A1–A8) y 77 criterios de evaluación estructurados en cuatro etapas:
detección, clasificación, evaluación y decisión/registro/seguimiento.

---

## Contenido del repositorio

| Fichero | Descripción |
|---|---|
| `knowledge-base/01_base_conocimiento_agente.md` | Base de conocimiento del agente de evaluación |
| `knowledge-base/02_catalogo_marco_validacion.json` | Catálogo de criterios estructurado para el agente |
| `knowledge-base/03_plantilla_informe_validacion.md` | Plantilla de informe de validación |
| `docs/ev_trace_md.html` | Herramienta interactiva (EV-TRACE-MD Assistant v0.9) |
| `docs/index.html` | Página index de visualización del asistente y acceso a los informes de ejemplo |
| `docs/report_ev_trace_md_v08_claude_code_reduce.html` | Informe de ejemplo simple — Caso A: Claude Code |
| `docs/report_ev_trace_md_v08_claude_code.html` | Informe de ejemplo completo — Caso A: Claude Code |
| `docs/report_ev_trace_md_v07_cheerio_reduce.html` | Informe de ejemplo simple — Caso B: Cheerio |
| `docs/report_ev_trace_md_v07_cheerio.html` | Informe de ejemplo completo — Caso B: Cheerio |

---

## Características principales

- Evaluación en modo manual o autónomo: en modo autónomo, el agente realiza llamadas a la API de Anthropic para consultar fuentes públicas del proveedor (políticas de privacidad, ToS, NVD/CVE, trust centers) y proponer un resultado por criterio.
- Clasificación de herramientas en seis familias (S1–S6) con activación transversal de S5 (IA).
- Sistema de pesos con triple nivel (Bloqueante · Condicionante · Observación)
  modulado por criticidad del proceso (Crítico · Importante · Bajo).
- Tres vías de exportación del informe: JSON, Markdown y HTML.
- La herramienta **nunca emite una resolución definitiva**: propone un estado documental
  que requiere revisión y aprobación humana obligatoria.

---

## Requisitos

La herramienta es un fichero HTML autocontenido y no requiere instalación. Puede usarse en dos modos:

- **Modo manual:** sin clave de API. El evaluador completa el expediente, clasifica la herramienta y registra los resultados de cada criterio de forma manual. No se realizan búsquedas automáticas.
- **Modo autónomo:** requiere una **clave de API de Anthropic** (`claude-sonnet-4-6`), que el evaluador introduce en la propia interfaz. El agente consulta fuentes públicas del proveedor y propone un resultado para cada criterio. No se almacena ninguna clave en el repositorio.

---

## Uso

1. Acceder a la demo en línea desde `docs/index.html` o descargar `docs/ev_trace_md.html` y abrirlo en un navegador moderno (Chrome, Firefox, Edge).
2. Seguir el flujo guiado del asistente: presentación inicial y pasos 1 a 6
   (solicitud → alcance → clasificación → evaluación → hallazgos → informe).
3. En el **paso 4 — Evaluación**, el asistente puede usarse en modo manual registrando los hallazgos criterio a criterio, o en modo autónomo si se dispone de una clave de API de Anthropic.

> **Aviso legal:** este prototipo es un artefacto de investigación académica.
> No constituye un sistema de gestión de calidad certificado ni sustituye
> los procedimientos internos del fabricante ni la responsabilidad del
> responsable de cumplimiento.

---

## Ejemplos de informes generados

La carpeta `docs/` contiene dos informes de validación generados con el asistente
EV-TRACE-MD sobre herramientas reales, correspondientes a los casos de uso del TFM.
Pueden consultarse directamente sin necesidad de cuenta Claude ni clave de API.

> **Aviso:** estos informes son artefactos ilustrativos generados por inteligencia
> artificial (Claude, Anthropic) con fines académicos y de demostración. No
> constituyen una evaluación de conformidad regulatoria real. Las marcas
> mencionadas (Claude Code, Anthropic, cheerio, undici) se usan de forma
> nominativa sin implicar aval de sus titulares.

---

## Demostración

El siguiente vídeo muestra la ejecución completa del EV-TRACE-MD Assistant
sobre un caso de uso, desde el inicio de la sesión
hasta la generación del informe de validación.

> La ejecución requiere cuenta activa de Claude y consumo de tokens de la API
> de Anthropic. El vídeo permite verificar el flujo sin necesidad de acceso
> a la herramienta.

[![YouTube](https://img.shields.io/badge/-YouTube-red?style=flat&logo=youtube&logoColor=white&label=)](https://youtu.be/N5mYKOyIB5E)

---

## Licencia

Este repositorio se publica bajo licencia **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)**.

Puedes consultar el texto completo en https://creativecommons.org/licenses/by-nc-nd/4.0/legalcode.

![License: CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)

Los informes de ejemplo en `docs/` mencionan productos y marcas de terceros
(Anthropic, npm) sujetos a sus propias condiciones; dichas marcas no quedan cubiertas
por esta licencia.

---

## Cita académica

Si referencias este trabajo, utiliza la siguiente cita (APA 7):

> Jalón, M. P. (2026). *EV-TRACE-MD: Marco de validación de herramientas software para el
> desarrollo de productos sanitarios*
> [Trabajo de Fin de Máster]. Universidad Internacional de La Rioja.
> https://github.com/majalon/ev-trace-md

---

## Contacto

https://www.linkedin.com/in/pilar-jal%C3%B3n-pascual-47689440/
