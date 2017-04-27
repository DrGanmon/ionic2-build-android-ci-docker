# ionic2-build-android-gitlab-ci

This image provides all tools required to automatically build and Android app out of your ionic 2 (and possibly also cordova) project.
## CI Configuration

Here is a sample .gitlab-ci.yml file for setting up the project and compiling it:

```yaml
image: DrGanmon/ionic2-build-android-giltab-ci:latest

compile_android:
  stage: build
  script:
    - cp debug.keystore ~/.android/debug.keystore
    - npm install
    - cordova platform update android    
    - ionic config build
    - ionic state restore
    - ionic build android
  artifacts:
    paths:
      - platforms\android\build\outputs\apk\android-debug.apk

```

It is important to manage your keystores correctly. For signing debug releases, the android build tools will automatically fall back to `~/.android/debug.keystore`, which should not be password protected.

## .gitignore

This example assumes, that your `.gitignore` looks approximately like this: 

```
node_modules/
platforms/
plugins/
www/lib/
```

That will avoid checking in generated files or dependencies, that will be restored using the aforementioned commands during build.

## Getting the files out

Using Gitlab-CI build Artifacts

```
artifacts:
    paths:
      - platforms\android\build\outputs\apk\android-debug.apk
```
