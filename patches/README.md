# WASM patches

The C++ kernel in this deployment is built from three upstream repositories,
each at a pinned commit (see `CPPINTEROP_COMMIT`, `CPPJIT_COMMIT` and
`XEUS_CPP_COMMIT` in `.github/workflows/deploy.yml`) with the patches here
applied in order by `git am`.

| Prefix | Upstream | Pinned commit |
| --- | --- | --- |
| `CppInterOp-` | <https://github.com/compiler-research/CppInterOp> | `070b0a4` (main) |
| `cppjit-` | <https://github.com/compiler-research/cppjit> | `5a33027` |
| `xeus-cpp-` | <https://github.com/compiler-research/xeus-cpp> | `82f9064` (main, v0.10.0) |

Every patch is one commit, and the numbers are the order they apply in. They
carry only what a browser build needs: the wasm side-module build of cppjit, the
files the interpreter has to find inside the browser's virtual filesystem, the
`%%python` magic that bridges the two, and the loader fixes without which the
kernel dies on the first cell. Nothing CUDA-related and nothing from the LLVM 23
work is included -- neither is needed here, and CUDA cannot work in a browser at
all (no device, no toolkit in the emscripten sysroot).

`cppjit` is pinned behind its upstream main on purpose: upstream has since
reworked how CppInterOp is located and loaded (`dlopen` in a static initializer,
paths anchored on the loaded library via `dladdr`), which the wasm patches here
predate. Re-porting them onto that scheme is a separate piece of work; until
then this commit is the newest upstream commit the series applies to and has
been verified against.

## Regenerating

The patches are `git format-patch` output, so a series can be rebuilt by
replaying the commits onto a newer upstream and exporting again:

```bash
git clone https://github.com/compiler-research/xeus-cpp.git && cd xeus-cpp
git checkout -b wasm-series <new-upstream-commit>
git am /path/to/patches/xeus-cpp-*.patch      # or cherry-pick from a branch
git format-patch --no-signature --zero-commit <new-upstream-commit>..HEAD
```

Keep the numbering contiguous, and update the pinned commit in the workflow in
the same change.
