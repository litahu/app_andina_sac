# 🚀 Salesforce Sales Cloud Implementation: Comercializadora Andina SAC

## 📌 Sobre el Proyecto
El proyecto resuelve la transición de una gestión comercial basada en Excel y correos electrónicos hacia un CRM centralizado. El objetivo es optimizar el seguimiento de prospectos, estructurar el pipeline de ventas y proveer analítica confiable para la toma de decisiones gerenciales.

Este repositorio contiene la documentación, el diseño de solución y la estrategia de configuración para la implementación de **Salesforce Sales Cloud** para **Comercializadora Andina SAC** (Distribución de Equipos Industriales). 

---

## 💼 Rol Desempeñado
*   **Salesforce Administrator & Business Analyst**
    *   Levantamiento y análisis de requerimientos del caso de negocio.
    *   Modelado de datos y mapeo del ciclo de vida del cliente (Lead-to-Cash).
    *   Configuración declarativa de la plataforma (Campos, Etapas, Reglas de Validación).
    *   Automatización de procesos mediante **Salesforce Flow**.
    *   Diseño de reportes y dashboards ejecutivos.

---

## 📑 Parte A: Análisis e Ingeniería de Requerimientos

### 1. Modelo de Datos: Objetos Estándar Utilizados
Para mantener las mejores prácticas de la plataforma y garantizar la escalabilidad, se seleccionaron los siguientes objetos estándar:
*   **Candidato (Lead):** Para calificar el interés inicial de Empresas o Personas antes de la segmentación.
*   **Cuenta (Account):** Centraliza la información de las empresas clientes (B2B) o clientes individuales (B2C).
*   **Contacto (Contact):** Almacena los datos de las personas clave y tomadores de decisión en las empresas.
*   **Oportunidad (Opportunity):** Gestiona el flujo financiero, las etapas de venta y la previsión del pipeline de ingresos.

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_formularios.PNG" width=600px> </kbd> <br>
</p> 
<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_app.PNG" width=600px> </kbd> <br>
</p> 


### 2. Flujo de Conversión: Lead ➔ Oportunidad
El proceso de negocio se estructuró bajo un embudo de conversión estricto:
1.  **Captura y Calificación:** El Lead se registra con datos clave (*Tipo de Cliente, Industria, Monto Potencial, Origen*).
2.  **Criterio de Conversión:** Si el Lead cumple con el perfil de cliente ideal y muestra intención de compra, se ejecuta la conversión nativa de Salesforce.
3.  **Resultado:** El sistema genera de forma automática una **Cuenta**, un **Contacto** y una **Oportunidad** en la etapa inicial, manteniendo la trazabilidad histórica.

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_data_0.PNG" width=600px> </kbd> 
   <br>
</p> 

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_pipeline2.PNG" width=600px> </kbd>
   <br>
</p> 

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_pipeline_1.PNG" width=600px> </kbd>
   <br>
</p> 


### 3. Necesidades Analíticas de la Gerencia Comercial
Para solucionar la falta de visibilidad del equipo de 8 vendedores y 1 gerente, se identificaron los siguientes KPIs críticos:
*   **Salud del Pipeline:** Valor monetario total y volumen de oportunidades distribuidas por etapa.
*   **Rendimiento del Equipo:** Ventas cerradas y oportunidades activas asignadas por ejecutivo.
*   **Eficiencia de Conversión:** Tasa de éxito de Leads transformados en oportunidades reales.
*   **Análisis de Pérdidas:** Motivos recurrentes de descarte para ajustar la estrategia comercial.

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_dash.PNG" width=600px> </kbd> <br>
</p> 


---

## 🛠️ Parte B: Configuración y Automatización

Se implementaron las siguientes configuraciones dentro de una organización *Developer Sandbox*:

### ⚙️ Arquitectura de Campos y Etapas
*   **Campos Personalizados en Leads:** Creación de listas de selección para *Tipo de Cliente* (Empresa/Persona), *Industria*, *Monto Potencial* y *Origen del Lead*.
*   **Etapas Personalizadas de Oportunidad:** Mapeo exacto del pipeline del cliente:
    `Prospecto Calificado` ➔ `Reunión Comercial` ➔ `Propuesta Enviada` ➔ `Negociación` ➔ `Ganada` / `Perdida`.

### ⚡ Automatización con Record-Triggered Flow
*   **Proceso:** Al momento de la creación de una Oportunidad, un flujo en segundo plano genera automáticamente una **Tarea de Seguimiento** asignada al propietario del registro, con una fecha de vencimiento configurada exactamente a **7 días calendario** a partir de la fecha de creación (`$Record.CreatedDate + 7`).

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_flujo.PNG" width=600px> </kbd> <br>
</p> 

---
### 🛠️ Tecnologías Utilizadas
*   Salesforce Sales Cloud (Lightning Experience)
*   Lightning Flow Builder
*   Data Import Wizard
*   Salesforce Reports & Dashboards


