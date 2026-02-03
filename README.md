# moon-clib-packages

Package repository for C library manifests used by tools like `moon-clib`.

Each package version lives under `{name}/{version}/manifest.json` and describes how to fetch, build, and install the library.

## Repository Layout

```text
{repo}/
  {name}/
    {version}/
      manifest.json
      ... optional patches/files ...
```

## Current Packages

| Package | Versions | License |
| --- | --- | --- |
| openssl | 3.2.0 | Apache-2.0 |
| tdlib | 1.8.0 | Boost-1.0 |
| zlib | 1.3.1 | Zlib |

## Manifest Overview

`manifest.json` is a declarative build recipe with a few common fields:

- `license`: SPDX identifier for the library.
- `homepage`: Optional upstream project URL.
- `source`: One or more source entries.
  - `type`: `tarball` or `git`.
  - `url`: Upstream source URL.
  - `ref`: Git ref or tag (for `git`).
  - `strip_components`: Tarball extraction prefix stripping.
  - `dest`: Directory name to place the extracted source.
- `dependencies`: Map of package name to version.
- `build`: Ordered build steps. Each step can set:
  - `run`: Array of command + args.
  - `cwd`: Working directory for the step.
  - `when`: Optional platform selector (e.g. `{ "os": "linux", "arch": "x86_64" }`).

Build steps can reference a few environment variables commonly used by the tooling:

- `PREFIX`: Install prefix path for the package.
- `DESTDIR`: Staging root used during install.
- `JOBS`: Parallel build jobs count.

## Adding or Updating a Package

1. Create a new `{name}/{version}/manifest.json` directory.
2. Keep versions immutable: add new versions instead of editing old ones.
3. Pin all sources and dependencies to exact versions.
4. Keep the manifest deterministic (no network access during `build` other than fetching `source`).

## License

Apache-2.0
