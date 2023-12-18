# Hanime Auto Coins Plus (Forked)

This repository is a fork of the [hanime-auto-coins-plus](https://github.com/zip6como/hanime-auto-coins-plus) project, with slight modifications tailored for personal use. The primary alteration involves integrating GitHub Actions to run the workflow. Additionally, the three original variables in the .env file (WEBHOOK_URL, HANIME_EMAIL, HANIME_PASSWORD) have been replaced with secrets for enhanced security.

## Changes Made

- Integration of GitHub Actions for automated workflow.
- Moved sensitive variables (WEBHOOK_URL, HANIME_EMAIL, HANIME_PASSWORD) from .env to GitHub Secrets.

## Usage

Before using, please refer to the original project. The original author has provided detailed instructions with images!

### Step One

1. Follow the instructions in the original project to obtain the `webhook_url`.

2. In the repository's settings, go to  

   > Secrets and variables -> Actions -> New repository

    and add the following three values:

   - `WEBHOOK_URL`
   - `HANIME_EMAIL`
   - `HANIME_PASSWORD`

### Step Two

Create a `.github/workflows/xxx.yml` file and add the following content:

```
name: hanime-auto-coins

on:
  workflow_dispatch:
  schedule:
    - cron: '0 */4 * * *'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v1
        with:
          python-version: 3.7
      - name: Cache pip packages
        uses: actions/cache@v2
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}
          restore-keys: |
            ${{ runner.os }}-pip-
      - name: Install requirements
        run: pip install -r ./requirements.txt
      - name: Working
        env:
          HANIME_EMAIL: ${{ secrets.HANIME_EMAIL }}
          HANIME_PASSWORD: ${{ secrets.HANIME_PASSWORD }}
          WEBHOOK_URL: ${{ secrets.WEBHOOK_URL }}
        run: python ./AIO.py
```

### Step Three

Manually trigger the workflow once and observe if it succeeds.

## Disclaimer

This project is solely for personal use, and any modifications or adaptations are the responsibility of the user. The original project [hanime-auto-coins-plus](https://github.com/zip6como/hanime-auto-coins-plus) holds the main documentation and usage guidelines.
