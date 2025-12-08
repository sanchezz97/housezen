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

## Documentación de Workflows

Documentar cada workflow importante aquí:

### Workflow: [Nombre]
- **Archivo**: `nombre-archivo.json`
- **Descripción**: Qué hace este workflow
- **Trigger**: Cómo se activa (webhook, cron, manual, etc.)
- **Dependencias**: APIs o servicios externos que usa
- **Credenciales necesarias**: Qué credenciales configurar

---

## Ejemplo:

### Workflow: Procesamiento de Leads
- **Archivo**: `workflow-procesamiento-leads.json`
- **Descripción**: Recibe leads de formulario web, los valida y los envía a CRM
- **Trigger**: Webhook (POST request)
- **Dependencias**:
  - API de CRM (Salesforce/HubSpot)
  - Email service (SendGrid)
- **Credenciales necesarias**:
  - CRM API Key
  - SendGrid API Key
