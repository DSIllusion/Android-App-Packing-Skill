# Android App Packing Skill

A reusable Codex skill for building Android release artifacts and verifying that the exact package is ready to hand off. It follows the project's existing build and signing setup.

## What it checks

- **APK for direct installation:** checks the signing certificate, application ID, version, minimum Android version and supported ABIs. For an update, it compares the package against the existing app or a trusted baseline.
- **AAB for Google Play handoff:** validates the bundle and checks its signature. An AAB is not directly installable, and validation does not mean Google Play has accepted it.
- **Device testing:** when authorized and a device is available, verifies installation or upgrade and launch. Otherwise, reports which checks were static and which were not run.
- **Handoff:** reports the artifact path, source revision, package version, signing result, test coverage and SHA-256. The skill does not publish to a store, push code or deploy by default.

## Install for Codex

Clone this repository, then run one of the following commands from its root. If you already have this skill installed, review the changes before replacing your local copy.

**macOS / Linux**

```bash
mkdir -p ~/.agents/skills/android-app-pack
cp android-app-pack/SKILL.md ~/.agents/skills/android-app-pack/SKILL.md
```

**Windows (PowerShell)**

```powershell
$dest = Join-Path $HOME '.agents\skills\android-app-pack'
New-Item -ItemType Directory -Force -Path $dest | Out-Null
Copy-Item -LiteralPath '.\android-app-pack\SKILL.md' -Destination $dest
```

This is a per-machine install. To share the skill within an Android project instead, add `android-app-pack/SKILL.md` under that project's `.agents/skills/` directory.

## Use it

Open Codex in your Android project and mention `$android-app-pack` in your request. For example:

> Use $android-app-pack to build a signed release APK for direct installation. Check whether it can update the current install, and report whether install and launch were actually tested.

The skill instructions are in [android-app-pack/SKILL.md](android-app-pack/SKILL.md).
