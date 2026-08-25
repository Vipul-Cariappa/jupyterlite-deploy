# My Instance of JupyterLite

A JupyterLite site deployed to GitHub Pages, built from `jupyterlite/demo`.

## Kernels

| Kernel | Comes from |
| --- | --- |
| C++23, C23 | xeus-cpp, built here for `emscripten-wasm32` with CppJIT for Python interop |
| Python | xeus-python, plus numpy/pandas/matplotlib and friends |
| Python (Pyodide) | jupyterlite-pyodide-kernel |
| JavaScript, P5 | jupyterlite-javascript-kernel, jupyterlite-p5-kernel |
| SQLite, Lua | xeus-sqlite, xeus-lua |
| KariLang | KariLang-Kernel, built here |

`cppjit-demo.ipynb` is the C++ kernel's tour: C++ cells that compile and run in
the browser, then `%%python` cells that call into that same C++ through CppJIT --
including a Python class that inherits from a C++ class and overrides a virtual
method, which C++ then calls back into.

The C++ kernel is compiled from three upstream repositories at pinned commits
with the patches in `patches/` applied; see `patches/README.md`. `environment.yml`
describes the wasm environment every kernel comes from, `build-environment.yml`
the native tooling that assembles the site.
