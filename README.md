# bootstrap-dotnet
This is a GitHub Action meant to be used as a [composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) within an existing workflow. This action encapsulates setting up .NET SDK, MSBuild path (on Windows only), and NuGet CLI in one step.  Each is optional, but I'd argue if you are only using one of these then just use the actual action.

The action encapsulates the following other actions:

- [actions/setup-dotnet](https://github.com/actions/setup-dotnet)
- [microsoft/setup-msbuild](https://github.com/microsoft/setup-msbuild)
- [NuGet/setup-nuget](https://github.com/NuGet/setup-nuget)

## Usage
You can use this composite Action in your own workflow by adding:

### Defaults
This action uses the following defaults:

- dotnet-version: 6.0.x
- MSBuild: latest (meaning the latest version installed on an agent)
- NuGet: latest (meaning the latest CLI version)
- 

```yml
name: OWASP (Zap Scan)

on:
  push:
    branches: [ feature/* ]
  workflow_dispatch:
  schedule:
    - cron: "0 9,18 * * 1-5"

jobs:
  zap-scan-pre-prod-test:
    name: OWASP scan Pre-Prod Test
    runs-on: ubuntu-latest
    steps:
      # - uses: actions/checkout@v2
      - name: Checkout and Zap Scan
        uses: awazevr/zap-scan-action@v1.0.1
        with:
          target: 'https://xxxxxxxxxx.xxx.xxxxxxx/swagger.json'
          issue_title: 'ZAP Scan Report'
          fail_action: 'true'
          rules_file_name: '.zap/rules.tsv' # << location of the rules file

```