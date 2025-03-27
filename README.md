# MultiCLI - Documentación del Proyecto

## Objetivo del Proyecto CLI

**Nombre Tentativo:** MultiCLI

**Propósito:**

Crear una herramienta de línea de comandos (CLI) para generar estructuras base de proyectos Node.js personalizados. La herramienta debe:

- Soportar diversas arquitecturas (**Hexagonal, Clean, Microservices, Event-driven, etc.**).
- Permitir la elección de opciones básicas de configuración, como el uso de **TypeScript** o **Bases de Datos** y **ORMs**.
- Generar módulos (**scaffolding**) para componentes comunes (**usuarios, autenticación, productos, etc.**).
- Generar archivos de configuración personalizados, como el archivo `config.mcli.json`.
- Integrar soporte para **Docker**, **Git**, y **Git Flow**.
- Ofrecer flexibilidad mediante flags o comandos adicionales para modificar la estructura del proyecto.
- Utilizar **Commander.js** y **Inquirer.js** para interactuar con el usuario, y crear una estructura de carpetas y archivos listos para iniciar el desarrollo.

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

- Modifica la configuración almacenada en `config.mcli.json`.

**list options o help**

- Muestra las arquitecturas, bases de datos, ORMs y módulos disponibles.

## Requerimientos No Funcionales

### 1. Lenguaje y Herramientas

- **Lenguaje:** **TypeScript**.
- **Frameworks y Dependencias:**
  - **Commander.js:** Manejo de comandos.
  - **Inquirer.js:** Interacción con el usuario.
  - **Chalk:** Mensajes resaltados en terminal.
  - **fs-extra y path:** Manejo de archivos y directorios.
  - **Lodash:** Utilidades para manipulación de datos.
  - **SimpleGit:** Para interacción con **Git**.

### 2. Modularidad y Extensibilidad

- Permitir agregar nuevas arquitecturas y soportar nuevos módulos sin grandes refactorizaciones.
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
│
├── src/
│   ├── bin/
│   │   └── cli.ts                  # Punto de entrada (la terminal)
│   │
│   ├── commands/                   # Carpeta para todos los comandos CLI
│   │   ├── createProjectCommand.ts  # Lógica para crear el proyecto
│   │   ├── addModuleCommand.ts     # Lógica para agregar un módulo
│   │   └── initCommand.ts          # Lógica para inicializar el proyecto
│   │
│   ├── generators/                 # Carpeta con templates y lógica de generación de archivos
│   │   ├── mvcGenerator.ts         # Lógica para crear un proyecto MVC
│   │   ├── restGenerator.ts        # Lógica para crear un proyecto REST
│   │   └── testGenerator.ts        # Lógica para crear pruebas en el proyecto
│   │
│   ├── templates/                  # Templates de archivos (por ejemplo, MVC, REST, etc.)
│   │   ├── mvcTemplate/            # Templates para la arquitectura MVC
│   │   │   ├── controller.ts
│   │   │   ├── model.ts
│   │   │   └── view.ts
│   │   └── restTemplate/           # Templates para REST
│   │       ├── controller.ts
│   │       ├── route.ts
│   │       └── service.ts
│   │
│   ├── utils/                      # Funciones utilitarias para manejar archivos y directorios
│   │   └── fileUtils.ts            # Funciones de utilidades de archivos (como crear directorios)
│   │
│   ├── config/                     # Configuración global (si la necesitas)
│   │   └── defaultConfig.ts        # Parámetros por defecto de la CLI
│   │
│   └── tests/                      # Pruebas unitarias de los generadores y comandos
│       ├── generateProject.test.ts # Pruebas para la generación de proyectos
│       └── utils.test.ts           # Pruebas para las utilidades
│
├── docs/
│   ├── README.md            # Documentación principal
│   ├── architecture.md      # Documentación de la estructura del código.
│   └── taskplan.md          # Documentación de la planificación de tareas.
│        
├── package.json             # Configuración de dependencias
├── cli.js                   # Archivo principal del CLI
├── .gitignore               # Archivos a ignorar en Git
├── config.mfli.json         # Archivo de configuración para personalización
└── tsconfig.json                   # Configuración de TypeScript
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

