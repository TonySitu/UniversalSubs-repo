# CI: patched ProcTap wheels

`build-proctap-wheels.yml` builds our patched ProcTap (per-app audio capture,
with the wrong Windows-10 build gate removed) for Python 3.9–3.13 on 64-bit
Windows, using cibuildwheel on GitHub's free runners.

## Why
ProcTap's official wheel is published for limited Python versions and carried
a build-gate bug that broke per-app capture on Windows 10 2004–20H2. We patch
the source (in `../proctap-src/`) and build our own wheels so every supported
Python gets working per-app capture.

## How to run it
- **On demand:** Actions tab → "Build patched ProcTap wheels" → Run workflow.
- **On a tag:** push a tag like `proctap-v1.0.3+us.1` and it builds + attaches
  the wheels to a GitHub Release automatically.

## After it runs
1. Download the `proctap-wheels` artifact (or grab them from the Release).
2. Drop all the `.whl` files into the repo's `wheels/` folder.
3. Commit. Now `install_and_run.bat` finds a matching wheel for whatever
   Python the user has (3.9–3.13), and per-app capture "just works" without
   pinning anyone to 3.11.

## Updating the patched source
If upstream ProcTap changes and you want the newer version, re-apply the
one-line gate removal in `proctap-src/src/proctap/_native.cpp` (delete the
`dwBuildNumber` version-check block) and re-run the workflow.
