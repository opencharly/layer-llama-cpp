# layer-llama-cpp

llama.cpp prebuilt binaries and GGUF conversion tools, as a standalone OpenCharly
layer repo.

The candy downloads the latest llama.cpp release into `~/llama.cpp`: the
`llama-quantize` and `llama-cli` binaries with their shared libraries, plus the
`convert_hf_to_gguf.py` script and the `gguf-py` package for converting
HuggingFace models to GGUF. Every artifact lands at a fixed path under
`~/llama.cpp`, so its presence and executability are directly checkable.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `llama-cpp` |
| Install path | `~/llama.cpp` |
| Binaries | `llama-quantize`, `llama-cli`, `lib*.so*` |
| Python tools | `convert_hf_to_gguf.py`, `gguf-py` |
| Env | `LLAMA_CPP_PATH=~/llama.cpp`; `~/llama.cpp` appended to `PATH` |
| Service / port | none |

The selection walks releases newest-first and takes the first that actually
ships the `ubuntu-x64` binary, then uses the **same** release for the source GGUF
tools, so binaries and conversion tools never drift.

## How to use it

This is a Tier 1 "post-install" layer with no `pixi.toml`; compose it inside an
environment-owning candy or as a nested `candy:` list in a named box body:

```yaml
my-ml-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-llama-cpp:v2026.240.0201'
```

## Layout

- `charly.yml` — the `llama-cpp:` candy entity (the env/path vars, the download
  plan step, the `check:` assertions, and the embedded `llama-cpp-skill:` skill
  entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-jupyter:llama-cpp` — the binaries, the GGUF conversion
  tools, and the `LLAMA_CPP_PATH` env var.
- `/charly-jupyter:unsloth` — fine-tuning; depends on llama.cpp for GGUF conversion.
- `/charly-languages:python-ml` — composes this candy.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
