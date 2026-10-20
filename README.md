# Optimo

A native macOS app for running Docker containers on Apple Silicon with as little memory as possible. Optimo runs Docker Engine, Compose and BuildKit in one lightweight virtual machine (Apple Virtualization via Colima/Lima) and manages it from a SwiftUI interface. It is built for Macs with 8 GB of memory.

**Status: beta.** Expect rough edges and please report them in [Issues](https://github.com/NikitaStogniy/optimo/issues).

## Install

1. Download the latest `Optimo-<version>-arm64.dmg` from [Releases](https://github.com/NikitaStogniy/optimo/releases).
2. Drag Optimo to Applications and open it.
3. Click **Prepare environment** and wait for "Environment ready".

Requirements: a Mac with Apple Silicon, macOS 14 or later. Docker CLI, Compose, Buildx, Colima/Lima and the Linux image are inside the app; Homebrew, Xcode and Docker Desktop are not needed. The internet is only needed to pull container images.

To use `docker` from Terminal, choose **Connect Docker** during setup or in Settings → Connection, then open a new Terminal window. Your previous Docker context can be restored there.

## What it does

- Compose stacks: add a `compose.yaml`, start, stop, build, view logs.
- Containers, images, builds, volumes and networks, with logs, stats, inspect and a shell.
- The VM gets 3 GiB by default (2, 3 or 4 GiB in Settings). It stops by itself after 5 minutes without running containers or builds; running stacks are never restarted automatically.
- File changes on the Mac reach watchers in containers (`nest --watch`, nodemon, Vite, webpack) without polling.
- `http://service.stack.localhost` names for published ports, with optional local HTTPS.

## Known limitations

- Auto-stop, local domains and reload on file changes work only while Optimo is open.
- After an atomic save (JetBrains "safe write", Vim without `backupcopy=yes`), `node --watch` and `vite build --watch` stop seeing that file until they restart.
- Bind mounts must be inside a project folder or a folder added in Settings → Files. Optimo checks this before starting a stack.
- Published UDP ports don't reach the Mac; UDP between containers works.
- Testcontainers doesn't read Docker contexts: set `DOCKER_HOST` (Settings → Connection has the line to copy).
- No Compose profiles or multi-file Compose yet.
- Not yet tested: VPN, corporate proxies, Firefox with local HTTPS.

## Updates

From 0.7.0 on, Optimo updates itself with [Sparkle](https://sparkle-project.org). Once a day it fetches `appcast.xml` from the latest release of this repository; it sends no system profile. A new version downloads in the background, as a small delta when one is published. Before unpacking, Optimo checks the update against its own signing key, and before installing, against the Developer ID signature.

When an update is ready, the version badge in the sidebar shows **Update ready** and Optimo asks: **Restart now** or **Later**. Later installs it the next time you quit Optimo. The VM and containers keep running while Optimo restarts. Settings → General has **Download updates automatically** (turn it off to be asked before each download), **Check for updates** and the release channel.

From 0.6.x or earlier, update by hand once: quit Optimo and drag the new version over the old one in Applications. Your VM, images and volumes stay as they are.

---

## По-русски

Optimo — нативное приложение macOS для Docker-контейнеров на Apple Silicon с минимальным расходом памяти: одна лёгкая VM (Apple Virtualization через Colima/Lima) с Docker Engine, Compose и BuildKit. Рассчитано на Mac с 8 ГБ памяти.

**Бета.** Об ошибках пишите в [Issues](https://github.com/NikitaStogniy/optimo/issues).

Установка: скачайте `Optimo-<версия>-arm64.dmg` в [Releases](https://github.com/NikitaStogniy/optimo/releases), перетащите в Applications, откройте и нажмите **Prepare environment**. Нужен Mac на Apple Silicon с macOS 14 или новее; Homebrew, Xcode и Docker Desktop не нужны. Язык интерфейса переключается в Settings → General → Language.

Обновления: начиная с 0.7.0 Optimo раз в день проверяет релизы этого репозитория, скачивает новую версию в фоне и спрашивает перед перезапуском; VM и контейнеры продолжают работать. С 0.6.x один раз обновитесь вручную: закройте Optimo и перетащите новую версию поверх старой в Applications.

Ограничения беты перечислены выше, в разделе «Known limitations».
