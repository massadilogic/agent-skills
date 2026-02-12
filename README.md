# Agent Skills Library

Una forma sencilla de integrar **Agent Skills** [Agent Skills open standard](https://agentskills.io) en cualquier proyecto o repositorio. Esta librería permite estandarizar y automatizar el uso de skills para diferentes asistentes de IA como Claude Code, Gemini CLI, Codex (OpenAI) y GitHub Copilot.

## 🚀 Características Principales

- **Integración Multi-IA**: Compatible con Claude Code, Gemini CLI, Codex y GitHub Copilot
- **Configuración Automática**: Scripts de configuración que crean symlinks y archivos necesarios
- **Sincronización Centralizada**: Sistema de sincronización para mantener AGENTS.md actualizado
- **Skills por Dominio**: Organización de skills por contextos (UI, API, etc.)
- **Skill Creator**: Herramienta para crear nuevas skills con formato estándar
- **Rutas Configurables**: Sistema de rutas para mapear contextos a directorios

## 📁 Estructura del Proyecto

```
agent-skills/
├── AGENTS.md              # Archivo principal de configuración
├── context/               # Carpeta principal de skills
│   ├── routes.json        # Configuración de rutas por contexto
│   └── skills/            # Directorio de skills
│       ├── setup.sh       # Script de configuración principal
│       ├── skill-sync/    # Sistema de sincronización
│       ├── skill-creator/ # Herramienta de creación de skills
│       └── [dominios]/    # Skills organizadas por dominio
│           ├── app/       # Skills de aplicación general
│           ├── ui/        # Skills de interfaz de usuario
│           ├── api/       # Skills de API
│           └── ...
└── README.md              # Este archivo
```

## 🛠️ Instalación y Configuración

### Paso 1: Integrar al Proyecto

Para integrar esta librería en tu proyecto, simplemente copia:

1. La carpeta `context/` completa
2. El archivo `AGENTS.md`

en la raíz de tu proyecto.

### Paso 2: Ejecutar Script de Configuración

El script principal de configuración crea la estructura necesaria para cada IA:

```bash
# Navegar al directorio de skills
cd context/skills

# Ejecutar script de configuración
./setup.sh
```

#### Opciones del Script de Configuración

```bash
# Modo interactivo (seleccionar IAs manualmente)
./setup.sh

# Configurar todos los asistentes de IA
./setup.sh --all

# Configurar asistentes específicos
./setup.sh --claude --codex
./setup.sh --gemini --copilot

# Ver ayuda
./setup.sh --help
```

#### ¿Qué hace el script de configuración?

El script `setup.sh` realiza las siguientes acciones:

1. **Claude Code**:
   - Crea symlink: `.claude/skills/ -> context/skills/`
   - Copia `AGENTS.md` a `CLAUDE.md`

2. **Gemini CLI**:
   - Crea symlink: `.gemini/skills/ -> context/skills/`
   - Copia `AGENTS.md` a `GEMINI.md`

3. **Codex (OpenAI)**:
   - Crea symlink: `.codex/skills/ -> context/skills/`
   - Usa `AGENTS.md` de forma nativa

4. **GitHub Copilot**:
   - Copia `AGENTS.md` a `.github/copilot-instructions.md`

### Paso 3: Personalizar Skills

Después de la configuración inicial:

1. **Eliminar skills no necesarias**: Remueve los directorios de skills que no correspondan a tu proyecto
2. **Agregar skills específicas**: Crea nuevas skills según las necesidades de tu proyecto
3. **Organizar por dominio**: Agrupa skills por contexto (UI, API, etc.) para mayor eficiencia

### Paso 4: Sincronizar Cambios

Después de modificar las skills, ejecuta el script de sincronización:

```bash
# Desde el directorio de skills
./skill-sync/assets/sync.sh
```

## 🔄 Sistema de Sincronización

El script `sync.sh` mantiene actualizado el archivo `AGENTS.md` con las skills disponibles.

### Uso del Script de Sincronización

```bash
# Sincronizar todas las skills
./skill-sync/assets/sync.sh

# Vista previa sin aplicar cambios
./skill-sync/assets/sync.sh --dry-run

# Sincronizar solo un contexto específico
./skill-sync/assets/sync.sh --scope ui

# Ver ayuda
./skill-sync/assets/sync.sh --help
```

### ¿Qué hace el script de sincronización?

1. **Escanea skills**: Busca todos los archivos `SKILL.md` en el directorio de skills
2. **Extrae metadata**: Lee la información de scope y auto-invoke de cada skill
3. **Genera tablas**: Crea tablas de auto-invoke organizadas por contexto
4. **Actualiza AGENTS.md**: Reemplaza la sección "Auto-invoke Skills" con la información actualizada

### Estructura de Metadata de Skills

Cada skill debe incluir metadata en su frontmatter YAML:

```yaml
---
name: nombre-de-la-skill
description: Descripción de la skill
metadata:
  scope: ui,api  # Contextos donde aplica
  auto_invoke:
    - "Acción que invoca esta skill"
    - "Otra acción"
---
```

## 🎯 Organización por Dominios

Las skills se organizan por dominios para mayor eficiencia:

### Dominios Comunes

- **`app/`**: Skills de aplicación general
- **`ui/`**: Skills de interfaz de usuario (React, Tailwind, etc.)
- **`api/`**: Skills de backend y APIs
- **`typescript/`**: Skills específicas de TypeScript
- **`testing/`**: Skills de pruebas y testing

### Configuración de Rutas

El archivo `context/routes.json` define cómo se mapean los dominios a directorios:

```json
{
  "root": ".",
  "ui": "ui",
  "api": "api"
}
```

Esto permite que las skills se activen automáticamente según el contexto del archivo donde se está trabajando.

## 🛠️ Skill Creator

Para crear nuevas skills usando el formato estándar:

```bash
# Usar el skill-creator
./skill-creator/assets/create.sh
```

El skill creator guía en la creación de nuevas skills con:
- Estructura YAML correcta
- Metadata necesaria
- Formato estándar de documentación

## 📋 Ejemplo de Flujo de Trabajo Completo

1. **Integración inicial**:
   ```bash
   cp -r /ruta/agent-skills/context ./context
   cp /ruta/agent-skills/AGENTS.md ./AGENTS.md
   ```

2. **Configuración**:
   ```bash
   cd context/skills
   ./setup.sh --all
   ```

3. **Personalización**:
   ```bash
   # Eliminar skills no necesarias
   rm -rf skills/database/
   
   # Crear nueva skill específica del proyecto
   ./skill-creator/assets/create.sh
   ```

4. **Sincronización**:
   ```bash
   ./skill-sync/assets/sync.sh
   ```

5. **Verificación**:
   ```bash
   # Revisar AGENTS.md actualizado
   cat ../../AGENTS.md
   ```

## 🔧 Configuración Avanzada

### Modificar Rutas

Para agregar nuevos contextos, edita `context/routes.json`:

```json
{
  "root": ".",
  "ui": "ui",
  "api": "api",
  "mobile": "mobile",
  "docs": "documentation"
}
```

### Skills Multi-contexto

Una skill puede aplicarse a múltiples contextos:

```yaml
---
metadata:
  scope: ui,api,mobile
  auto_invoke:
    - "Crear componente"
    - "Definir endpoint"
---
```

## 📚 Referencia de Scripts

### setup.sh

**Propósito**: Configurar el entorno para diferentes asistentes de IA

**Ubicación**: `context/skills/setup.sh`

**Parámetros**:
- `--all`: Configurar todos los asistentes
- `--claude`: Configurar Claude Code
- `--gemini`: Configurar Gemini CLI
- `--codex`: Configurar Codex (OpenAI)
- `--copilot`: Configurar GitHub Copilot

**Archivos creados**:
- `.claude/skills/` (symlink)
- `.gemini/skills/` (symlink)
- `.codex/skills/` (symlink)
- `.github/copilot-instructions.md` (copia)
- `CLAUDE.md`, `GEMINI.md` (copias)

### sync.sh

**Propósito**: Sincronizar metadata de skills con AGENTS.md

**Ubicación**: `context/skills/skill-sync/assets/sync.sh`

**Parámetros**:
- `--dry-run`: Vista previa sin cambios
- `--scope <contexto>`: Sincronizar solo un contexto

**Proceso**:
1. Lee archivos `SKILL.md`
2. Extrae metadata YAML
3. Genera tablas de auto-invoke
4. Actualiza sección en AGENTS.md

## 🤝 Contribución

Para contribuir nuevas skills:

1. Usa el skill-creator para mantener formato estándar
2. Incluye metadata completa (scope, auto_invoke)
3. Agrupa por dominio apropiado
4. Ejecuta sync.sh después de agregar o eliminar skills
5. Prueba con diferentes asistentes de IA

## 📄 Licencia

Esta librería está diseñada para ser integrada en proyectos de código abierto y privados. Las skills individuales pueden tener sus propias licencias.

## 🔗 Recursos Adicionales

- [Documentación de AGENTS.md](./AGENTS.md)
- [Skills disponibles](./context/skills/)
- [Agent Skills open standard](https://agentskills.io)

---

**Nota**: Después de cualquier cambio en las skills o configuración, recuerda ejecutar el script de sincronización para mantener actualizado el sistema de auto-invoke.
