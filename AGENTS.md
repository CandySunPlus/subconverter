# Repository Guidelines

## Project Structure & Module Organization

`src/` contains the C++20 application code. Major areas are `src/parser/` for subscription parsing, `src/generator/` for output generation, `src/handler/` for request handling, `src/server/` for HTTP serving, `src/script/` for runtime integration, and `src/utils/` for shared helpers. Bundled headers live in `include/`, CMake modules in `cmake/`, runtime templates and rule lists in `base/`, and utility or packaging scripts in `scripts/`.

## Build, Test, and Development Commands

- `cmake -S . -B build -DCMAKE_BUILD_TYPE=Release`: configure a normal release build.
- `cmake --build build -j$(nproc)`: compile the `subconverter` executable.
- `cmake -S . -B build-static -DBUILD_STATIC_LIBRARY=ON`: configure the static library variant used by embedders.
- `./build/subconverter -f base/pref.example.yml`: run locally with the example preferences.
- `docker build -f scripts/Dockerfile -t subconverter .`: build the Docker image used by the packaging flow.

Dependencies include CMake, a C++20 compiler, pkg-config, libcurl, yaml-cpp, PCRE2, RapidJSON, toml11, QuickJS, and libcron.

## Coding Style & Naming Conventions

Match the existing C++ style: 4-space indentation, braces on their own line for functions and control blocks, compact condition spacing such as `if(!retVal)`, and descriptive camelCase or PascalCase identifiers. Keep headers beside their implementation domain, prefer existing utilities in `src/utils/`, and avoid new bundled dependencies unless CMake discovery and licensing are clear.

## Testing Guidelines

There is no dedicated test suite in this repository. Before submitting changes, run a clean CMake configure and build. For parser, generator, or config changes, also run the binary against representative URLs or local subscription files and compare generated output for affected targets such as `clash`, `surge`, `quanx`, or `singbox`. Include manual verification steps in the PR.

## Commit & Pull Request Guidelines

Recent history uses short imperative or descriptive subjects, often with prefixes such as `feat:` and optional PR references, for example `feat: add custom User-Agent support via diyua parameter` or `Fix icon-url not correctly appended to generated Surge configs`. Keep commits focused and explain user-visible behavior changes.

Pull requests should include a concise summary, linked issue when applicable, build results, and before/after examples for conversion output changes. Note new dependencies, configuration keys, Docker changes, or rule/template updates.

## Security & Configuration Tips

Do not commit personal tokens, subscription URLs, or generated `gistconf.ini` secrets. Use `base/pref.example.yml` as the safe reference configuration and keep local credentials in ignored files or environment-specific deployment settings.
