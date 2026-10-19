# name: Build Luna APK

on:
  workflow_dispatch:
  push:
    branches: [main, master]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - uses: gradle/actions/setup-gradle@v4

      - name: Unzip Luna project
        run: unzip -o Luna_AndroidIDE_Ready.zip -d luna-source

      - name: Build APK
        run: |
          GRADLEW="$(find luna-source -name gradlew -type f | head -n 1)"
          test -n "$GRADLEW" || exit 1
          cd "$(dirname "$GRADLEW")"
          chmod +x gradlew
          ./gradlew assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Luna-debug-APK
          path: luna-source/**/build/outputs/apk/debug/*.apk
          if-no-files-found: error
          Luna-_Aneroid
