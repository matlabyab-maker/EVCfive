# EVCfive — Android Video Editor

Repository: `EVCfive`  
Application label: `EVCfive`  
Project version: `0.2.192` (`versionCode 392`)

## Project configuration

- Android application module: `app`
- Gradle root project: `EVCfive`
- Application ID / namespace: `com.evc.app` (intentionally unchanged to preserve app identity)
- Minimum Android SDK: 24
- Target / compile SDK: 35
- Java and Kotlin JVM target: 17
- Native ABI filter: `armeabi-v7a` only (32-bit ARM)
- Debug APK shrinking: disabled

## GitHub Actions and the previous Commit/Actions issue

Workflow: `.github/workflows/android.yml`

The workflow is deliberately configured to run for pushes to **all branches** using:

```yaml
on:
  push:
    branches: ["**"]
  pull_request:
    branches: ["**"]
  workflow_dispatch:
```

This avoids the previous trigger problem where a push to a branch other than `main` or `master` could appear in Git history but would not start an Actions build. Manual workflow execution is also enabled.

The workflow checks out the repository, configures JDK 17 and Android SDK 35, builds FFmpegKit from its maintained source for **armeabi-v7a only**, verifies the AAR contains that ABI and no 64-bit/x86 libraries, then builds the debug APK with Gradle 8.9 and uploads it as an Actions artifact. The 32-bit ARM native library is intentionally built in CI because the maintained Maven binaries publish arm64-v8a and x86_64, not armeabi-v7a.

## First push to the new repository

1. On GitHub, create a **new empty repository** named `EVCfive` under `matlabyab-maker`. Do not initialize it with a README, license, or `.gitignore`.
2. Extract this ZIP. The project root is the `EVCfive` folder; do not add another nested project folder inside the repository.
3. Open the extracted `EVCfive` folder in MGit. This ZIP already contains a fresh Git repository on branch `main`; do not initialize a second repository over it.
4. Confirm the remote is `origin` and points to `https://github.com/matlabyab-maker/EVCfive.git`.
5. Push branch `main`. The first push should trigger `EVCfive Android Build` in GitHub Actions. Any later branch push also triggers the workflow.
6. If Actions does not appear, verify that GitHub Actions are enabled for the repository and that `.github/workflows/android.yml` is present on the default branch.

## Clean start

- Removed all previous EVCfour Git branches, remotes, refs, and commit history.
- Created a new Git repository with one initial commit on `main` and a single `origin` remote for EVCfive.
- Kept Android source, resources, Gradle configuration, and the working timeline/export logic.
- Removed no functional application source files; no nested duplicate project, build output, or Gradle cache is included.
- Preserved the application ID and Kotlin package (`com.evc.app`).

## Split stability changes in 0.2.192

- Split now rejects repeated taps while an earlier Split task is active.
- The status line reports the part currently being generated.
- Failed/incomplete MediaStore outputs are deleted instead of being left as broken entries.
- A missing or incompatible native FFmpeg library is reported as an ARMv7 loading error instead of being silently treated as a normal split failure.
- Original streams are still copied with `-c copy`; no re-encoding is requested. Stream-copy boundaries can still move to nearby keyframes.

A successful GitHub Actions build and on-device runtime behavior have not yet been verified. The first push must complete the native FFmpegKit source build and APK build in Actions before installing the artifact.
