# Calificación Automatizada de Leads B2B con IA

Este es el proyecto final para la entrega sobre automatizaciones e IA. Armé un flujo de trabajo en n8n que recibe leads B2B, los analiza con Inteligencia Artificial y te notifica por correo antes de tomar cualquier acción comercial.

---

## 🛠️ Tecnologías utilizadas

* **n8n:** Orquestador principal de todo el flujo.
* **Airtable:** Base de datos para guardar los datos del lead y la propuesta generada.
* **OpenAI (`gpt-4o-mini`):** Modelo de IA que analiza el mensaje del lead, le asigna un puntaje y redacta un diagnóstico.
* **Gmail:** Canal de correo para recibir la alerta antes de contactar al cliente.

---

## ⚙️ ¿Cómo funciona el flujo?

El proceso sigue 4 pasos simples:

1. **Nuevo Lead:** El flujo se activa automáticamente cuando se crea o modifica un registro en **Airtable**.
2. **Análisis con IA:** El nodo de **OpenAI** evalúa la información del prospecto, calcula un puntaje (score) y genera una recomendación comercial.
3. **Guardado:** La propuesta generada por la IA se guarda en el campo `Propuesta_IA` dentro de **Airtable**.
4. **Notificación por Mail:** Llega un e-mail a **Gmail** con el resumen del lead para que un ejecutivo comercial revise la propuesta.

---

## 🛡️ Manejo de errores (Resiliencia)

Para evitar que la automatización se frena por completo si la API de OpenAI llega a fallar (por falta de créditos, límite de peticiones o caída del servicio):

* En el nodo de OpenAI configure la opción **`On Error: Continue (using error output)`**.
* **Resultado:** Si la API devuelve un error, el flujo no se detiene abruptamente, sino que captura la falla y permite continuar o gestionar la excepción sin colgar el sistema.

---

## 👤 Control Humano (*Human-in-the-Loop*)

Para garantizar la calidad de la atención y no enviar respuestas automáticas a ciegas al cliente:

* El flujo **no le escribe directamente al lead**.
* En su lugar, envía una notificación por correo al equipo de ventas con el diagnóstico sugerido por la IA.
* Un usuario humano revisa la propuesta en Airtable y da el visto bueno final antes de contactar al prospecto.

---

## 📂 Archivos y Enlaces de la Entrega

| **Blueprint n8n** | https://prnt.sc/2rVDAhlJ_q_M | Archivo `.json` exportado para importar el flujo tal cual en n8n. |

| **Base de Datos** | https://airtable.com/invite/l?inviteId=invCa93E74q7fps9L&inviteToken=396c816d73e65328a4de6d6ffe3471949d44c0da660024708365e8912011a27f&utm_medium=email&utm_source=product_team&utm_content=transactional-alerts | Vista de lectura pública de la tabla de leads. |

| **Video Demo** (https://youtu.be/btTCvRteU-8) | Grabación de pantalla mostrando la ejecución del flujo en tiempo real. |

---

## 🚀 Pasos para probarlo

1. Descargar el archivo `Trabajo final AI Automation.json` de este repositorio.
2. En tu instancia de **n8n**, ir a **Workflows** -> **Import from File** y cargar el archivo.
3. Conectar tus credenciales de Airtable, OpenAI y Gmail.
4. Activar el disparador (*Airtable Trigger*) y probar creando un registro.
