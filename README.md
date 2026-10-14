# WASTE

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-waste_management-lightgrey)

> Anticloud-hardened packaging of the upstream project `WASTE` in category **WASTE MANAGEMENT**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** WASTE MANAGEMENT · **Upstream:** https://github.com/Richienb/ros-data-waster · **Upstream pin:** `0aeed8891184d496b0b6e714264af7fa970d4a63` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

# <img width="24px" height="24px" style="position: relative; top: 2px;" src="assets/favicon.png"/> wastebin

[![Rust](https://github.com/matze/wastebin/actions/workflows/rust.yml/badge.svg)](https://github.com/matze/wastebin/actions/workflows/rust.yml)

## <strong><a href="https://war.ukraine.ua/support-ukraine/">support 🇺🇦</a> • <a href="https://commission.europa.eu/strategy-and-policy/state-union/state-union-2022/defending-eu-values">defend 🇪🇺</a></strong>

A minimal pastebin with a design shamelessly copied from
[bin](https://github.com/WantGuns/bin).

<p align="center"><img src="https://raw.githubusercontent.com/matze/wastebin/master/assets/screenshot.webp"></p>

<p align="center"><strong><a href="https://bin.bloerg.net">DEMO</a></strong> (resets every day) • <strong><a href="https://matze.github.io/wastebin/">THEMES</a></strong></p>

You are reading the documentation for an **unreleased version**. You can refer
to released versions here:

**[3.8.0](https://github.com/matze/wastebin/tree/3e2f1425aa4aa8072da79b2a8c0c6af75074efa6)**
•
[3.7.2](https://github.com/matze/wastebin/tree/dfca1dc82b78394d8e2dfe209d9ef3fb539da605)
•
[3.6.2](https://github.com/matze/wastebin/tree/f9cb25a4d5b19fa938eef0af42f68f2835f17b98)
•
[3.5.0](https://github.com/matze/wastebin/tree/d379a6e3e73e6f1fcf23f93c9b0cc857f46acbce)
•
[3.4.1](https://github.com/matze/wastebin/tree/c9d717329a6e357e8a13a324bfa9a53d41ae9b35)
•
[3.3.0](https://github.com/matze/wastebin/tree/a297749b932ed9ff32569f3af7ee8e4a5b499834)
•
[3.2.0](https://github.com/matze/wastebin/tree/3fdec3abde4f32b92323864ffea51577ce1e625e)
•
[3.1.0](https://github.com/matze/wastebin/tree/e404ecec61eaafa1187b8d6b45282d72b076563d)
•
[3.0.0](https://github.com/matze/wastebin/tree/14a30bb540110e76da6a6045cd0e83fd2218cdd7)
•
[2.7.1](https://github.com/matze/wastebin/tree/85a519ef9079c4618f851cce575b5a84334a6f42)

## Features

* [axum](https://github.com/tokio-rs/axum) and [sqlite3](https://www.sqlite.org)
  backend
* comes as a single binary with low memory footprint
* compresses pastes using [zstd](https://github.com/facebook/zstd)
* syntax highlighting for > 170 languages with
  [syntect](https://github.com/trishume/syntect)
* renders Markdown pastes to HTML, including GitHub-flavored tables, task lists
  and admonitions
* comes with [nine color themes](https://matze.github.io/wastebin/) in light and
  dark mode
* encrypts entries using ChaCha20Poly1305 and argon2 hashed passwords
* allows deletion after expiration, after reading or by anonymous owners
* shows QR code to browse a paste's URL on mobile devices
* translated into Chinese and German

### Non-features

* user authentication and admin functionality
* arbitrary file uploads
* mitigations for all kinds of DoS attack vectors

> [!CAUTION]
> Due to lack of authentication and further DoS mitigations, it is not advised
> to run wastebin facing the internet *as is*. If you plan to do so, you are
> strongly advised to rate limit inbound requests via iptables rules or a
> properly configured reverse proxy of your choice.

## Installation

### Run pre-built binaries

You can download pre-built, statically compiled [Linux and MacOS
binaries](https://github.com/matze/wastebin/releases). After extraction run the
contained `wastebin` binary.

### Run a Docker image

Alternatively, you can run a pre-built Docker image pushed to
`quxfoo/wastebin:<VERSION>` and `quxfoo/wastebin:latest` respectively. To
persist the database as `state.db` via the `WASTEBIN_DATABASE_PATH` environment
variable use a bind mount to `/path/for/storage` like this

```bash
docker run \
    -e WASTEBIN_DATABASE_PATH=/data/state.db \
    -v /path/for/storage:/data \
    -u $(id -u):$(id -g) \
    quxfoo/wastebin:latest
```

> [!NOTE]
> The image is based on scratch which means it neither comes with a shell nor
> with `TMPDIR` being set. If database migrations fail with an extended sqlite
> error code 6410, pass `TMPDIR` pointing to a location sqlite can write to.

### Run with docker-compose

```yaml
services:
  wastebin:
    restart: always
    environment:
      - WASTEBIN_DATABASE_PATH=/data/state.db
    ports:
      - "8088:8088"
    volumes:
      - './data:/data'
    image: 'quxfoo/wastebin:latest'
```

Make sure the `./data` folder is writable by the user 10001.

### Run with Nix

For Nix users, a `flake.nix` is also provided. Build and execute it directly
with:

```bash
nix run 'github:matze/wastebin#wastebin'
```

Or install the provided `wastebin` package like you normally would.

### Build from source

Install a Rust 2024 toolchain containing Rust 1.85 with
[rustup](https://rustup.rs) and run the server binary with

```bash
cargo run --release
```

### Build a container image

It is possible to build a container image using Docker or Podman. The
`Dockerfile` is designed to be run on an x86_64 host but capable of building
images for both x86_64 and aarch64 via the `--target` flag:

```bash
# Docker
docker build -t wastebin:v3.0.0 -f Dockerfile --target amd64 .
docker build -t wastebin:v3.0.0 -f Dockerfile --target arm64 .

# Podman
podman build -t wastebin:v3.0.0 -f Dockerfile --target amd64
podman build -t wastebin:v3.0.0 -f Dockerfile --target arm64
```

Note that you *cannot* build aarch64 images on aarch64 hosts with it.

To interact with a running wastebin instance the bundled `wastebin-ctl` tool can
be used, e.g.:

```bash
podman exec -e RUST_LOG=debug -it wastebin /app/wastebin-ctl
```

## Usage

### Browser interface

When viewing a paste, you can use

* <kbd>r</kbd> to view the raw paste,
* <kbd>n</kbd> to go the index page,
* <kbd>y</kbd> to copy the current URL to the clipboard,
* <kbd>c</kbd> to copy the content to the clipboard,
* <kbd>q</kbd> to display the current URL as a QR code,
* <kbd>p</kbd> to view the formatted paste,
* <kbd>w</kbd> to toggle line wrapping on and off (off by default),
* <kbd>m</kbd> to toggle the rendered view on Markdown pastes and
* <kbd>?</kbd> to view the list of keybindings.

To paste some

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Rust / Cargo** (manifests: Cargo.toml, Cargo.lock; scanned in UPSTREAM_CLONE)
- Top-level source layout: `assets/`, `crates/`
- Snapshot size: **91 files**, **8162 lines of code** (measured; see Benchmarks)
- Primary languages: `.rs` (39), `.html` (11), `.toml` (9), `.sql` (7), `(none)` (4), `.js` (4)
- Upstream commit pinned for this packaging: `0aeed8891184d496b0b6e714264af7fa970d4a63`

---

## Installation

No installation section was found in the upstream readme, so the commands below are generated from the manifests detected in this project directory.

```sh
cargo build --release
```

Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

```bash
docker run \
    -e WASTEBIN_DATABASE_PATH=/data/state.db \
    -v /path/for/storage:/data \
    -u $(id -u):$(id -g) \
    quxfoo/wastebin:latest
```

> [!NOTE]
> The image is based on scratch which means it neither comes with a shell nor
> with `TMPDIR` being set. If database migrations fail with an extended sqlite
> error code 6410, pass `TMPDIR` pointing to a location sqlite can write to.

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

to released versions here:

**[3.8.0](https://github.com/matze/wastebin/tree/3e2f1425aa4aa8072da79b2a8c0c6af75074efa6)**
•
[3.7.2](https://github.com/matze/wastebin/tree/dfca1dc82b78394d8e2dfe209d9ef3fb539da605)
•
[3.6.2](https://github.com/matze/wastebin/tree/f9cb25a4d5b19fa938eef0af42f68f2835f17b98)
•
[3.5.0](https://github.com/matze/wastebin/tree/d379a6e3e73e6f1fcf23f93c9b0cc857f46acbce)
•
[3.4.1](https://github.com/matze/wastebin/tree/c9d717329a6e357e8a13a324bfa9a53d41ae9b35)
•
[3.3.0](https://github.com/matze/wastebin/tree/a297749b932ed9ff32569f3af7ee8e4a5b499834)
•
[3.2.0](https://github.com/matze/wastebin/tree/3fdec3abde4f32b92323864ffea51577ce1e625e)
•
[3.1.0](https://github.com/matze/wastebin/tree/e404ecec61eaafa1187b8d6b45282d72b076563d)
•
[3.0.0](https://github.com/matze/wastebin/tree/14a30bb540110e76da6a6045cd0e83fd2218cdd7)
•
[2.7.1](https://github.com/matze/wastebin/tree/85a519ef9079c4618f851cce575b5a84334a6f42)

*Section quoted from the upstream readme.*
---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Rust / Cargo |
| Manifests detected | Cargo.toml, Cargo.lock |
| Files in snapshot | 91 |
| Lines of code | 8162 |
| Dependency references | 66 |
| Dependencies by ecosystem | cargo: 66 |
| Upstream license | MIT |
| Overlay license | Anticommons 0.1.0 |

Top dependency references recorded in the benchmark snapshot:

| Ecosystem | Name | Version | Source file |
|-----------|------|---------|-------------|
| cargo | resolver | 2 | Cargo.toml |
| cargo | version | 3.8.0 | Cargo.toml |
| cargo | edition | 2024 | Cargo.toml |
| cargo | rust-version | 1.95 | Cargo.toml |
| cargo | askama | 0.16.0 | Cargo.toml |
| cargo | serde | 1 | Cargo.toml |
| cargo | syntect | 5 | Cargo.toml |
| cargo | tokio | 1 | Cargo.toml |
| cargo | two-face | 0.5.0-rc1 | Cargo.toml |
| cargo | thiserror | 2 | Cargo.toml |
| cargo | tracing | 0.1 | Cargo.toml |
| cargo | unsafe_code | forbid | Cargo.toml |
| cargo | cast_possible_truncation | deny | Cargo.toml |
| cargo | cast_possible_wrap | deny | Cargo.toml |
| cargo | cast_sign_loss | deny | Cargo.toml |
| ... | (51 more) | | |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

No configuration section was found in the upstream readme. Configuration-relevant files detected in this project directory:

- `Cargo.toml`
- `Dockerfile`

Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

Upstream contributions: fork the `WASTE` project, create a feature branch, and open a pull request against upstream. Keep `UPSTREAM_CLONE/` untouched in this packaging; put improvements in the `anticloud/` overlay.

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: MIT** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
Copyright 2022 Matthias Vogelgesang

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original MIT terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `MIT` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `WASTE` (category: WASTE MANAGEMENT)
- **Upstream URL:** https://github.com/Richienb/ros-data-waster
- **Pinned commit (SHA):** `0aeed8891184d496b0b6e714264af7fa970d4a63`
- **Branch:** master
- **Pin provenance:** GitHub API commits/master. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`0ba4be3fab4e5012a0f226804e9dcdb4972cbabc41f59ad9c15d398a839a2481`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

