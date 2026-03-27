# OpenSees Windows 11 -- Quick Start

Compile any OpenSees source tree on Windows 11 by running 4 scripts.

## Step 1: Install tools (one time, needs admin)

Open PowerShell as Administrator:

```powershell
git clone https://github.com/nmorabowen/opensees_windows_compilation_files.git C:\work\harness
cd C:\work\harness
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\1_install_dependencies.ps1
```

Installs: Git, Visual Studio 2022, Intel oneAPI (ifx, MKL, MPI), CMake, Ninja, Python 3.11.

## Step 2: Fetch source (normal PowerShell)

```powershell
cd C:\work\harness
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\2_fetch_source.ps1 `
  -OpenSeesRepo "https://github.com/OpenSees/OpenSees.git"
```

To build a different fork or branch:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\2_fetch_source.ps1 `
  -OpenSeesRepo "https://github.com/YOUR_USER/OpenSees.git" `
  -OpenSeesBranch "your-branch"
```

## Step 3: Build

```powershell
cd C:\work\OpenSees-src
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\3_build.ps1
```

## Step 4: Package (optional)

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\4_package.ps1
```

## What you get

In `build-win11\`:

| File | What it is |
|------|-----------|
| `OpenSees.exe` | Serial Tcl interpreter |
| `OpenSeesSP.exe` | Parallel single-program (MPI) |
| `OpenSeesMP.exe` | Parallel multi-program (MPI) |
| `opensees.pyd` | Python 3.11 module |
| `OpenSees-Launch.cmd` | Double-click to run serial OpenSees |
| `OpenSeesSP-Launch.cmd` | Runs SP with mpiexec |
| `OpenSeesMP-Launch.cmd` | Runs MP with mpiexec |

## Run it

```cmd
build-win11\OpenSees-Launch.cmd
```

## Something went wrong?

| Problem | Fix |
|---------|-----|
| `ifx` not found | `VS2022INSTALLDIR` not set. Run `call SCRIPTS\init_oneapi_windows11.cmd` first |
| `init.tcl` not found | Use the `.cmd` launchers, not the `.exe` directly |
| MPI fails | Run `mpiexec -n 2 hostname`. If it fails, run `SCRIPTS\fix_intel_mpi_windows11.ps1` as admin |

Full details: `WINDOWS11_BUILD_ANY_OPENSEES_SOURCE.md`
