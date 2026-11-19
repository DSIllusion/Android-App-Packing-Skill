---
name: android-app-pack
description: Build and verify Android APKs for direct installation or AABs for store handoff. Use for Android packaging, signing verification, device compatibility checks, and artifact handoff; not for feature implementation or store publishing.
---

# Android packaging and acceptance

## Resolve the build
- Read the repo's instructions and established build or CI commands. Record the source revision and whether local changes are included. Use the intended workspace; do not copy the whole project, switch branches, discard edits, or change source merely to make a package.
- Determine the Android app/module, variant, distribution target and signing profile. For a phone handoff, produce a signed installable APK (a standalone APK unless split installation was requested); for Play handoff, produce the project's signed AAB. An AAB or a single device split is not a standalone APK. Do not substitute a debug artifact for an expected release.
- Reuse the established SDK, build command, and authorized signing configuration. If expected signing or a consequential build choice cannot be resolved from the project, ask rather than guessing. Do not expose signing secrets, generate replacement keys, or change compatibility settings without approval.

## Verify the exact artifact
- Confirm the build succeeded, locate its new output rather than a stale package, and inspect the final artifact. Record application ID, version name/code, build variant, minimum SDK, and included native ABIs when applicable. Check install mode (fresh install vs. update), target device OS/ABI, and any mandatory device features. Do not conflate inspected metadata with device compatibility confirmed by installation.
- For APKs, verify the actual signing certificate with Android SDK `apksigner verify --print-certs`. If the package should upgrade an existing install, compare application ID, version code and signing compatibility against that app or a trusted baseline; account for Play App Signing or authorized key rotation where relevant. For AABs, validate with `bundletool validate` and verify the configured bundle signature separately; do not present that as Play acceptance.
- If an authorized target phone or emulator is available and an install was requested or already allowed, test the intended install or upgrade and launch, then record the actual device and observed result. For an AAB, test equivalent generated APKs with `bundletool` if appropriate and state what was tested. Never uninstall, clear data, or reset a device to force a pass. If no device is available, report “static checks passed; install/launch not tested.” If a check fails, report the failing check and do not label the package ready.

## Handoff
Report the artifact path, commit or local source state, version, variant, signing result, target compatibility, SHA-256, and which install/launch checks passed, failed, or were not run. Keep signing secrets out of logs and outputs. Upload or publish only to the destination the user requested; packaging alone does not authorize store publishing, Git push, or any deployment. If a coordinating assistant owns delivery, return the verified artifact path and results to it.
