# HouseZen - Sistema de Gestión Inteligente de Averías

Sistema automatizado de gestión y clasificación de averías de propiedades usando n8n, inteligencia artificial y análisis legal basado en la Ley de Arrendamientos Urbanos (LAU).

## Acerca del Proyecto

HouseZen es una plataforma de automatización que ayuda a gestionar averías en propiedades de alquiler de forma inteligente. Utiliza IA (Google Gemini) para:

- Clasificar automáticamente averías según requieran seguro o reparación
- Determinar responsabilidades legales (inquilino vs propietario) según la LAU
- Automatizar comunicaciones con todas las partes involucradas
- Gestionar presupuestos y coordinación con empresas de reparación

### Características Principales

✅ **Clasificación Inteligente con IA**: Analiza cada avería y determina si requiere seguro o servicio de reparación
✅ **Análisis Legal LAU**: Determina responsabilidades según la Ley de Arrendamientos Urbanos
✅ **Automatización de Comunicaciones**: Envía emails automáticos a inquilinos, propietarios y empresas
✅ **Integración con Google Forms**: Recibe averías directamente desde formularios
✅ **100% Gratuito**: Usa herramientas open-source y APIs gratuitas

## Tecnologías Utilizadas

- **n8n**: Plataforma de automatización de workflows (self-hosted)
- **Docker**: Contenedorización de la aplicación
- **Google Gemini AI**: Inteligencia artificial para clasificación
- **Google Sheets API**: Recepción de formularios de averías
- **Gmail API**: Automatización de comunicaciones por email

## Requisitos Previos

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) instalado (gratis)
- [Git](https://git-scm.com/downloads) instalado
- Cuenta de Google (para Google Sheets, Gemini AI, y Gmail)
- Editor de código (VS Code recomendado)

## Instalación Inicial

### 1. Clonar el repositorio

```bash
git clone https://github.com/sanchezz97/housezen.git
cd housezen
```

### 2. Configurar variables de entorno

```bash
# Copiar el archivo de ejemplo
cp .env.example .env

# Editar el archivo .env con tus credenciales
# IMPORTANTE: Cambiar el password por uno seguro
```

### 3. Iniciar n8n

```bash
# Levantar el contenedor
docker-compose up -d

# Ver los logs (opcional)
docker-compose logs -f
```

### 4. Acceder a n8n

Abre tu navegador en: http://localhost:5678

Credenciales (según tu archivo .env):
- Usuario: admin
- Password: (el que configuraste en .env)

### 5. Importar el workflow del proyecto

1. En n8n, ve a "Workflows"
2. Click en "Import from File"
3. Selecciona el archivo `workflows/workflow-clasificacion-averias.json`
4. El workflow se importará con todos sus nodos

### 6. Configurar credenciales de APIs

Ver documentación detallada en [workflows/README.md](workflows/README.md)

**Credenciales necesarias**:
1. Google Sheets Trigger OAuth2
2. Google Gemini (PaLM) API
3. Gmail OAuth2

**Guías rápidas**:
- [Obtener Google Gemini API Key](https://makersuite.google.com/app/apikey)
- [Configurar Google Cloud Console](https://console.cloud.google.com/)
- Habilitar APIs: Google Sheets API y Gmail API

## Uso Diario

### Iniciar n8n
```bash
docker-compose up -d
```

### Detener n8n
```bash
docker-compose down
```

### Ver logs
```bash
docker-compose logs -f n8n
```

### Reiniciar n8n
```bash
docker-compose restart
```

## Flujo de Trabajo Colaborativo

### Exportar Workflows

1. En n8n, ve a tu workflow
2. Click en el menú (3 puntos) → "Download"
3. Guarda el archivo JSON en la carpeta `workflows/`
4. Nombra el archivo descriptivamente: `workflow-nombre-descriptivo.json`

### Importar Workflows

1. En n8n, ve a "Workflows"
2. Click en "Import from File"
3. Selecciona el archivo JSON de la carpeta `workflows/`

### Subir Cambios a GitHub

```bash
# Ver estado de archivos
git status

# Añadir workflows nuevos o modificados
git add workflows/

# Crear commit
git commit -m "Descripción del workflow o cambio"

# Subir a GitHub
git push origin main
```

### Sincronizar Cambios de GitHub

```bash
# Descargar últimos cambios
git pull origin main

# Importar los workflows nuevos en tu n8n local
```

### Trabajar en Paralelo (Ramas)

Si dos personas trabajan en workflows diferentes:

```bash
# Persona 1 - Crear rama para su feature
git checkout -b feature/workflow-envio-emails
# ... trabaja en su workflow ...
git add workflows/
git commit -m "Add: Workflow de envío de emails"
git push origin feature/workflow-envio-emails

# Persona 2 - Crear rama para su feature
git checkout -b feature/workflow-notificaciones
# ... trabaja en su workflow ...
git add workflows/
git commit -m "Add: Workflow de notificaciones"
git push origin feature/workflow-notificaciones

# Luego hacer Pull Requests en GitHub y merge
```

## Cómo Funciona el Workflow

El workflow de clasificación de averías funciona de la siguiente manera:

1. **Recepción**: Un formulario de Google Forms recoge información de averías
2. **Trigger**: Google Sheets detecta nuevas entradas cada minuto
3. **Clasificación IA**: Determina si es caso de seguro o requiere reparación
4. **Análisis Legal**: Si es reparación, analiza según LAU quién es responsable
5. **Comunicación**: Envía emails automáticos a las partes correspondientes
6. **Gestión**: Coordina con empresas de reparación según sea necesario

Ver documentación completa del workflow en [workflows/README.md](workflows/README.md)

## Estructura del Proyecto

```
housezen/
├── docker-compose.yml                      # Configuración de Docker
├── .env                                    # Variables de entorno (NO SUBIR A GIT)
├── .env.example                           # Plantilla de variables
├── .gitignore                             # Archivos ignorados por Git
├── workflows/                             # Workflows exportados (JSON)
│   ├── README.md                         # Documentación detallada de workflows
│   └── workflow-clasificacion-averias.json  # Workflow principal del proyecto
├── credentials/                           # Plantillas de credenciales
│   └── credentials.example.json
├── shared/                                # Archivos compartidos entre workflows
└── README.md                             # Este archivo
```

## Credenciales y Seguridad

⚠️ **IMPORTANTE**:
- NUNCA subir el archivo `.env` a GitHub
- NUNCA subir credenciales reales a GitHub
- Usar el archivo `credentials.example.json` como plantilla
- Cada desarrollador configura sus propias credenciales localmente

## Backup de Datos

Los datos de n8n se guardan en un volumen de Docker. Para hacer backup:

```bash
# Exportar todos los workflows desde la interfaz de n8n
# O hacer backup del volumen Docker:
docker run --rm -v housezen_n8n_data:/data -v $(pwd)/backup:/backup alpine tar czf /backup/n8n-backup.tar.gz /data
```

## Solución de Problemas

### El contenedor no inicia
```bash
docker-compose logs n8n
```

### Resetear n8n completamente
```bash
docker-compose down -v  # ⚠️ ESTO BORRA TODOS LOS DATOS
docker-compose up -d
```

### Puerto 5678 ya está en uso
Cambiar el puerto en `docker-compose.yml`:
```yaml
ports:
  - "5679:5678"  # Cambiar 5678 por otro puerto
```

## Despliegue en Producción

Para producción (workflows 24/7), opciones gratuitas:

1. **Railway** (Recomendado)
   - 500 horas gratis/mes
   - Fácil deploy desde GitHub

2. **Render**
   - Tier gratuito disponible
   - Deploy automático

3. **Oracle Cloud**
   - Always Free tier
   - Más complejo de configurar

Documentación para deploy vendrá en futuras actualizaciones.

## Recursos Útiles

- [Documentación oficial de n8n](https://docs.n8n.io/)
- [Templates de workflows](https://n8n.io/workflows/)
- [Comunidad n8n](https://community.n8n.io/)

## Contribuir

1. Crear rama para tu feature
2. Hacer cambios y commits
3. Push a GitHub
4. Crear Pull Request
5. Esperar revisión y merge

## Licencia

Proyecto privado - Housezen
