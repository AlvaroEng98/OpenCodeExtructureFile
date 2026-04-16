## Configuración de opencode: guía completa

Se ha demostrado que **opencode** ha marcado un antes y un después en el mundo del desarrollo. Por esta razón, tener una configuración adecuada es de vital importancia.

Los archivos de configuración pueden ubicarse en diferentes localizaciones, y su comportamiento es **fusionarse entre sí**, no sobrescribirse completamente unos a otros.

## Ubicación de los archivos de configuración

- Configuración remota (`.well-known/opencode`): valores predeterminados de la organización  
- Configuración global (`~/.config/opencode/opencode.json`): preferencias del usuario  
- Configuración personalizada (`OPENCODE_CONFIG`): anulaciones específicas  
- Configuración del proyecto (`opencode.json`): configuración propia del repositorio  

---

## Estructura del archivo de configuración

### Encabezado

Define el esquema base para validación:

```json
"$schema": "https://json-schema.org/draft/2020-12/schema",
```

### Nivel de logs

Controla la cantidad de información mostrada en la terminal.

```json
{
  "logLevel": "DEBUG"
}
```
### Servidor web

Configura el servidor para **opencode server** y **opencode web**.

```json
{
  "server": {
    "port": 4000,
    "hostname": "localhost",
    "mdns": true,
    "mdnsDomain": "miequipo.local",
    "cors": ["https://mi-app.com"]
  }
}
```
## Comandos personalizados

Permite definir atajos reutilizables mediante plantillas.

```json
{
  "command": {
    "review": {
      "template": "Revisa el código en {{file}} y sugiere mejoras",
      "description": "Revisión rápida de código",
      "agent": "build"
    },
    "doc": {
      "template": "Genera documentación JSDoc para {{selection}}",
      "description": "Documentar código seleccionado"
    }
  }
}
```
## Carpeta de skills

Permite añadir rutas locales o remotas con instrucciones especializadas.

```json
{
  "skills": {
    "paths": [
      "./my-skills",
      "/home/user/shared-skills"
    ],
    "urls": [
      "https://example.com/.well-known/skills/"
    ]
  }
}
```
## Modelo principal

Define el modelo por defecto.

```json
{
  "model": "anthropic/claude-opus-4-5",
  "small_model": "anthropic/claude-haiku-4-5-20251001"
}
```
## Agentes

Configura los agentes disponibles.

```json
{
  "agent": {
    "build": {
      "model": "anthropic/claude-sonnet-4-6",
      "prompt": "Eres un experto en TypeScript. Usa siempre tipos estrictos.",
      "steps": 30,
      "permission": {
        "bash": "allow",
        "edit": "allow",
        "webfetch": "deny"
      }
    },
    "mi-agente": {
      "mode": "subagent",
      "description": "Especialista en tests",
      "model": "anthropic/claude-haiku-4-5-20251001",
      "prompt": "Solo escribe tests.",
      "color": "#FF5733"
    }
  }
}
```
## Permisos globales

Define acciones permitidas sin confirmación.

```json
{
  "permission": {
    "bash": "ask",
    "edit": "allow",
    "read": "allow",
    "webfetch": "deny",
    "websearch": "allow"
  }
}
```
## Proveedores de modelos

Permite registrar o personalizar proveedores.

```json
{
  "provider": {
    "openai": {
      "options": {
        "apiKey": "sk-...",
        "baseURL": "https://api.openai.com/v1"
      }
    },
    "ollama": {
      "api": "openai",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      }
    }
  }
}
```
## Servidores MCP

Define integraciones externas.

```json
{
  "mcp": {
    "filesystem": {
      "type": "local",
      "command": ["npx", "-y", "@modelcontextprotocol/server-filesystem", "/ruta/proyecto"],
      "enabled": true
    },
    "mi-api": {
      "type": "remote",
      "url": "https://mcp.mi-empresa.com/sse",
      "headers": { "Authorization": "Bearer token" }
    }
  }
}
```
## Language Servers (LSP)

Mejora el análisis del código.

```json
{
  "lsp": {
    "typescript": {
      "command": ["typescript-language-server", "--stdio"],
      "extensions": [".ts", ".tsx", ".js", ".jsx"]
    },
    "python": {
      "command": ["pylsp"],
      "extensions": [".py"]
    }
  }
}
```
## Formateo de código

Define herramientas de formateo.

```json
{
  "formatter": {
    "prettier": {
      "command": ["npx", "prettier", "--write"],
      "extensions": [".ts", ".tsx", ".js", ".json", ".css"]
    },
    "black": {
      "command": ["black"],
      "extensions": [".py"]
    }
  }
}
```
## Instrucciones

Archivos incluidos como contexto adicional.

```json
{
  "instructions": [
    "./CONVENTIONS.md",
    "./docs/ARCHITECTURE.md",
    "./.opencode/context/*.md"
  ]
}
```
## Compartir sesiones

```json
{
  "share": "manual"
}
```
## Actualizaciones automáticas

```json
{
  "autoupdate": "notify"
}
```
## Compactación del modelo
Le indica que hacer cuando se alcanza el limite de contexto

```json
{
  "compaction": {
    "auto": true,
    "prune": true,
    "reserved": 8000
  }
}
```
## Snapshot de archivos 
Guarda snapshots del sistema de archivos, util para que pueda revertir cambios, y si se desactiva mejora el rendimiento

```json
{
  "snapshot": true
}
```
## Filtrado de proveedores

```json
{
  "enabled_providers": ["anthropic", "openai"],
  "disabled_providers": ["cohere"]
}
```
## Funciones experimentales

```json
{
  "experimental": {
    "batch_tool": true,
    "openTelemetry": false,
    "continue_loop_on_deny": true
  }
}
```
## Nombre de usuario
Para tener un nombre personalizado para interactuar con la herramienta

```json
{
  "username": "Custom"
}
```
## Agente por defecto

```json
{
  "default_agent": "build"
}
```
## Observador de archivos

```json
{
  "watcher": {
    "ignore": ["node_modules", "dist", ".git", "*.log"]
  }
}
```
