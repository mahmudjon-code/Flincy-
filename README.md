# Flincy-name: Build APK
on: workflow_dispatch
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Unpack newest zip
        run: |
          f=$(git log --name-only --diff-filter=AM --pretty=format: -- '*.zip' | grep -m1 '\.zip$')
          echo "Using: $f"
          unzip -q "$f"
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'
          build-root-directory: WordCards
      - run: gradle assembleDebug
        working-directory: WordCards
      - uses: actions/upload-artifact@v4
        with:
          name: WordCards-apk
          path: WordCards/app/build/outputs/apk/debug/app-debug.apk
