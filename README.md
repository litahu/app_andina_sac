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
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_app.PNG" width=600px> </kbd> <br>
  Figura 1. Diagrama de relación de entidades
</p> 


### 2. Flujo de Conversión: Lead ➔ Oportunidad
El proceso de negocio se estructuró bajo un embudo de conversión estricto:
1.  **Captura y Calificación:** El Lead se registra con datos clave (*Tipo de Cliente, Industria, Monto Potencial, Origen*).
2.  **Criterio de Conversión:** Si el Lead cumple con el perfil de cliente ideal y muestra intención de compra, se ejecuta la conversión nativa de Salesforce.
3.  **Resultado:** El sistema genera de forma automática una **Cuenta**, un **Contacto** y una **Oportunidad** en la etapa inicial, manteniendo la trazabilidad histórica.

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_data_0.PNG" width=600px> </kbd> <br>
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_pipeline2.PNG" width=600px> </kbd> <br>
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_pipeline_1.PNG" width=600px> </kbd> <br>
  Figura 1. Diagrama de relación de entidades
</p> 


### 3. Necesidades Analíticas de la Gerencia Comercial
Para solucionar la falta de visibilidad del equipo de 8 vendedores y 1 gerente, se identificaron los siguientes KPIs críticos:
*   **Salud del Pipeline:** Valor monetario total y volumen de oportunidades distribuidas por etapa.
*   **Rendimiento del Equipo:** Ventas cerradas y oportunidades activas asignadas por ejecutivo.
*   **Eficiencia de Conversión:** Tasa de éxito de Leads transformados en oportunidades reales.
*   **Análisis de Pérdidas:** Motivos recurrentes de descarte para ajustar la estrategia comercial.

<p align= "center">
  <kbd><img src="https://github.com/litahu/app_andina_sac/blob/main/resource_andina_sac/resource_dash.PNG" width=600px> </kbd> <br>
  Figura 1. Diagrama de relación de entidades
</p> 


---

## 🛠️ Parte B: Configuración y Automatización

Se implementaron las siguientes configuraciones dentro de una organización *Developer Sandbox*:

### ⚙️ Arquitectura de Campos y Etapas
*   **Campos Personalizados en Leads:** Creación de listas de selección para *Tipo de Cliente* (Empresa/Persona), *Industria*, *Monto Potencial* y *Origen del Lead*.
*   **Etapas Personalizadas de Oportunidad:** Mapeo exacto del pipeline del cliente:
    `Prospecto Calificado` ➔ `Reunión Comercial` ➔ `Propuesta Enviada` ➔ `Negociación` ➔ `Ganada` / `Perdida`.

### 🛡️ Reglas de Validación (Data Quality)
*   **Obligatoriedad de Motivo de Pérdida:** Restringe el cambio de etapa a 'Perdida' si el usuario no documenta la causa en el campo personalizado `Motivo_Perdida__c`.
    ```excel
    AND(
        ISPICKVAL(StageName, "Perdida"),
        ISBLANK(TEXT(Motivo_Perdida__c))
    )
    ```
*   **Control de Actividad en Propuestas:** Validación avanzada mediante lógica de negocio para asegurar que no se avance a 'Propuesta Enviada' sin al menos una llamada, tarea o evento registrado en el historial de la oportunidad.

### ⚡ Automatización con Record-Triggered Flow
*   **Proceso:** Al momento de la creación de una Oportunidad, un flujo en segundo plano genera automáticamente una **Tarea de Seguimiento** asignada al propietario del registro, con una fecha de vencimiento configurada exactamente a **7 días calendario** a partir de la fecha de creación (`$Record.CreatedDate + 7`).

### 📊 Gestión de Datos (Data Loading)
*   Carga de registros de prueba utilizando **Data Import Wizard** para validar la integridad de las relaciones:
    *   5 Leads calificados.
    *   3 Cuentas corporativas correlacionadas con 3 Contactos clave.
    *   5 Oportunidades activas en distintas etapas del pipeline.

---

## 📊 Parte C: Inteligencia de Negocios y Reportes

Se construyó un **Dashboard Ejecutivo** enfocado en el control de operaciones, integrado por los siguientes componentes visuales:
1.  **Reporte de Pipeline por Etapa:** Gráfico de embudo (Funnel) que muestra el valor monetario acumulado en cada fase para identificar cuellos de botella.
2.  **Reporte de Oportunidades por Vendedor:** Gráfico de barras horizontales que compara la carga de trabajo y el pronóstico de cierre de los 8 ejecutivos comerciales.
3.  **Métricas de Control de Datos:** Indicadores tipo *Gauge* para medir la cantidad de leads sin gestionar y motivos de pérdida más comunes.

---

## 🧠 Enfoque Consultivo y Criterio de Negocio (QA)

### 1. Visibilidad Total de Datos vs. Seguridad
*Si un cliente solicita que todos los usuarios vean toda la información del CRM, antes de configurar consultaría:*
*   ¿Existen datos sensibles o normativas de privacidad (como GDPR o leyes locales de protección de datos) que debamos cumplir?
*   ¿Tienen competidores internos o equipos que manejen cuentas clave/VIP que requieran confidencialidad?
*   ¿Cuál es la estrategia de crecimiento? Abrir la visibilidad total facilita la colaboración, pero puede generar problemas de propiedad de registros si el equipo crece. Propondría un modelo híbrido basado en **Roles y jerarquías**.

### 2. Gestión de Automatizaciones Complejas con Procesos Difusos
*Si se solicita una automatización compleja con un proceso de negocio mal definido, mi plan de acción es:*
1.  **Frenar la configuración:** No automatizar el caos. Un mal proceso automatizado solo genera errores más rápido.
2.  **Sesión de Discovery (Design Thinking):** Reunirme con los líderes del proceso para diagramar el flujo actual ("As-Is") y co-diseñar el flujo deseado ("To-Be").
3.  **MVP (Producto Mínimo Viable):** Implementar la automatización por fases, validando la lógica básica de forma manual antes de construir código o flows complejos.

### 3. Gestión de Mejoras Proactivas (Out of Scope)
*Al detectar una mejora no solicitada por el cliente:*
*   **Documentación:** Registro la idea en una bitácora de mejoras con su respectivo impacto estimado (ROI, ahorro de tiempo o mitigación de errores).
*   **Comunicación:** Durante las reuniones de estatus, presento la propuesta no como una crítica, sino como un valor agregado para una **Fase 2**.
*   **Priorización:** Evito el "Scope Creep" (corrupción del alcance). Primero aseguro el éxito de los entregables firmados en la Fase 1 antes de consumir recursos en nuevas funciones.

---

## 🔮 RoadMap: Propuesta de Escalabilidad (Fase 2)
Para maximizar el retorno de inversión (ROI) de Comercializadora Andina SAC, se recomiendan las siguientes mejoras tecnológicas en el corto plazo:
*   **Salesforce Inbox / Einstein Activity Capture:** Para sincronizar automáticamente los correos de Outlook/Gmail y calendarios de los vendedores sin registro manual.
*   **Lead Scoring básico:** Implementar reglas de puntuación para priorizar la atención de leads industriales de alto valor.
*   **Web-to-Lead:** Automatizar la captura de prospectos desde la página web oficial de la distribuidora directamente hacia Salesforce.

---
### 🛠️ Tecnologías Utilizadas
*   Salesforce Sales Cloud (Lightning Experience)
*   Lightning Flow Builder
*   Data Import Wizard
*   Salesforce Reports & Dashboards


