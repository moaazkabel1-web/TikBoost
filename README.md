YAML
name: Build TikBoost APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install Capacitor
        run: |
          npm init -y
          npm install @capacitor/core @capacitor/cli @capacitor/android

      - name: Configure Capacitor
        run: |
          mkdir -p www
          cp index.html www/index.html
          npx cap init TikBoost com.tikboost.app --web-dir www
          npx cap add android

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 21

      - name: Build APK
        run: |
          cd android
          chmod +x gradlew
          ./gradlew assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: TikBoost-APK
          path: android/app/build/outputs/apk/debug/app-debug.apk
