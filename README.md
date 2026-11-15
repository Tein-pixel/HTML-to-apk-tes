# HTML-to-apk-tes

This repository contains the TEIN Android project archive and a GitHub Actions workflow that builds it into a debug APK.

## Build the APK

1. Open the repository's **Actions** tab.
2. Select **Build TEIN dari ZIP**.
3. Choose **Run workflow**.
4. Open the completed run and download the **TEIN-apk** artifact.
5. Extract the artifact to get the generated debug APK.

The workflow is manually triggered with `workflow_dispatch`; editing files does not automatically start a build.

## Expected source layout

The workflow extracts `TEIN.zip` into a directory named `proj`, then runs Gradle from `proj/TEIN`. The archive must therefore contain a `TEIN/` project directory with at least:

- `settings.gradle`
- `app/build.gradle`
- The Android application source and resources.

## CI toolchain

The current workflow uses:

- Ubuntu GitHub-hosted runner.
- Eclipse Temurin Java 17.
- Gradle 8.5.
- Gradle task: `gradle :app:assembleDebug --stacktrace`.

The artifact is uploaded from `proj/TEIN/app/build/outputs/apk/debug/*.apk` under the name `TEIN-apk`.

## Troubleshooting

**The workflow cannot find `TEIN.zip`**  
Confirm the archive is committed at the repository root and the name uses the exact uppercase spelling.

**The build cannot find `proj/TEIN`**  
Inspect the archive contents. It must extract to `proj/TEIN/settings.gradle` and `proj/TEIN/app/build.gradle`, not an extra nested folder.

**Gradle fails**  
Open the failed Actions run and inspect the first Gradle error. Confirm the project is compatible with the Java 17 and Gradle 8.5 environment configured in the workflow.

## Notes

The workflow produces a debug APK for testing. It does not create a Play Store-ready release or configure production signing.
