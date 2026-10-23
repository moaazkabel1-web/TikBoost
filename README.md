github/workflows/build-apk.yml

name: Build TikBoost APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Download project files
        uses: actions/checkout@v4

      - name: Build Android APK
        uses: cemcoe/web-to-apk-action@v1
        with:
          app_name: "TikBoost"
          app_id: "com.tikboost.app"
          build_command: "echo Static HTML app"
          web_dir: "."
