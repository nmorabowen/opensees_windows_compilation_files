# El Ladruno OpenSees Installer Note (Windows 11)

Last updated: 2026-03-08

## 1) Purpose

Create a redistributable installer from an already compiled OpenSees build (`build-win11`).

Branding used by default:

- Product name: `El Ladruno OpenSees`
- Publisher: `El Ladruno`

## 2) Inputs Required

You must already have a successful build in:

- `build-win11\OpenSees.exe`
- `build-win11\OpenSeesSP.exe`
- `build-win11\OpenSeesMP.exe`
- `build-win11\opensees.pyd`
- `build-win11\lib\...` (Tcl runtime)

## 3) Main Command

From repository root:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File SCRIPTS\create_el_ladruno_installer.ps1 `
  -BuildDir .\build-win11 `
  -OutputDir .\dist
```

## 4) Outputs

Generated in `dist\`:

- `ElLadrunoOpenSeesSetup_<version>_x64.exe` (Windows installer, if Inno Setup compiler is available)
- `ElLadrunoOpenSees_<version>_portable.zip` (portable package)
- `ElLadrunoOpenSees.iss` (Inno Setup script)
- `ElLadrunoOpenSees-stage\` (staging directory used for packaging)

## 5) Optional Flags

Useful options:

- `-SkipInnoCompile`: only stage + zip + `.iss` (no installer compilation)
- `-SkipZip`: skip portable zip generation
- `-IncludeAllExamples`: include full `EXAMPLES` tree
- `-IncludeExamples:$false`: package without examples
- `-AppVersion "3.7.2"`: force installer version
- `-AppName "El Ladruno OpenSees"`: change product name
- `-Publisher "El Ladruno"`: change publisher
- `-InnoCompilerPath "C:\Path\To\ISCC.exe"`: explicit Inno compiler path

## 6) Inno Setup Requirement

If `ISCC.exe` is not found, the script still produces:

- staging directory
- portable zip
- `.iss` script

To install Inno Setup using winget:

```powershell
winget install --id JRSoftware.InnoSetup -e --accept-source-agreements --accept-package-agreements
```

## 7) Install On Another Machine

For end users:

1. Run `ElLadrunoOpenSeesSetup_<version>_x64.exe`.
1. Launch from Start Menu shortcut `El Ladruno OpenSees`.

Important runtime note:

- Serial OpenSees runs from packaged files.
- `OpenSeesSP` and `OpenSeesMP` require a working MPI environment (`mpiexec`) on the target machine.

## 8) Recommended Validation After Install

On target machine:

```cmd
OpenSees-Launch.cmd
```

For MPI health:

```cmd
mpiexec -n 2 hostname
```

If MPI check fails, fix Intel MPI before testing SP/MP launchers.

## 9) Files This Chapter Depends On

To reproduce installer generation in another source tree, ensure these files exist at these paths:

- `SCRIPTS\create_el_ladruno_installer.ps1`
- `SCRIPTS\build_windows11_full.ps1` (provides launchers/runtime packaging)
- `SCRIPTS\fix_intel_mpi_windows11.ps1` (for MPI repair guidance in distributed docs)
- `vcpkg.json`
- `CMakeLists.txt`
- `cmake\cmake\OpenSeesDependenciesWin.cmake`
