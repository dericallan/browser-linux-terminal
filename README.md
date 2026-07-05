# Browser Linux Terminal

A real Linux kernel + BusyBox, booted with [v86](https://github.com/copy/v86)
(x86 emulated to WebAssembly) and running entirely client-side. There is no
backend: once the page and its assets are loaded, everything — the CPU, the
kernel, the shell — runs inside the visitor's own browser tab. That's what
makes it free to host: GitHub Pages just serves static files.

## What "limited configuration" means here

- The boot profile is fixed: one kernel image (`images/buildroot-bzimage68.bin`,
  a minimal [Buildroot](https://buildroot.org/) Linux built for v86's own test
  suite), one BIOS, no settings UI.
- The shell is BusyBox: a small, fixed set of applets (`ls`, `cat`, `vi`, `ps`,
  `echo`, ...), no package manager, no compiler.
- No virtual network device is configured, so there's no networking in/out of
  the guest.
- No persistent disk — state lives only in memory and resets on restart/reload.

## Files

```
index.html                        page + xterm.js + boot wiring
vendor/libv86.js, vendor/v86.wasm  the v86 emulator (from npm package "v86")
bios/seabios.bin, bios/vgabios.bin BIOS images (from the v86 repo)
images/buildroot-bzimage68.bin     the kernel+rootfs (from v86's own image CDN, i.copy.sh)
```

All assets are vendored in this repo so the page has no third-party runtime
dependency other than the xterm.js CDN bundle used for terminal rendering.

## Run locally

Any static file server works, e.g.:

```
python -m http.server 8020
```

then open http://localhost:8020/. (Opening `index.html` directly via `file://`
won't work — the assets are loaded via `fetch`, which requires http/https.)

## Deploy for free on GitHub Pages

1. Create a repo and push this folder (or push `linux-terminal/` as the repo root).
2. In the repo settings, enable **Pages** → deploy from the `main` branch, root folder.
3. GitHub gives you a `https://<user>.github.io/<repo>/` URL — that's it, no server to run or pay for.

## Credits / licenses

- [v86](https://github.com/copy/v86) — Simplified BSD License.
- [xterm.js](https://github.com/xtermjs/xterm.js) — MIT License, loaded from jsDelivr.
- Kernel image from v86's public test-image CDN (documented in the v86 `Readme.md`
  as the canonical way to fetch `buildroot-bzimage68.bin`).
