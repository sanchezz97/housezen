# Shared Files

Esta carpeta es para archivos compartidos entre workflows.

## Uso

Puedes usar esta carpeta para:
- Archivos CSV que procesen tus workflows
- Imágenes o assets que uses en tus automatizaciones
- Archivos de configuración compartidos
- Templates de emails o documentos

## Acceso desde n8n

Los workflows pueden acceder a esta carpeta en la ruta:
```
/home/node/shared/
```

## Ejemplo de Uso

En un nodo "Read Binary File":
```
File Path: /home/node/shared/template-email.html
```

En un nodo "Write Binary File":
```
File Path: /home/node/shared/output/reporte-{{ $now.format('YYYY-MM-DD') }}.csv
```

## Estructura Sugerida

```
shared/
├── templates/          # Templates de emails, PDFs, etc.
├── input/             # Archivos de entrada para workflows
├── output/            # Archivos generados por workflows
├── assets/            # Imágenes, logos, etc.
└── temp/              # Archivos temporales (ignorados por Git)
```

## Nota

Archivos grandes o temporales deben ir en la carpeta `temp/` que está ignorada por Git.
