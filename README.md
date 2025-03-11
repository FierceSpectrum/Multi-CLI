# MultiCLI - Documentación del Proyecto

## Objetivo del Proyecto CLI

**Nombre Tentativo:** MultiCLI

**Propósito:**

Crear una herramienta de línea de comandos (CLI) para generar estructuras base de proyectos Node.js personalizados. La herramienta debe:

- Soportar diversas arquitecturas (**Hexagonal, Clean, Microservices, Event-driven, etc.**).
- Permitir la elección de base de datos (**SQL vs NoSQL**) y ORMs (**Sequelize, Prisma, Mongoose, etc.**).
- Generar módulos (**scaffolding**) para componentes comunes (**usuarios, autenticación, productos, etc.**).
- Permitir la generación de archivos de configuración (`config.mcli.json`).
- Integrar soporte para **Docker**, **Git**, y **Git Flow**.
- Ofrecer flexibilidad mediante flags o comandos adicionales para modificar la estructura del proyecto.

## Requerimientos Funcionales

### 1. Comandos Base

**create <project-name>**

- Crea un nuevo proyecto con la estructura base.
- Flags y opciones:
  - `--arch <tipo>`: Seleccionar la arquitectura (**hexagonal, clean, etc.**).
  - `--db <tipo>`: Elegir la base de datos (**postgres, mysql, mongodb, sqlite**).
  - `--orm <tipo>`: Seleccionar el ORM (**sequelize, prisma, mongoose**).
  - `--modules <lista>`: Generar módulos específicos (**user, auth, product, etc.**).
  - `--typescript`: Indicar que el proyecto será en **TypeScript**.
  - `--docker`: Incluir archivos de **Docker**.
  - `--git`: Inicializa un repositorio **Git** y crea un `.gitignore`.
  - `--gitflow`: Inicializa **Git Flow**.
  - `--remote <repo_url>`: Agregar un repositorio remoto.
  - `--commit`: Realizar un primer commit automáticamente.

**generate module <nombre>**

- Agrega un nuevo módulo al proyecto existente.
- Genera archivos base (**controlador, modelo, rutas, etc.**).

**update config o config set <clave> <valor>**

- Modifica la configuración almacenada en `config.mfli.json`.

**list options o help**

- Muestra las arquitecturas, bases de datos, ORMs y módulos disponibles.

## Requerimientos No Funcionales

### 1. Lenguaje y Herramientas

- **Lenguaje:** Preferiblemente **TypeScript**, aunque se permite **JavaScript**.
- **Frameworks y Dependencias:**
  - **Commander.js:** Manejo de comandos.
  - **Inquirer.js:** Interacción con el usuario.
  - **Chalk:** Mensajes resaltados en terminal.
  - **fs-extra y path:** Manejo de archivos y directorios.
  - **Lodash:** Utilidades para manipulación de datos.
  - **SimpleGit:** Para interacción con **Git**.

### 2. Modularidad y Extensibilidad

- Permitir agregar nuevas arquitecturas y soportar nuevos ORMs sin grandes refactorizaciones.
- Facilitar contribuciones externas.

### 3. Configuración Persistente

El archivo `config.mcli.json` almacenará la configuración del proyecto, por ejemplo:

```json
{
  "projectName": "MiProyecto",
  "architecture": "hexagonal",
  "database": "postgres",
  "orm": "sequelize",
  "docker": true,
  "src": true,
  "modules": ["user", "auth"],
  "useGit": true,
  "gitRemote": "https://github.com/user/repo.git",
  "useGitFlow": true
}
```

### 4. Soporte para Contenedores y Despliegue

- Generación automática de `Dockerfile` y `docker-compose.yml` si la opción `--docker` está activada.

### 5. Validación y Feedback

- Corrección de nombres incorrectos en los comandos.
- Mensajes claros para indicar progreso y errores.

## Arquitectura del Proyecto CLI

### 1. Estructura Interna

```
/MultiCLI
├── bin/
│   └── cli.js               # Punto de entrada del CLI
├── src/
│   ├── application/         # Casos de uso y lógica de negocio
│   ├── domain/              # Entidades y modelos
│   ├── infrastructure/      # Adaptadores, frameworks externos
│   │   ├── cli/             # Comandos CLI
│   │   ├── fileSystem/      # Manejo de archivos y configuración
│   │   ├── templates/       # Plantillas de estructura de proyectos
│   ├── interfaces/          # Interfaces de usuario, CLI o API
│   ├── main/                # Punto de entrada
│   ├── utils/               # Funciones de utilidad
│   ├── config/              # Configuración del CLI
├── test/                    # (Opcional) Pruebas unitarias e integración
│   ├── application/
│   ├── domain/
│   ├── infrastructure/
│   ├── interfaces/
│   ├── main/
├── README.md                # Documentación principal
├── package.json             # Configuración de dependencias
├── cli.js                   # Archivo principal del CLI
├── .gitignore               # Archivos a ignorar en Git
├── config.mfli.json         # Archivo de configuración para personalización
```

### 2. Flujo de Ejecución

1. El usuario ejecuta `npx multi-cli create my-app --arch hexagonal --db postgres ...`
2. Se interpretan flags y opciones.
3. Se copian las plantillas de la arquitectura seleccionada.
4. Se genera `config.mcli.json` con la configuración elegida.
5. Se instalan dependencias básicas.
6. Se muestra feedback al usuario.

## Casos de Uso y Escenarios

### 1. Crear un proyecto nuevo

Comando:
```sh
npx multi-cli create my-app --arch hexagonal --db postgres --orm sequelize --modules user,auth --docker --git --gitflow
```

### 2. Agregar un módulo

Comando:
```sh
npx multi-cli generate module client
```

### 3. Modificar la configuración

Comando:
```sh
npx multi-cli config set orm prisma
```

## Consideraciones Finales

- **Flexibilidad:** Soporta múltiples arquitecturas, bases de datos y ORMs.
- **Extensibilidad:** Modularidad para futuras mejoras.
- **Configuración Persistente:** Uso de `config.mcli.json`.
- **Interacción y Validación:** Mensajes claros y sugerencias de correcciones.
- **Documentación:** README detallado y ayuda en línea.

