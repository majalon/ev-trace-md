# EV-TRACE-MD

**Evaluation & Traceability Framework for Regulated Compliant Environments — Medical Devices**

> Artefacto del Trabajo de Fin de Máster en Ciberseguridad · UNIR, 2026  
> Autora: María del Pilar Jalón Pascual
> Estado: prototipo de investigación · v0.7

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
| `ev_trace_md_v06.html` | Herramienta interactiva (EV-TRACE-MD Assistant v0.7) |
| `02_catalogo_marco_validacion.json` | Catálogo de criterios estructurado para el agente |
| `01_base_conocimiento_agente.md` | Base de conocimiento del agente de evaluación |

---

## Características principales

- Evaluación autónoma por criterio mediante llamadas individuales a la API de Anthropic,
  con lectura de fuentes públicas (políticas de privacidad, ToS, NVD/CVE, trust centers).
- Clasificación de herramientas en seis familias (S1–S6) con activación transversal de S5 (IA).
- Sistema de pesos con triple nivel (Bloqueante · Condicionante · Observación) 
  modulado por criticidad del proceso (Crítico · Importante · Bajo).
- Tres vías de exportación del informe: JSON, Markdown y HTML.
- La herramienta **nunca emite una resolución definitiva**: propone un estado documental 
  que requiere revisión y aprobación humana obligatoria.

---

## Requisitos

La herramienta es un fichero HTML autocontenido. Para su funcionamiento completo 
se requiere una **clave de API de Anthropic** (`claude-sonnet-4-6`), que el evaluador 
introduce en la propia interfaz. No se almacena ninguna clave en el repositorio.

---

## Uso

1. Descargar `ev_trace_md_v07.html`.
2. Abrirlo en un navegador moderno (Chrome, Firefox, Edge).
3. Introducir la clave de API de Anthropic cuando se solicite.
4. Seguir las etapas E1–E4 del asistente.

> **Aviso legal:** este prototipo es un artefacto de investigación académica. 
> No constituye un sistema de gestión de calidad certificado ni sustituye 
> los procedimientos internos del fabricante ni la responsabilidad del 
> responsable de cumplimiento.

---

## Licencia

Este repositorio se publica bajo licencia **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)**.

Puedes consultar el texto completo en https://creativecommons.org/licenses/by-nc-nd/4.0/legalcode.
![License: CC BY-NC-ND 4.0](https://licensebuttons.net/l/by-nc-nd/4.0/88x31.png)

**Está expresamente prohibido** el uso comercial, la redistribución de versiones 
modificadas y la integración en productos o servicios sin autorización escrita de la autora.

---

## Cita académica

Si referencias este trabajo, utiliza la siguiente cita (APA 7):

> Jalón, María P. (2026). *EV-TRACE-MD: Marco de evaluación y 
> trazabilidad de herramientas software de terceros para entornos regulados de 
> dispositivos médicos* [Trabajo de Fin de Máster]. Universidad Internacional 
> de La Rioja. https://github.com/majalon/ev-trace-md

---

## Contacto

https://www.linkedin.com/in/pilar-jal%C3%B3n-pascual-47689440/
