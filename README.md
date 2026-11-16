# Angry Birds Trilogy Recompiled

A game-specific desktop build based on [Zakuro](https://github.com/fearkov/zakuro), a high-level-emulation Nintendo 3DS runtime, and [3dsrecomp](https://github.com/fearkov/3dsrecomp), its ahead-of-time ARM code recompiler.

This fork is deliberately limited to **Angry Birds Trilogy for Nintendo 3DS**. Other games are hidden from the library and cannot be launched through the application.

## Supported game dumps

| Region | Nintendo 3DS title ID |
|---|---|
| USA | 00040000000AE200 |
| Europe | 00040000000AF400 |

The application expects a legally obtained, decrypted dump of the game. It does not include the ROM, game code, textures, music, or other game assets.

## Playing

1. Download the package for your operating system from [Releases](https://github.com/nvd123-YT/AngryBirdsRecomp/releases).
2. Extract the download and start Angry Birds Trilogy.
3. Choose the folder containing your decrypted Angry Birds Trilogy dump.
4. Select the game and press **Recompile**. The ARM code is recompiled locally on your computer. This is a one-time step for each game dump/build; recompile again when the runtime reports the installed code is outdated.
5. Start the game. The application uses the local recompiled code where available and falls back to its interpreter for code that is not covered.

The title-specific recompiled output stays on the user's computer. It is not included in this repository, CI artifacts, or GitHub Releases.

## Included runtime features

This fork keeps the underlying Zakuro runtime and focuses it on Angry Birds Trilogy:

- Ahead-of-time code recompilation through 3dsrecomp, with interpreter fallback.
- Keyboard, mouse/touchscreen, and common game-controller input.
- Configurable key and controller mappings.
- 3DS screen layouts, scaling, and graphics settings.
- Audio and per-game settings.
- Local save data stored separately under the Angry Birds Trilogy application-data directory.

Features inherited from the runtime remain subject to the support and behaviour of this particular game.

## Downloads

- **Windows:** unzip angry-birds-trilogy-windows-x86_64.zip and run angry-birds-trilogy.exe.
- **macOS Apple silicon:** unzip angry-birds-trilogy-macos-arm64.zip and open Angry Birds Trilogy.app.
- **macOS Intel:** unzip angry-birds-trilogy-macos-x86_64.zip and open Angry Birds Trilogy.app.
- **Linux:** use AngryBirdsTrilogy-x86_64.AppImage, or extract angry-birds-trilogy-linux-x86_64.tar.gz and run ./angry-birds-trilogy.

The macOS app is not notarized. On first launch, macOS may block it; allow it through **System Settings → Privacy & Security → Open Anyway**. The Linux AppImage needs the host system's ALSA sound library.

## Building from source

The project uses the Rust workspace and requires Rust 1.95 or newer.

~~~
cargo build --release --locked -p zakuro
cargo test --release --workspace
~~~

The desktop binary is written to target/release/angry-birds-trilogy (or .exe on Windows). The macOS package script bundles MoltenVK into the app:

~~~
packaging/macos-app.sh target/release/angry-birds-trilogy 0.1.0 angry-birds-trilogy-macos-arm64.zip
~~~

For a normal user, the prebuilt packages on the Releases page are the easiest option.

## GitHub Actions and releases

- **CI** runs on pushes and pull requests. It builds and tests the Rust workspace on Windows, macOS, and Linux. It also keeps a Windows executable as a temporary Actions artifact.
- **Release** builds Windows x64, macOS Apple silicon, macOS Intel, and Linux x64 packages. It runs when a version tag is created or pushed. Once every platform build succeeds, it attaches the packages to the GitHub Release and publishes it. If you created a draft release on GitHub first, the workflow fills that draft with the packages and publishes it after the builds finish.
- To test a branch without publishing, open **Actions → Angry Birds Trilogy Release → Run workflow**, enter the branch name (such as `main`) in `tag`, and set `publish` to `false`. The packages remain temporary Actions artifacts.

### Creating version 1.0.0 on GitHub

1. Open the repository and select **Releases → Draft a new release**.
2. In **Choose a tag**, type `v1.0.0`, then select **Create new tag: v1.0.0**.
3. Set **Target** to `main`.
4. Select **Save draft**. Creating the tag on the Releases page triggers the release build. The workflow will attach all four platform packages and publish the draft when they are ready.

The Rust workspace version is `1.0.0`, and the matching Git tag is `v1.0.0`.

## Attribution and legal notes

This project is an unofficial, game-specific fork of [fearkov/zakuro](https://github.com/fearkov/zakuro) and uses [fearkov/3dsrecomp](https://github.com/fearkov/3dsrecomp). The original Zakuro project is MIT-licensed; see [LICENSE](LICENSE) for the preserved notice.

Angry Birds is a trademark of its respective owner. This project is not affiliated with or endorsed by Rovio or Activision. No game ROM, generated title-specific C code, compiled game library, copyrighted game assets, or other game data is distributed. Users must provide their own lawful game dump and keep the resulting local recompilation on their own device.
