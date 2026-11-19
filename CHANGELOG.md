# Changelog

## 0.3.0 (2026-11-19)

- The panel reports `Google Chrome`, not `Brave`, in the brands a site reads. Brave's user agent already looked like Chrome, but `Sec-CH-UA` and `navigator.brave` still gave it away, and Brave is missing from the browser list Microsoft Conditional Access lets through. A small extension, loaded for the panel only, swaps the brand in `sec-ch-ua`, `sec-ch-ua-full-version-list` and `navigator.userAgentData`, and hides `navigator.brave`. `install --brand brave` keeps the browser's own brand; `--brand chrome` is the default and the `T3_HEADED_BROWSER_BRAND` variable overrides it per launch.
- `install` now stops the T3 browser on every run, not only when it shims a binary for the first time, so a changed browser, brand or GPU setting takes effect on the next tab instead of the next crash.

## 0.2.0 (2026-11-17)

- `install --gpu` makes the browser render on a real GPU instead of SwiftShader. The shim drops Playwright's `--disable-gpu` and `--enable-unsafe-swiftshader` and runs ANGLE on EGL through the DRI render node, so WebGL reports the actual GPU. A SwiftShader renderer is a strong bot signal for checks such as PerimeterX. If the render node isn't accessible at launch, the shim keeps software rendering. `--no-gpu` turns it off, and `status` shows it.

## 0.1.1

- T3's HTML previews, which launch the shell with `--headless=new`, run the original headless shell.
- The shim disables WebAuthn (merged into Playwright's `--disable-blink-features`), because a headed browser's passkey prompt can't be shown in the panel and blocks the page.
- The shim sets `XAUTHORITY` to `~/.Xauthority` unless the inherited one belongs to the same display.

## 0.1.0 (2026-10-17)

First release.

- `t3-headed-browser install` swaps T3 Code's bundled `chrome-headless-shell` for a shim. The shim runs a full Chromium-based browser (Google Chrome, Chromium, Brave or Microsoft Edge) headed on an X display, so Google sign-in and sign-up work in the browser panel of a remote T3 server.
- Persistent profiles survive T3 closing a profile's first page. The browser starts windowless, and its first page opens through a second invocation.
- The shim falls back to the original headless shell when the browser or the X display is missing.
- `t3-headed-browser.path` re-applies the shim when T3 downloads a new shell version. It also stops a headless browser that T3 launched before the shim landed.
- `uninstall` restores the original binaries, and `status` shows the configuration and each shell's state.
- Includes an example `t3-xvfb.service` for servers without a display.
