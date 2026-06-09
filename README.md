# Agente de Prospección B2B con IA 

## Descripción General

Este proyecto implementa un agente de prospección B2B automatizado en Make que utiliza múltiples modelos de Inteligencia Artificial para investigar empresas, identificar prospectos relevantes, evaluar su potencial comercial y generar una priorización automática de contactos.

El flujo está diseñado para apoyar equipos de ventas, SDRs y Business Development Representatives en la identificación de oportunidades comerciales alineadas con las soluciones de Hibrids.

---

# Arquitectura

```text
Webhook
   │
   ▼
Research Assistant (Gemini)
   │
   ▼
Scraping Assistant (Gemini)
   │
   ▼
Reasoning Assistant (Claude)
   │
   ▼
Scoring Assistant (Claude)
```

Cada agente tiene una responsabilidad específica dentro del proceso de prospección.

---

# Flujo de Ejecución

## 1. Webhook de Entrada

El escenario inicia mediante un Custom Webhook de Make que recibe información básica sobre la empresa objetivo.

### Parámetros de entrada

| Campo     | Descripción                      |
| --------- | -------------------------------- |
| empresa   | Nombre de la empresa objetivo    |
| rol       | Cargo o perfil buscado           |
| linkedin  | Perfil de referencia en LinkedIn |
| categoria | Categoría de negocio             |
| tematica  | Temática o vertical de interés   |
| pais      | País o región                    |

### Ejemplo

```json
{
  "empresa": "HubSpot",
  "rol": "VP Sales",
  "linkedin": "",
  "categoria": "CRM",
  "tematica": "AI para ventas",
  "pais": "USA"
}
```

---

## 2. Research Assistant

### Modelo

Gemini 2.5 Flash

### Objetivo

Realizar investigación empresarial para generar contexto estratégico sobre la organización.

### Información obtenida

* Revenue estimado
* Inversión recibida
* KPIs relevantes
* Visión estratégica
* Actividad dentro de la categoría
* Competidores principales
* Alineación con soluciones AI + CRM
* Fuentes consultadas

### Salida

```json
{
  "empresa": "",
  "inversion_categoria": "",
  "revenue_estimado": "",
  "kpis_relevantes": [],
  "vision_empresa": "",
  "que_hacen_en_categoria": "",
  "competidores": [],
  "alineacion_con_ai_crm": "",
  "fuentes": []
}
```

---

## 3. Scraping Assistant

### Modelo

Gemini 2.5 Flash

### Objetivo

Identificar perfiles relevantes asociados a la empresa objetivo.

### Información obtenida

* Nombre del prospecto
* Cargo actual
* Antigüedad aproximada
* URL de LinkedIn
* Temas de publicaciones recientes
* Responsabilidades principales
* Señales de interés comercial

### Salida

```json
{
  "perfiles": [
    {
      "nombre": "",
      "rol_actual": "",
      "años_en_empresa": "",
      "linkedin_url": "",
      "publicaciones_recientes": [],
      "responsabilidades_clave": [],
      "señales_de_interes": ""
    }
  ]
}
```

---

## 4. Reasoning Assistant

### Modelo

Claude Sonnet 4.5

### Objetivo

Evaluar la calidad y relevancia de cada prospecto identificado.

### Factores Analizados

* Alineación con la visión de Hibrids
* Compatibilidad estratégica
* Potencial de compra
* Señales positivas
* Riesgos identificados
* Prioridad de contacto

### Salida

```json
{
  "evaluacion": [
    {
      "nombre": "",
      "alineacion_vision": "",
      "justificacion_alineacion": "",
      "señales_positivas": [],
      "señales_negativas": [],
      "recomendacion": ""
    }
  ]
}
```

---

## 5. Scoring Assistant

### Modelo

Claude Sonnet 4.5

### Objetivo

Asignar una puntuación objetiva a cada prospecto.

### Criterios de Evaluación

| Criterio               | Máximo |
| ---------------------- | ------ |
| Visión Hibrids         | 25     |
| Alineación Estratégica | 25     |
| Influencia             | 25     |
| Tamaño y Capacidad     | 25     |

### Escala

| Nivel | Score      |
| ----- | ---------- |
| A     | 80 - 100   |
| B     | 60 - 79    |
| C     | 40 - 59    |
| D     | Menor a 40 |

### Salida

```json
{
  "scoring_final": [
    {
      "nombre": "",
      "linkedin_url": "",
      "rol_actual": "",
      "score_total": 0,
      "nivel": "",
      "accion_recomendada": ""
    }
  ],
  "contacto_principal": "",
  "score_empresa": 0,
  "reporte_resumen": ""
}
```

---

# Tecnologías Utilizadas

* Make
* Google Gemini 2.5 Flash
* Anthropic Claude Sonnet 4.5
* Webhooks HTTP
* JSON como formato de intercambio

---

# Cómo Ejecutar el Escenario

## 1. Importar Blueprint

1. Abrir Make.
2. Crear un nuevo Scenario.
3. Seleccionar "Import Blueprint".
4. Cargar el archivo JSON.

---

## 2. Configurar Conexiones

### Gemini

Crear conexión utilizando:

* API Key de Google AI Studio

Modelos utilizados:

* Gemini 2.5 Flash

### Claude

Crear conexión utilizando:

* API Key de Anthropic

Modelos utilizados:

* Claude Sonnet 4.5

---

## 3. Activar el Webhook

Copiar la URL generada por Make.

Ejemplo:

```text
https://hook.us2.make.com/xxxxxxxx
```

---

## 4. Ejecutar

Enviar una solicitud HTTP POST:

```bash
curl -X POST \
"https://hook.us2.make.com/xxxxxxxx" \
-H "Content-Type: application/json" \
-d '{
  "empresa":"HubSpot",
  "rol":"VP Sales",
  "linkedin":"",
  "categoria":"CRM",
  "tematica":"AI para ventas",
  "pais":"USA"
}'
```

---

# Resultado Esperado

El agente devolverá:

1. Investigación empresarial.
2. Prospectos potenciales.
3. Evaluación estratégica.
4. Ranking de contactos.
5. Recomendaciones de acción comercial.

---

# Mejoras Futuras

## Integraciones

* LinkedIn Sales Navigator
* PhantomBuster


---

# Autor

Juan Manuel velosa y Juan Pablo Ordoñez

Agente de Prospección B2B basado en Inteligencia Artificial para identificación, evaluación y priorización de oportunidades comerciales.
