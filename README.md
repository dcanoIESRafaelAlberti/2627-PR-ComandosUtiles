# 2627-PR - Comandos y conceptos útiles para trabajar con Python y Git

## Comprobación del ejecutable de Python que estamos ejecutando

Para comprobar que ejecutable de `python` estáis ejecutando en realidad en la Terminal, podéis ejecutar lo siguiente:

```powershell
python -c "import sys; print(sys.executable)"
```

También podéis comprobar qué `python` encuentra PowerShell:

```powershell
Get-Command python
```

y, si quieres ver todas las coincidencias disponibles en el `PATH`:

```powershell
Get-Command python -All
```

Otra posibilidad clásica de Windows:

```powershell
where.exe python
```

Con el `.venv` activo probablemente verás primero:

```text
C:\...\.venv\Scripts\python.exe
```

y después puede aparecer el Python global instalado en el equipo.

## ¿Qué es un entorno virtual?

Un **entorno virtual de Python** es un entorno aislado para un proyecto que dispone de su propio intérprete y de sus propios paquetes instalados.

Por ejemplo, podemos tener:

```text
Proyecto A → Python 3.14 + pytest 9.x
Proyecto B → Python 3.13 + pytest 8.x
```

De esta forma, las librerías instaladas para un proyecto **no interfieren con las de otros proyectos ni con la instalación global de Python**.

Por eso es recomendable crear un entorno `.venv` para cada proyecto:

```bash
python -m venv .venv
```

Una vez activado, los paquetes que instalemos con `pip` quedarán asociados a ese entorno virtual.

## Crear el entorno virtual desde Terminal (Git Bash)

Si primero has creado el entorno:

```bash
python3 -m venv .venv
```

lo activas con:

```bash
source .venv/bin/activate
```

Verás normalmente:

```text
(.venv) alumno@equipo:~/ProgPython$
```

Para comprobar la versión instalada:

```bash
./.venv/Scripts/python.exe --version
```

### 1. Forma básica

Desde la carpeta del proyecto:

```bash
python -m venv .venv
```

Significa:

```text
python        → ejecuta el intérprete Python disponible
-m venv       → ejecuta el módulo estándar venv
.venv         → carpeta donde se creará el entorno virtual
```

El entorno se crea utilizando **la misma versión de Python que estamos ejecutando con `python`**.

Antes de crearlo podemos comprobarla:

```bash
python --version
```

y comprobar exactamente qué ejecutable estamos utilizando:

```bash
python -c "import sys; print(sys.executable)"
```

Por ejemplo:

```text
Python 3.14.0
C:\Users\...\AppData\Local\Programs\Python\Python314\python.exe
```

Entonces:

```bash
python -m venv .venv
```

creará `.venv` utilizando ese Python 3.14.

### 2. Si tenemos varias versiones de Python en Windows: `py`

Windows dispone del launcher `py`, que permite seleccionar explícitamente una instalación de Python.

Para ver las versiones disponibles:

```powershell
py -0p
```

Por ejemplo, podría mostrar:

```text
-V:3.14 *  C:\...\Python314\python.exe
-V:3.13    C:\...\Python313\python.exe
-V:3.12    C:\...\Python312\python.exe
```

El `*` indica la versión que utilizaría `py` por defecto.

También podemos comprobar simplemente:

```powershell
py --version
```

### 3. Crear el entorno con una versión concreta

Por ejemplo, con Python 3.14:

```powershell
py -3.14 -m venv .venv
```

Con Python 3.13:

```powershell
py -3.13 -m venv .venv
```

### 4. Eliminar un entorno virtual

Un entorno virtual no necesita desinstalarse. Para eliminarlo basta con **desactivarlo y borrar la carpeta `.venv`**.

Primero, si está activo:

```bash
deactivate
```

Después eliminamos la carpeta según la terminal utilizada:

| Sistema / Terminal | Comando |
|---|---|
| **Linux** | `rm -rf .venv` |
| **Git Bash en Windows** | `rm -rf .venv` |
| **PowerShell en Windows** | `Remove-Item -Recurse -Force .venv` |
| **CMD en Windows** | `rmdir /s /q .venv` |

Por ejemplo, en Linux o Git Bash:

```bash
deactivate
rm -rf .venv
```

En PowerShell:

```powershell
deactivate
Remove-Item -Recurse -Force .venv
```

En CMD:

```cmd
deactivate
rmdir /s /q .venv
```

Esto **no elimina Python ni los archivos del proyecto**. Solo elimina el entorno virtual y los paquetes que estuvieran instalados dentro de él.

## Activar/Desactivar entorno virtual (.venv)

| Entorno | Activar | Desactivar |
|---|---|---|
| **Linux** | `source .venv/bin/activate` | `deactivate` |
| **Git Bash en Windows** | `source .venv/Scripts/activate` | `deactivate` |
| **PowerShell en Windows** | `.\.venv\Scripts\Activate.ps1` | `deactivate` |
| **CMD en Windows** | `.venv\Scripts\activate.bat` | `deactivate` |

En Windows puede ocurrir que PowerShell impida activar el entorno por la política de ejecución:

```powershell
.\.venv\Scripts\Activate.ps1
```

mostrando un error indicando que **la ejecución de scripts está deshabilitada**.

**Podemos consultar la política actual con:**

```powershell
Get-ExecutionPolicy
```

Una solución habitual para el usuario actual es ejecutar:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

y confirmar el cambio cuando PowerShell lo solicite.

Después podremos activar normalmente:

```powershell
.\.venv\Scripts\Activate.ps1
```

`RemoteSigned` permite ejecutar scripts creados localmente y exige que los scripts descargados de Internet estén firmados.

Para comprobar las políticas aplicadas en los distintos ámbitos:

```powershell
Get-ExecutionPolicy -List
```

**Importante en los PCs del centro:** usar `CurrentUser` evita modificar la política para todos los usuarios del equipo. Además, si el centro aplica una política mediante administración del sistema o directivas de grupo, esta puede tener prioridad y no conviene intentar eludirla.

En resumen, el cambio corresponde a `UsuarioT`. No estás estableciendo `RemoteSigned` globalmente para `Alberti` y `UsuarioM`.

## Git - Rama principal: `master` o `main`

Tradicionalmente, Git utilizaba **`master`** como nombre predeterminado para la rama principal de un nuevo repositorio. Actualmente, **`main` es la convención más extendida** y es también el nombre predeterminado utilizado por GitHub para los nuevos repositorios.

Para evitar trabajar con `master` en local y `main` en GitHub, es recomendable configurar Git para utilizar `main` desde el principio.

Podemos comprobar nuestra configuración con:

```bash
git config --global init.defaultBranch
```

Para establecer `main` como rama inicial por defecto:

```bash
git config --global init.defaultBranch main
```

Esta configuración se realiza **una sola vez por usuario**. A partir de entonces:

```bash
git init
```

creará los nuevos repositorios utilizando `main` como rama principal.

## Git - Archivo `.gitignore`

El archivo `.gitignore` indica a Git qué archivos o directorios **no queremos incluir en el repositorio**. En un proyecto Python podemos utilizar, por ejemplo:

```gitignore
.venv/
__pycache__/
*.pyc
.vscode/
.idea/
.DS_Store
```

Cada entrada tiene una finalidad:

- **`.venv/`** → entorno virtual de Python. Contiene el intérprete y los paquetes instalados específicamente para ese entorno. **No debe versionarse**, ya que puede recrearse en cada equipo. En versiones modernas de Python, `venv` crea además su propio `.gitignore` dentro de `.venv` con `*`, por lo que su contenido ya queda ignorado por Git. Aun así, mantener `.venv/` en el `.gitignore` del proyecto es explícito y hace la configuración independiente de la versión de Python utilizada.

- **`__pycache__/`** → directorios que Python crea automáticamente para almacenar código compilado en bytecode y acelerar la carga de módulos. Se pueden regenerar automáticamente y, por tanto, no tiene sentido almacenarlos en Git.

- **`*.pyc`** → archivos que contienen *bytecode* generado por Python a partir del código fuente. CPython puede utilizarlos para evitar recompilar determinados módulos cuando no han cambiado. Se generan automáticamente y no necesitamos almacenarlos en Git.

- **`.vscode/`** → configuración específica de **Visual Studio Code** para el proyecto: preferencias, configuraciones de ejecución, extensiones recomendadas, etc. Para estas primeras prácticas es razonable ignorarla para evitar subir configuraciones particulares del equipo de cada alumno. En proyectos reales, algunos archivos de `.vscode` sí pueden compartirse deliberadamente con el equipo.

- **`.idea/`** → directorio utilizado por los IDE de **JetBrains**, como PyCharm o IntelliJ IDEA, para almacenar configuración del proyecto. En estas prácticas evitamos versionarlo porque contiene principalmente configuración del IDE.

- **`.DS_Store`** → archivo que crea automáticamente **macOS Finder** para almacenar información sobre la visualización de una carpeta (posición de iconos, vistas, etc.). No forma parte del proyecto y no debe versionarse.

La idea que interesa transmitirles es que `.gitignore` **no significa «archivos que Git no puede guardar»**, sino:

> **Archivos que existen en nuestro directorio de trabajo, pero hemos decidido que no forman parte del código o contenido que queremos versionar.**

Y una observación importante: `.gitignore` actúa normalmente sobre archivos **que todavía no están siendo seguidos por Git**. Si previamente hemos hecho `git add`/`commit` de un archivo, añadirlo posteriormente a `.gitignore` no hace que Git deje automáticamente de seguirlo.

## ¿Python es un lenguaje interpretado?

Sí, pero aquí hay un matiz importante: decir que **Python es interpretado** es correcto como simplificación, pero **CPython no ejecuta directamente el código fuente `.py` línea por línea**.

`Python` es el lenguaje y `CPython` es la implementación estándar que normalmente instalamos y utilizamos para ejecutar Python. **CPython** transforma el código en bytecode que después ejecuta su máquina virtual.

El proceso habitual de CPython es:

```text
Código fuente Python
     programa.py
          │
          ▼
     compilación
          │
          ▼
       BYTECODE
          │
          ▼
Máquina virtual de Python
          │
          ▼
      ejecución
```

Por ejemplo, escribimos:

```python
x = 5
print(x)
```

CPython transforma internamente esas instrucciones en **bytecode**, unas instrucciones intermedias que entiende la máquina virtual de Python.

### ¿Y los `.pyc`?

Python puede guardar ese bytecode compilado para reutilizarlo posteriormente. De ahí aparecen directorios como:

```text
__pycache__/
```

y dentro archivos como:

```text
modulo.cpython-314.pyc
```

Ese `.pyc` contiene **bytecode**, no código máquina del procesador.

Por eso lo ponemos en `.gitignore`:

```gitignore
__pycache__/
*.pyc
```

porque Python puede regenerarlo cuando sea necesario.

### Entonces, ¿Python es compilado o interpretado?

Aquí está lo interesante. Con la implementación habitual, **CPython**, ocurren ambas cosas:

```text
.py
 │
 │ compilación
 ▼
bytecode
 │
 │ interpretación por la VM de Python
 ▼
ejecución
```

Por eso solemos clasificar Python como un **lenguaje interpretado**, porque el programador normalmente ejecuta:

```bash
python programa.py
```

sin realizar previamente un paso explícito de compilación para generar un ejecutable nativo.

Pero internamente **sí existe una fase de compilación a bytecode**.

Es bastante parecido conceptualmente a algo que tus alumnos verán después con Java/Kotlin:

```text
Java/Kotlin                   Python (CPython)

código fuente                 código fuente
     ↓                             ↓
 bytecode                       bytecode
     ↓                             ↓
    JVM                     VM de Python
     ↓                             ↓
 ejecución                       ejecución
```

Aunque hay diferencias importantes entre ambas plataformas, esta comparación les puede venir muy bien cuando pasemos de Python a Kotlin.

Y un detalle interesante: **no todos los `.py` que ejecutas producen necesariamente un `.pyc` visible al lado**. El mecanismo de caché se observa especialmente con **módulos importados**, así que si quieres demostrárselo en clase conviene preparar un `import` sencillo.

## Git - Manejo básico de `stash`

`stash` permite **guardar temporalmente cambios pendientes sin crear un commit**, dejando limpio el directorio de trabajo para poder realizar otras tareas.

Guardar los cambios actuales:

```bash
git stash push -m "Cambio provisional"
```

Consultar los cambios guardados:

```bash
git stash list
```

Por ejemplo:

```text
stash@{0}: On main: Cambio provisional
stash@{1}: On main: Otro cambio
```

Recuperar un cambio guardado **sin eliminarlo del stash**:

```bash
git stash apply "stash@{0}"
```

Una vez comprobado que se ha recuperado correctamente, eliminarlo:

```bash
git stash drop "stash@{0}"
```

También podemos aplicar el último `stash` y eliminarlo directamente:

```bash
git stash pop
```

Si solo queremos recuperar el último `stash` sin eliminarlo, no es necesario indicar su identificador:

```bash
git stash apply
```

**Importante en PowerShell:** conviene escribir referencias como `stash@{0}` entre comillas:

```powershell
git stash apply "stash@{0}"
git stash drop "stash@{0}"
```

PowerShell interpreta `@{...}` con un significado propio y, sin las comillas, puede procesar incorrectamente el comando antes de pasárselo a Git. Las comillas también funcionan correctamente en Linux y Git Bash, por lo que podemos utilizarlas siempre.

**IMPORTANTE**, **temporalmente tienes dos copias de esos cambios**, y es intencionado.

Cuando haces:

```bash
git stash apply "stash@{0}"
```

Git hace conceptualmente esto:

```text
ANTES

Directorio de trabajo       STASH
       limpio          →    Cambio provisional


DESPUÉS DE apply

Directorio de trabajo       STASH
 Cambio provisional    ←    Cambio provisional
                               ↑
                         sigue guardado
```

`apply` significa literalmente **«aplica estos cambios»**, no «sácalos del stash». Git conserva la copia como medida de seguridad. Así puedes comprobar que se ha aplicado correctamente y, si estás satisfecho, eliminarla:

```bash
git stash drop "stash@{0}"
```
