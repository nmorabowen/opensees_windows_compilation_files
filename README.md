# OpenSees Windows Compilation Files

Compile **any** OpenSees source tree on Windows 11 in four steps.

Produces: `OpenSees.exe`, `OpenSeesSP.exe`, `OpenSeesMP.exe`, `opensees.pyd`

## Quick Start (4 scripts, in order)

```powershell
# 1. Install dependencies (elevated PowerShell -- run once)
git clone https://github.com/nmorabowen/opensees_windows_compilation_files.git C:\work\harness
cd C:\work\harness
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\1_install_dependencies.ps1

# 2. Fetch source + build harness (normal PowerShell)
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\2_fetch_source.ps1 `
  -OpenSeesRepo "https://github.com/OpenSees/OpenSees.git"

# 3. Build (from the source root created by step 2)
cd C:\work\OpenSees-src
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\3_build.ps1

# 4. Package into portable zip / Windows installer (optional)
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\4_package.ps1
```

Each script prints the next step to run when it finishes.

## What Each Script Does

| Script | Needs Admin? | Purpose |
|--------|:---:|---------|
| `1_install_dependencies.ps1` | Yes | Installs VS2022, Intel oneAPI, CMake, Ninja, Python 3.11, Git via `winget` |
| `2_fetch_source.ps1` | No | Clones OpenSees source, vcpkg, MUMPS, and this repo; copies build harness into the source tree |
| `3_build.ps1` | No | Configures + compiles all four targets with vcpkg, MKL, Intel MPI |
| `4_package.ps1` | No | Stages artifacts into a portable zip and/or Inno Setup installer |

All scripts support `-DryRun` to preview commands without executing.

## Customization

Build your own fork:
```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\2_fetch_source.ps1 `
  -OpenSeesRepo "https://github.com/YOUR_USER/OpenSees.git" `
  -OpenSeesBranch "my-branch" `
  -WorkDir "D:\my-builds"
```

Change installer branding:
```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\4_package.ps1 `
  -AppName "My OpenSees" -Publisher "My Lab"
```

## Repository Contents

### Numbered workflow scripts
- `SCRIPTS/1_install_dependencies.ps1` — prerequisite installer with verification
- `SCRIPTS/2_fetch_source.ps1` — source + harness fetcher
- `SCRIPTS/3_build.ps1` — build wrapper
- `SCRIPTS/4_package.ps1` — packaging wrapper

### Build harness (copied into target source tree by step 2)
- `CMakeLists.txt` — cross-platform CMake build system
- `vcpkg.json` — pinned dependency manifest (tcl 8.6.10, hdf5 1.14.6, eigen3 3.4.1, zlib 1.3.1, libaec 1.1.4)
- `cmake/cmake/OpenSeesDependencies.cmake` — unified dependency discovery (all platforms)
- `cmake/cmake/OpenSeesDependenciesWin.cmake` — legacy Windows-specific (deprecated)

### Core scripts (used by the numbered wrappers)
- `SCRIPTS/build_windows11_full.ps1` — full build orchestrator
- `SCRIPTS/create_el_ladruno_installer.ps1` — installer/zip packager
- `SCRIPTS/fix_intel_mpi_windows11.ps1` — Intel MPI repair (admin)
- `SCRIPTS/init_oneapi_windows11.cmd` — oneAPI VS2022 init wrapper

### Source-code patches (MSVC linker fixes)
- `SRC/element/UWelements/Tcl_generateInterfacePoints.cpp`
- `SRC/modelbuilder/tcl/myCommands.cpp`

### Documentation
- `WINDOWS11_BUILD_RUNBOOK.md` — detailed runbook for this repo's build
- `WINDOWS11_BUILD_ANY_OPENSEES_SOURCE.md` — portable guide for any fork
- `WINDOWS11_INSTALLER_EL_LADRUNO.md` — installer packaging guide

## Requirements

- Windows 11 (x64)
- Visual Studio 2022 with C++ workload
- Intel oneAPI Base Toolkit (MKL) + HPC Toolkit (ifx, Intel MPI)
- CMake >= 3.29, Ninja, Python 3.11, Git

All installed automatically by `1_install_dependencies.ps1`.

## Notes

- Python 3.11 is pinned for `OpenSeesPy` ABI compatibility
- MPI launchers assume Intel MPI on Windows
- The build system is cross-platform — `CMakeLists.txt` also works on Linux/macOS
- Tcl is provided by vcpkg (no system Tcl install needed)
