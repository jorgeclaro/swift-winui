# swift-winui

Swift Language Bindings for common WinRT APIs and the Windows App SDK. For now, this package hosts bindings for WinUI 3, the Windows App SDK, UWP, Windows Foundation, and WebView 2 Core.

## Bindings

The bindings are generated using [thebrowsercompany/swift-winrt](https://github.com/thebrowsercompany/swift-winrt). To discover which bindings each product provides bindings for, see the `projections.json` file in the corresponding subdirectory of the `Support` directory.

## Useful resources

- [Windows App SDK API documentation](https://learn.microsoft.com/en-us/windows/windows-app-sdk/api/winrt/microsoft.ui.xaml.controls?view=windows-app-sdk-1.7)
- [Official WinUI 3 GitHub repo](https://github.com/microsoft/microsoft-ui-xaml)

## SDK Versions

These bindings target a specific Windows SDK and a specific Windows App SDK. Bindings for the Windows SDK are compatible with all Windows SDK versions that follow. Bindings for Windows App SDK APIs on the other hand require that a matching Windows App Runtime version is installed.

1. Windows SDK: `10.0.18362.2005` (the `Microsoft.Windows.SDK.Contracts` the bindings are generated from)
2. Windows App SDK: `1.7.250909003`

The versions are set in the `projections.json` files under `Support`. Keep them the same in every one of them.

### Branches and tags

`main` targets the newest Windows App SDK version here (1.7). Older versions that are still kept live on branches named `winappsdk-<major.minor>`, cut from `main` at the moment it moved on. A change that every kept version needs (a fix, a new binding) is made on `main` and cherry-picked to those branches. Tags are named `winappsdk-<major.minor>-r<n>`: the SDK version, then the revision of this repository's own changes on top of it (`winappsdk-1.7-r1` is the first). To depend on a version, pin a tag's commit or a revision.

To move to a newer Windows App SDK, first keep the current one: `git branch winappsdk-1.7`, from `main`. Then on `main` change the version in each `projections.json` (and in `Bundler.toml`, `Support/WinAppSDK/packages.config` and the runtime links below), run each `Support/*/generate-bindings.ps1`, then build and fix what the newer SDK changed.

## Dependencies

### Compiletime dependencies

To compile an app that uses swift-winui, you must install a matching version of the Windows SDK;

```pwsh
winget install --id Microsoft.WindowsSDK.10.0.18362
```

### Runtime dependencies

To run an app that uses swift-winui, you must install the correct version of the Windows App Runtime;

- x64: [windowsappruntimeinstall-x64.exe](https://aka.ms/windowsappsdk/1.7/1.7.250909003/windowsappruntimeinstall-x64.exe)
- arm64: [windowsappruntimeinstall-arm64.exe](https://aka.ms/windowsappsdk/1.7/1.7.250909003/windowsappruntimeinstall-arm64.exe)

Or with winget: `winget install Microsoft.WindowsAppRuntime.1.7`.

### The bootstrapper DLL

An unpackaged app starts the Windows App Runtime through `Microsoft.WindowsAppRuntime.Bootstrap.dll`. `WindowsAppRuntimeInitializer` looks for it in the `swift-winui_CWinAppSDK.bundle` folder next to the executable. An app that would rather carry the DLL inside its executable can set `WindowsAppRuntimeInitializer.bootstrapperPathProvider` before `SwiftApplication.main()` runs: it is asked for the DLL's full path when the folder is not there. The app decides where the DLL comes from and is responsible for checking that it is the file it expects before returning the path.
