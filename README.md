# EVCfive — Android Video Editor

Repository: `EVCfive`  
Application label: `EVCfive`  
Project version: `0.2.191` (`versionCode 391`)

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

The workflow checks out the repository, configures JDK 17 and Android SDK 35, builds the debug APK with Gradle 8.9, and uploads the ARM32 APK as an Actions artifact.

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

A successful Android build and on-device runtime behavior have not been verified in this environment. Confirm the first build in GitHub Actions after pushing.
