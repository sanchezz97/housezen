# Workflows

Esta carpeta contiene todos los workflows de n8n exportados como archivos JSON.

## Convención de Nombres

Usar nombres descriptivos en minúsculas con guiones:

- `workflow-envio-emails.json`
- `workflow-procesamiento-leads.json`
- `workflow-notificaciones-slack.json`
- `workflow-backup-datos.json`

## Cómo Exportar un Workflow

1. Abre el workflow en n8n
2. Click en el menú (⋮) en la esquina superior derecha
3. Selecciona "Download"
4. Guarda el archivo en esta carpeta
5. Renombra con un nombre descriptivo

## Cómo Importar un Workflow

1. En n8n, ve a la página de Workflows
2. Click en "Import from File"
3. Selecciona el archivo JSON de esta carpeta
4. El workflow se importará con todos sus nodos

---

## Workflows del Proyecto

### Workflow: Clasificación Inteligente de Averías
- **Archivo**: `workflow-clasificacion-averias.json`
- **Estado**: ✅ Activo
- **Descripción**: Sistema automatizado de gestión de averías que clasifica incidencias utilizando IA y determina responsabilidades según la Ley de Arrendamientos Urbanos (LAU)

#### Funcionamiento

1. **Trigger**: Google Sheets (cada minuto)
   - Detecta nuevas filas en el formulario de averías
   - Formulario Google: "HouseZen (respuestas)"

2. **Clasificación Inicial (IA)**:
   - Analiza si la avería requiere seguro o manitas
   - Usa Google Gemini para clasificación inteligente
   - Criterios: responsabilidad civil, daños a terceros, desgaste normal

3. **Flujo según responsabilidad**:

   **Si es SEGURO → Notifica al propietario**
   - Envía email al propietario con detalles de la avería

   **Si es MANITAS → Análisis legal LAU**
   - Segundo análisis IA basado en Ley de Arrendamientos Urbanos
   - Determina si es responsabilidad del INQUILINO o PROPIETARIO
   - Incluye artículo LAU aplicable y justificación legal

   **Si es responsabilidad del INQUILINO:**
   - Envía email con opciones:
     - "Por mi cuenta"
     - "Manitas de HouseZen"
   - Si elige "Manitas de HouseZen" → Envía presupuesto a empresa de reparaciones

   **Si es responsabilidad del PROPIETARIO:**
   - Envía directamente presupuesto a empresa de reparaciones

#### Nodos del Workflow

1. **Google Sheets Trigger** - Detecta nuevas averías
2. **Limit** - Procesa solo la última entrada
3. **AI Agent** - Clasificación Seguro/Manitas
4. **Google Gemini Chat Model** - Modelo de IA
5. **If** - Decisión según clasificación
6. **AI Agent1** - Análisis legal LAU (si es manitas)
7. **Google Gemini Chat Model1** - Segundo modelo de IA
8. **Send a message** - Email a empresa reparaciones
9. **Send a message1** - Email a propietario (si es seguro)
10. **Send a message2** - Email a inquilino (si es su responsabilidad)
11. **If1** - Decisión según respuesta del inquilino

#### Dependencias

- **Google Sheets API**: Lectura de formulario de averías
- **Google Gemini API**: Inteligencia artificial para clasificación
- **Gmail API**: Envío de notificaciones por email

#### Credenciales Necesarias

Para usar este workflow necesitas configurar las siguientes credenciales en n8n:

1. **Google Sheets Trigger OAuth2**
   - Tipo: `googleSheetsTriggerOAuth2Api`
   - Permisos: Lectura de Google Sheets
   - Setup:
     1. Ve a [Google Cloud Console](https://console.cloud.google.com/)
     2. Crea un proyecto o selecciona uno existente
     3. Habilita Google Sheets API
     4. Crea credenciales OAuth 2.0
     5. Añade scopes: `https://www.googleapis.com/auth/spreadsheets.readonly`

2. **Google Gemini (PaLM) API**
   - Tipo: `googlePalmApi`
   - Cómo obtener:
     1. Ve a [Google AI Studio](https://makersuite.google.com/app/apikey)
     2. Crea una API Key
     3. Copia la key en n8n

3. **Gmail OAuth2**
   - Tipo: `gmailOAuth2`
   - Permisos: Envío de emails
   - Setup:
     1. Usa el mismo proyecto de Google Cloud Console
     2. Habilita Gmail API
     3. Usa las mismas credenciales OAuth 2.0
     4. Añade scopes: `https://www.googleapis.com/auth/gmail.send`

#### Configuración Necesaria

Antes de activar el workflow:

1. **Google Sheet**:
   - Crear o usar el formulario existente: "HouseZen (respuestas)"
   - ID del documento: `1ITVkDhlO71RJRIcyGbUBktM9uyeJibiFIKthCvmHOzY`
   - Hoja: "Respuestas de formulario 1"
   - Columnas requeridas:
     - Marca temporal
     - ¿Cuál es la avería?
     - Descripción detallada
     - ¿Es urgente?
     - Incluye una imagen o vídeo

2. **Emails**:
   - Actualizar el email destino en los nodos de Gmail (actualmente: `javierpm0609@gmail.com`)
   - Personalizar mensajes de email según necesidades

3. **Configurar credenciales** en n8n:
   - Google Sheets Trigger account
   - Google Gemini API account
   - Gmail account

#### Diagrama de Flujo

```
Google Sheets (nueva avería)
    ↓
Limit (última entrada)
    ↓
AI Agent (¿Seguro o Manitas?)
    ↓
    ├─ Si SEGURO → Email a propietario
    │
    └─ Si MANITAS → AI Agent LAU (¿Inquilino o Propietario?)
                        ↓
                        ├─ Si PROPIETARIO → Email a empresa reparaciones
                        │
                        └─ Si INQUILINO → Email a inquilino con opciones
                                              ↓
                                              └─ Si elige "Manitas HouseZen"
                                                 → Email a empresa reparaciones
```

#### Datos de Ejemplo

El workflow espera recibir datos con este formato del Google Sheet:

```json
{
  "Marca temporal": "2024/12/08 10:30:00",
  "¿Cuál es la avería?": "Grifo que gotea",
  "Descripción detallada": "El grifo del baño no cierra bien y gotea constantemente",
  "¿Es urgente?": "No",
  "Incluye una imagen o vídeo": "https://drive.google.com/file/d/..."
}
```

#### Notas Importantes

- El workflow está configurado para ejecutarse cada minuto
- Usa el nodo "Limit" para procesar solo la última fila nueva
- Los análisis de IA se basan en la Ley de Arrendamientos Urbanos española
- Cada clasificación incluye justificación legal y artículo LAU aplicable
- Los emails son interactivos (el inquilino puede responder con su elección)

---

## Plantilla de Documentación

Para documentar nuevos workflows, usa este formato:

### Workflow: [Nombre]
- **Archivo**: `nombre-archivo.json`
- **Descripción**: Qué hace este workflow
- **Trigger**: Cómo se activa (webhook, cron, manual, etc.)
- **Dependencias**: APIs o servicios externos que usa
- **Credenciales necesarias**: Qué credenciales configurar
