# 2627-PR-ComandosUtiles

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
