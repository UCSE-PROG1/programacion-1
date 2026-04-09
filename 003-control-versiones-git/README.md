# Unidad 3: Control de Versiones con Git y GitHub

## Tabla de Contenidos

1. [¿Qué es Git y el Control de Versiones?](#1-qué-es-git-y-el-control-de-versiones)
2. [Configuración Inicial de Git](#2-configuración-inicial-de-git)
3. [Inicializar un Repositorio](#3-inicializar-un-repositorio)
4. [El Flujo Básico de Git](#4-el-flujo-básico-de-git)
5. [Ver el Historial](#5-ver-el-historial)
6. [Trabajar con Repositorios Remotos (GitHub)](#6-trabajar-con-repositorios-remotos-github)
7. [Resolución de Conflictos](#7-resolución-de-conflictos)
8. [Buenas Prácticas](#8-buenas-prácticas)
9. [.gitignore](#9-gitignore)

---

## 1. ¿Qué es Git y el Control de Versiones?

Imaginá que estás desarrollando un proyecto y guardás los archivos así:

```
proyecto_final.cs
proyecto_final_v2.cs
proyecto_final_v2_corregido.cs
proyecto_final_ESTE_SI.cs
proyecto_final_ESTE_SI_de_verdad.cs
```

Este es un problema muy común cuando no se usa ninguna herramienta de control de versiones. Se vuelve imposible saber cuál es la versión correcta, qué cambió entre una y otra, o quién hizo cada modificación.

Un **Sistema de Control de Versiones (VCS)** resuelve estos problemas al registrar cada cambio realizado en el código a lo largo del tiempo, permitiendo volver a cualquier versión anterior, identificar quién hizo cada cambio y cuándo, y facilitar el trabajo de múltiples personas sobre el mismo proyecto.

**Git** es el sistema de control de versiones distribuido más utilizado en el mundo. La palabra *distribuido* significa que cada desarrollador tiene una copia completa del repositorio en su propia máquina, incluyendo todo el historial. Git registra por cada cambio:

- **Qué** archivos fueron modificados y exactamente qué cambió.
- **Quién** realizó el cambio (nombre y email del autor).
- **Cuándo** se realizó el cambio (fecha y hora exacta).
- **Por qué** se realizó el cambio (mensaje del commit escrito por el desarrollador).

### Conceptos clave

| Concepto | Descripción |
|---|---|
| **Repositorio** | Carpeta del proyecto gestionada por Git, contiene todo el historial |
| **Commit** | Una "foto" o instantánea del estado del proyecto en un momento dado |
| **Branch (Rama)** | Una línea de desarrollo paralela e independiente |
| **Staging Area** | Zona intermedia donde se preparan los cambios antes de hacer un commit |

### Cómo fluyen los archivos en Git

```
┌─────────────────┐    git add     ┌──────────────────┐    git commit    ┌──────────────────┐
│                 │  ──────────>   │                  │  ─────────────>  │                  │
│  Directorio de  │                │  Staging Area    │                  │   Repositorio    │
│     Trabajo     │                │  (Área de        │                  │   Local (.git)   │
│  (Working Dir)  │  <──────────   │   Preparación)   │                  │                  │
│                 │  git restore   │                  │                  │                  │
└─────────────────┘                └──────────────────┘                  └──────────────────┘
       ^                                                                          |
       |                                                                          |
       └──────────────────────────── git checkout ──────────────────────────────┘
```

- **Working Dir**: donde editás los archivos normalmente.
- **Staging Area**: donde "marcás" los cambios que querés incluir en el próximo commit.
- **Repositorio Local**: donde se guarda el historial permanente de commits.

Cada commit queda registrado como un punto en la línea de tiempo del proyecto:

```
Tiempo ──────────────────────────────────────────────────>

  [v1]          [v2]          [v3]          [v4]
   |             |             |             |
   |  Creo el    |  Agrego la  |  Corrijo    |  Agrego
   |  proyecto   |  clase      |  un bug     |  tests
   |  inicial    |  Persona    |  en Login   |
   |             |             |             |
  (commit 1)   (commit 2)   (commit 3)   (commit 4)
```

En cualquier momento podés volver a cualquiera de esos puntos.

### Instalación de Git

#### Windows

1. Descargá el instalador desde [git-scm.com](https://git-scm.com/download/win).
2. Ejecutá el instalador (`.exe`) y seguí los pasos. Las opciones por defecto son correctas para la mayoría de los casos.
3. Durante la instalación, en el paso **"Choosing the default editor"**, se recomienda seleccionar **Visual Studio Code** si ya lo tenés instalado.
4. Al finalizar, abrí **Git Bash** (se instala junto con Git) o una terminal y verificá:

```bash
git --version
# Salida esperada: git version 2.x.x.windows.x
```

#### macOS

En macOS, Git suele venir preinstalado. Verificá abriendo la terminal:

```bash
git --version
```

Si no está instalado, el sistema te ofrecerá instalar las **Xcode Command Line Tools** automáticamente. Aceptá y seguí los pasos.

Alternativamente, podés instalarlo con **Homebrew**:

```bash
# Instalar Homebrew (si no lo tenés)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Instalar Git
brew install git
```

---

## 2. Configuración Inicial de Git

Antes de empezar, verificá que Git esté instalado:

```bash
git --version
# Salida esperada: git version 2.43.0
```

Luego configurá tu identidad. Esta información queda registrada en cada commit que realices. La configuración `--global` aplica a todos los repositorios en tu máquina.

```bash
# Configurar nombre de usuario (reemplazá con tu nombre real)
git config --global user.name "Juan Pérez"

# Configurar email (usá el mismo email que tu cuenta de GitHub)
git config --global user.email "juan.perez@gmail.com"

# Configurar editor de texto (Visual Studio Code)
git config --global core.editor "code --wait"

# Configurar que la rama por defecto se llame 'main'
git config --global init.defaultBranch main

# Verificar toda la configuración guardada
git config --list

# Verificar un valor específico
git config user.name
# Salida: Juan Pérez
```

---

## 3. Inicializar un Repositorio

El comando `git init` convierte cualquier carpeta en un repositorio Git. Crea una carpeta oculta llamada `.git` que contiene todo el historial del proyecto.

> **Importante**: nunca modificues ni eliminés manualmente la carpeta `.git`. Si la borrás, perdés todo el historial del proyecto.

```bash
# Crear una nueva carpeta para el proyecto
mkdir mi-proyecto-csharp
cd mi-proyecto-csharp

# Inicializar Git en esa carpeta
git init
# Salida: Initialized empty Git repository in /mi-proyecto-csharp/.git/

# Ver el estado inicial del repositorio
git status
# Salida:
# On branch main
# No commits yet
# nothing to commit (create/copy files and use "git add" to track)
```

---

## 4. El Flujo Básico de Git

El flujo de trabajo cotidiano en Git se basa en tres comandos que se repiten constantemente:

1. **`git status`**: muestra qué archivos fueron modificados, cuáles están en el staging area y cuáles no están siendo rastreados.
2. **`git add`**: agrega archivos al staging area.
   - `git add nombre-archivo.cs` — agrega un archivo específico.
   - `git add .` — agrega todos los archivos modificados y nuevos.
3. **`git commit -m "mensaje"`**: crea un commit con los archivos del staging area.

```bash
# 1. Crear un archivo nuevo
echo "// Mi primer archivo C#" > Calculadora.cs

# 2. Ver el estado: Git detecta el archivo pero no lo rastrea aún
git status
# Salida:
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         Calculadora.cs

# 3. Agregar el archivo al staging area
git add Calculadora.cs

# 4. Ver el estado: el archivo ahora está listo para el commit
git status
# Salida:
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   Calculadora.cs

# 5. Crear el commit con un mensaje descriptivo
git commit -m "Agregar clase Calculadora con estructura inicial"
# Salida:
# [main (root-commit) a3f8b2c] Agregar clase Calculadora con estructura inicial
#  1 file changed, 1 insertion(+)
#  create mode 100644 Calculadora.cs

# 6. Verificar que el repositorio está limpio
git status
# Salida:
# On branch main
# nothing to commit, working tree clean
```

---

## 5. Ver el Historial

El comando `git log` permite consultar el historial de commits de diferentes formas:

| Comando | Descripción |
|---|---|
| `git log` | Historial completo con hash, autor, fecha y mensaje |
| `git log --oneline` | Una línea por commit, más compacto |
| `git log --oneline --graph --all` | Vista gráfica de todas las ramas |

```bash
# Historial completo
git log
# Salida:
# commit d4e5f6a (HEAD -> main)
# Author: Juan Pérez <juan@gmail.com>
# Date:   Mon Mar 18 10:30:00 2026 -0300
#
#     Agregar método de división con validación
#
# commit b2c3d4e
# Author: Juan Pérez <juan@gmail.com>
# Date:   Mon Mar 18 09:15:00 2026 -0300
#
#     Agregar método de resta

# Historial compacto
git log --oneline
# Salida:
# d4e5f6a (HEAD -> main) Agregar método de división con validación
# b2c3d4e Agregar método de resta
# a1b2c3d Agregar clase Calculadora con suma inicial

# Vista con gráfico de ramas
git log --oneline --graph --all
```

Cómo leer el historial:
- El commit más reciente aparece **primero** (arriba).
- `HEAD -> main` indica en qué commit y rama estás actualmente.
- El hash corto (como `d4e5f6a`) identifica unívocamente cada commit.

---

## 6. Trabajar con Repositorios Remotos (GitHub)

**GitHub** es una plataforma web que hospeda repositorios Git remotos. Permite tener una copia de seguridad en la nube, colaborar con otros desarrolladores y gestionar las entregas de la materia.

La forma que vamos a usar en esta materia es siempre la misma: **primero crear el repositorio en GitHub y luego clonarlo en una carpeta vacía local**. Esto evita tener que vincular manualmente el repositorio remoto y garantiza que todo esté correctamente configurado desde el inicio.

### Flujo de trabajo

**Paso 1 — Crear el repositorio en GitHub**

1. Ir a [github.com](https://github.com) e iniciar sesión.
2. Hacer clic en el botón **"New"** (o el ícono `+` → "New repository").
3. Completar el nombre del repositorio.
4. Seleccionar **Private**.
5. Tildar **"Add a README file"** para que el repositorio no quede vacío.
6. Hacer clic en **"Create repository"**.

**Paso 2 — Agregar a los profesores como colaboradores**

Para que los profesores puedan ver y revisar tu repositorio privado, debés agregarlos como colaboradores:

1. En la página del repositorio, ir a **Settings** → **Collaborators** → **"Add people"**.
2. Agregar los siguientes correos uno por uno:
   - `gonzaperez2312@gmail.com`
   - `mateolerda03@gmail.com`
   - `maximilianolovera@gmail.com`
3. Hacer clic en **"Add collaborator"** para cada uno.

> Este paso es obligatorio en cada entrega. Sin acceso, los profesores no pueden corregir el trabajo.

**Paso 4 — Clonar el repositorio en tu máquina**

En la página del repositorio recién creado, hacer clic en el botón verde **"Code"** y copiar la URL (HTTPS).

```bash
# Clonar el repositorio en una carpeta nueva (se crea sola con el nombre del repo)
git clone https://github.com/tu-usuario/mi-proyecto-csharp.git

# Entrar a la carpeta clonada
cd mi-proyecto-csharp
```

A partir de este punto, la carpeta ya está vinculada con el repositorio remoto. No es necesario ningún `git remote add`.

**Paso 5 — Trabajar, commitear y subir cambios**

```bash
# Hacer cambios en los archivos del proyecto...

# Ver el estado
git status

# Agregar los cambios al staging area
git add .

# Crear un commit
git commit -m "Agregar clase Calculadora con estructura inicial"

# Subir los commits al repositorio remoto
git push
```

**Paso 6 — Descargar cambios del remoto**

Si trabajás desde otra computadora o un compañero subió cambios, siempre sincronizá antes de empezar:

```bash
git pull
```

### Referencia de comandos

| Comando | Descripción |
|---|---|
| `git clone URL` | Descarga el repositorio remoto y lo vincula automáticamente |
| `git push` | Sube los commits locales al repositorio remoto |
| `git pull` | Descarga y fusiona los cambios del remoto |

### Repositorios públicos vs privados

- **Público**: cualquier persona en internet puede ver el código. Ideal para portfolios o proyectos de código abierto.
- **Privado**: solo las personas con acceso explícito pueden verlo. En esta materia las entregas son **privadas** y se otorga acceso al docente.

### El archivo README.md

El `README.md` es la "portada" del repositorio: se muestra automáticamente en GitHub al entrar al repositorio. Un buen README incluye una descripción del proyecto, instrucciones de uso y estructura de carpetas.

### Fork y Pull Request

En proyectos colaborativos el flujo típico es:

1. **Fork**: hacer una copia del repositorio original en tu propia cuenta de GitHub.
2. **Clone**: descargar tu fork a tu máquina local.
3. **Branch + Commits**: crear una rama, hacer los cambios y subirlos.
4. **Pull Request (PR)**: proponer al dueño del repositorio original que incorpore tus cambios.

---

## 7. Resolución de Conflictos

Un **conflicto** ocurre cuando Git no puede fusionar automáticamente dos ramas porque ambas modificaron la misma línea del mismo archivo. Git marca el archivo con delimitadores especiales:

```
<<<<<<< HEAD
    // Esta es la versión de la rama actual (main)
    public int Sumar(int a, int b) => a + b;
=======
    // Esta es la versión de la rama que se está fusionando
    public int Sumar(int x, int y) => x + y;
>>>>>>> feature/renombrar-parametros
```

- Entre `<<<<<<< HEAD` y `=======`: código de la rama actual.
- Entre `=======` y `>>>>>>>`: código de la rama que se quiere fusionar.

**Pasos para resolver un conflicto:**

1. Identificar los archivos en conflicto con `git status`.
2. Abrir cada archivo y decidir qué código conservar.
3. Eliminar los marcadores `<<<<<<<`, `=======` y `>>>>>>>`.
4. Guardar el archivo con el resultado final.
5. Agregar el archivo resuelto: `git add`.
6. Completar el merge: `git commit`.

```bash
# Al fusionar, Git reporta el conflicto
git merge feature/renombrar-parametros
# CONFLICT (content): Merge conflict in Calculadora.cs
# Automatic merge failed; fix conflicts and then commit the result.

# Ver cuáles archivos tienen conflictos
git status
# Unmerged paths:
#         both modified:   Calculadora.cs

# (Editamos el archivo, resolvemos el conflicto y guardamos)

# Marcar como resuelto y completar el merge
git add Calculadora.cs
git commit -m "Resolver conflicto: mantener nombres de parámetros a, b en Sumar"
```

---

## 8. Buenas Prácticas

**Sincronizá antes de empezar a trabajar:**
```bash
git pull
```
Evita trabajar sobre una versión desactualizada y genera conflictos innecesarios.

**Mensajes de commit descriptivos**: usá modo imperativo ("Agregar", "Corregir", "Refactorizar"), no "Agregué" ni "Se agrega". Un buen mensaje responde a: *¿qué hace este commit al proyecto?*

```bash
# MALOS mensajes
git commit -m "fix"
git commit -m "cambios"
git commit -m "listo"

# BUENOS mensajes
git commit -m "Agregar clase Persona con propiedades Nombre y Edad"
git commit -m "Corregir validación de edad negativa en constructor"
git commit -m "Agregar tests unitarios para el caso de división por cero"
```

**Commits pequeños y frecuentes**: es mejor muchos commits pequeños que uno grande con todos los cambios. Facilita encontrar bugs y entender la evolución del proyecto.

**Nunca hacer force push a main**: `git push --force` reescribe el historial remoto y puede hacer que otros colaboradores pierdan trabajo.

**Mínimo 4 commits por práctica**: cada entrega debe tener al menos 4 commits significativos que muestren la evolución del trabajo.

---

## 9. .gitignore

El archivo **`.gitignore`** le indica a Git qué archivos o carpetas debe ignorar. Sirve para no subir archivos generados automáticamente (binarios, compilados), configuración personal del IDE o credenciales.

Para proyectos .NET, los patrones más importantes son:

| Patrón | Qué ignora |
|---|---|
| `bin/` | Archivos compilados (ejecutables, DLLs) |
| `obj/` | Archivos intermedios de compilación |
| `.vs/` | Configuración de Visual Studio (específica de cada PC) |
| `*.user` | Archivos de configuración de usuario del proyecto |

```gitignore
# Archivos de compilación
bin/
obj/

# Configuración de Visual Studio
.vs/
*.user
*.suo
*.userosscache
*.sln.docstates

# Archivos de NuGet
*.nupkg
packages/
project.lock.json

# Archivos del sistema operativo
.DS_Store
Thumbs.db
desktop.ini

# Archivos de cobertura de tests
coverage/
*.coverage
*.coveragexml

# Logs
*.log
```

```bash
# Agregar y commitear el .gitignore
git add .gitignore
git commit -m "Agregar .gitignore para proyecto .NET"

# Verificar que los archivos ignorados ya no aparecen en git status
git status
```

---

> Unidad 3 - Programación 1 | UCSE
