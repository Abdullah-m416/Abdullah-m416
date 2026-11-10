name: Generate Snake

on:
  # run automatically every 12 hours
  schedule:
    - cron: "0 */12 * * *"

  # allow manual runs from the Actions tab
  workflow_dispatch:

  # run on every push to the main branch
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Generate snake animation
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg?color_snake=#D4FF24&color_dots=#0b1220,#102a5c,#1558C9,#5b8def,#D4FF24

      - name: Push snake to the output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
