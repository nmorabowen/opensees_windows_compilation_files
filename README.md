# OpenSees Windows Compilation Files

This repository is a reusable Windows 11 file pack for compiling OpenSees source trees with:

- `OpenSees`
- `OpenSeesSP`
- `OpenSeesMP`
- `OpenSeesPy`

It contains the files that need to be copied into another OpenSees source tree to reuse the Windows build, MPI repair, and installer workflow.

## Included Files

- `SCRIPTS/build_windows11_full.ps1`
- `SCRIPTS/init_oneapi_windows11.cmd`
- `SCRIPTS/fix_intel_mpi_windows11.ps1`
- `SCRIPTS/create_el_ladruno_installer.ps1`
- `vcpkg.json`
- `CMakeLists.txt`
- `cmake/cmake/OpenSeesDependenciesWin.cmake`
- `WINDOWS11_BUILD_RUNBOOK.md`
- `WINDOWS11_BUILD_ANY_OPENSEES_SOURCE.md`
- `WINDOWS11_INSTALLER_EL_LADRUNO.md`

## Start Here

- Use `WINDOWS11_BUILD_ANY_OPENSEES_SOURCE.md` if you want to apply this workflow to another OpenSees source tree.
- Use `WINDOWS11_BUILD_RUNBOOK.md` if you want the exact workflow used in the source repository this pack came from.
- Use `WINDOWS11_INSTALLER_EL_LADRUNO.md` if you want to package a built tree into a portable zip or Windows installer.

## Typical Use

1. Clone your target OpenSees source tree.
2. Copy these files into that tree, preserving the same relative paths.
3. Run the prerequisite install commands in `WINDOWS11_BUILD_ANY_OPENSEES_SOURCE.md`.
4. Build with `SCRIPTS/build_windows11_full.ps1`.
5. Optionally package with `SCRIPTS/create_el_ladruno_installer.ps1`.

## Notes

- The current workflow is pinned to Python `3.11` for `OpenSeesPy`.
- MPI launchers assume Intel MPI on Windows.
- The batch helper `SCRIPTS/init_oneapi_windows11.cmd` is included to avoid the Visual Studio detection warning seen on some oneAPI installs.
