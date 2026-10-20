# Optimo

A native macOS app for running Docker containers on Apple Silicon with as little memory as possible. Optimo runs Docker Engine, Compose and BuildKit in one lightweight virtual machine (Apple Virtualization via Colima/Lima) and manages it from a SwiftUI interface. It is built for Macs with 8 GB of memory.

**Status: beta.** Expect rough edges and please report them with **Help → Report a problem…** or in [Issues](https://github.com/NikitaStogniy/optimo/issues).

## Install

1. Download the latest `Optimo-<version>-arm64.dmg` from [Releases](https://github.com/NikitaStogniy/optimo/releases).
2. Drag Optimo to Applications and open it.
3. Click **Prepare environment** and wait for "Environment ready".

Requirements: a Mac with Apple Silicon, macOS 14 or later. Docker CLI, Compose, Buildx, Colima/Lima and the Linux image are inside the app; Homebrew, Xcode and Docker Desktop are not needed. The internet is only needed to pull container images, check for updates and send what you choose to share (see [Privacy](#privacy)).

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

## Privacy

From 0.8.0 on, Optimo can send data to its developer through [PostHog](https://posthog.com) (US Cloud). IP addresses aren't stored, there are no accounts, and each Mac gets a random ID.

- **Usage data and crash reports: only with your consent.** Optimo asks once; closing the question means no. If you agree, it sends which actions you use, whether they worked and how long they took, the page you're on, the first line of an error and crash details. Container, image, project, volume and network names, paths, file contents and environment variables are never sent. Change it anytime in Settings → General → **Share usage data and crash reports**.
- **Report a problem** (Help menu, Settings → General or Diagnostics) sends only when you click **Send**: your description, an optional email, and the diagnostics report if you leave it attached. You can see the report before sending; your home folder appears in it as `~`. If you don't share crash reports, the launch after a crash offers to send one with the crash log.

---

## По-русски

Optimo — нативное приложение macOS для Docker-контейнеров на Apple Silicon с минимальным расходом памяти: одна лёгкая VM (Apple Virtualization через Colima/Lima) с Docker Engine, Compose и BuildKit. Рассчитано на Mac с 8 ГБ памяти.

**Бета.** Об ошибках сообщайте через **Help → Report a problem…** или в [Issues](https://github.com/NikitaStogniy/optimo/issues).

Установка: скачайте `Optimo-<версия>-arm64.dmg` в [Releases](https://github.com/NikitaStogniy/optimo/releases), перетащите в Applications, откройте и нажмите **Prepare environment**. Нужен Mac на Apple Silicon с macOS 14 или новее; Homebrew, Xcode и Docker Desktop не нужны. Язык интерфейса переключается в Settings → General → Language.

Обновления: начиная с 0.7.0 Optimo раз в день проверяет релизы этого репозитория, скачивает новую версию в фоне и спрашивает перед перезапуском; VM и контейнеры продолжают работать. С 0.6.x один раз обновитесь вручную: закройте Optimo и перетащите новую версию поверх старой в Applications.

Приватность: начиная с 0.8.0 данные об использовании и отчёты о сбоях уходят разработчику через PostHog только с вашего согласия. Optimo спрашивает один раз, закрытие вопроса означает отказ; изменить можно в Settings → General. Имена контейнеров, образов и проектов, пути, содержимое файлов и переменные окружения не отправляются, IP-адреса не сохраняются. Отчёт о проблеме (Help → Report a problem…) уходит, только когда вы нажмёте **Send**, и его можно посмотреть перед отправкой.

Ограничения беты перечислены выше, в разделе «Known limitations».
