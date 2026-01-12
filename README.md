# RISC-V Rootfs Trial: The LFS Gate

Language: [简体中文](./README.zh-CN.md)

Guten Tag! traveler — you have stepped through the Gate.

Your quest is to forge a **riscv64** **Linux From Scratch** rootfs and seal it inside your own GitHub **Releases** vault.

This repository does **not** accept the rootfs payload itself. We only accept two things:

1) A Markdown “scroll” written by you, which **must include “how to run” instructions**.
2) A screenshot of `fastfetch` running **inside that environment**.

If your PR is merged, the castle machinery (CI) will use your **GitHub ID** to create an account for you in our internal realm.

---

## What you must submit

Your PR must include:

### 1) The write-up scroll

Create a new file:

- `submissions/<githubid>.md`

### 2) The fastfetch screenshot

Commit one PNG screenshot into the repo:

- `evidence/<githubid>/fastfetch.png`

Screenshot requirements:

- Must be captured from `fastfetch` running inside your published riscv64 rootfs environment
- The screenshot must clearly show the architecture info (**riscv64**) and basic system information

> Do **not** commit the rootfs (or any large binaries) into this repository. Only submit the Markdown + screenshot.

---

## Where your rootfs should live

Your rootfs must be published in the **Releases** of any GitHub repository you own, and must satisfy:

- The Release asset filename **must be exactly**:
  - `rootfs-riscv64-lfs-<githubid>.tar.zst`

In your scroll, you must provide:

- The rootfs Release page link
- The rootfs sha256 checksum

---

## Taboos

- Do **not** include any secrets in the rootfs: keys, tokens, SSH keys, `.git-credentials`, private keys, cloud credentials, etc.
- Do **not** accidentally pack directories like `/etc/ssh/`, `~/.ssh/`, or `~/.config/` from your machine into the rootfs.

You must declare this in your scroll (template below).

---

## Scroll Template

Copy the following into `submissions/<githubid>.md` and fill it in:

```md
# <githubid>'s Trial Log

## Basics

- GitHub ID: <githubid>
- Contact email: <email>
- Rootfs release repo: https://github.com/<you>/<repo>

## Rootfs Asset

- Filename: rootfs-riscv64-lfs-<githubid>.tar.zst
- SHA256: <sha256>

## How to run from the rootfs

> Goal: the shortest steps from "download the rootfs" to "enter the environment and run fastfetch".

### Method 1

1. ...

## fastfetch evidence

<!-- Insert your fastfetch screenshot here -->

## How it was forged (LFS summary)

- Tutorial / version reference:
- Key configuration notes (toolchain / glibc / kernel / init / busybox / packaging strategy, etc.):
- Any deviations from "vanilla LFS" (if any):

## Key lessons learned

- Lesson 1: ...
- Lesson 2: ...

## Known issues / TODO (if any)

- ...

## License & security declaration

- I confirm the rootfs contains no secrets/tokens/SSH keys/credentials/private data.
```
