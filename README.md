# Hiddenlayer Local Model Scanner Github Action

Integrate model scanning into your continuous integration (CI) process with HiddenLayer's GitHub Actions (GHA) integration. This action can scan your models that are stored in a repository.

## GitHub Runner Size

> [!WARNING]
> The default storage of a Github hosted runner is 14 GB. Some models can far exceed this limitation leading to the experience of a failure for the GitHub action to complete due to running out of disk space. Please reference the [Github Runner Sizing Guide](https://docs.github.com/en/actions/using-github-hosted-runners/using-larger-runners/about-larger-runners) to choose runners appropriate for the size of models you plan to scan.


## Inputs

`model_path` (required): Path to the model(s), relative to root of your repository.

`fail_on_detection`: True to fail the pipeline if a model is deemed malicious. Defaults to `True`.

`output_file`: Output file to write the scan results to. Defaults to display the results in stdout.

`sarif_file`: Output file to write the scan results to in the SARIF format.

`run_id`: Run id for the current run of the model scanner. Defaults to `YYYYMMDDTHHMMSS`

`upload_to_hiddenlayer` (**see Environment Variables**): True to upload results of scan to Hiddenlayer Console. Defaults to `True`.

`upload_to_github_security`: True to upload a SARIF report to Github Security. Defaults to `False`.

`local_scanner_image`: Image to use to scan. Defaults to `quay.io/hiddenlayer/distro-cli-modelscanner`.

`local_scanner_version`: Version of the image to use to scan. Defaults to `latest`.

`model_name` (**Optional. Only used if uploading results to Hiddenlayer**): Name of the model to attach results to.

`model_version` (**Optional. Only used if uploading results to Hiddenlayer**): Version of the model to attach results to.


## Environment Variables

`HL_CLIENT_ID` (**required for SaaS result upload only**): Your HiddenLayer API Client ID.

`HL_CLIENT_SECRET` (**required for SaaS result upload only**): Your HiddenLayer API Client Secret.

`HL_CONTAINER_REGISTRY_USER`: Your Quay Username used to pull the Hiddenlayer Model Scanner Image.

`HL_CONTAINER_REGISTRY_SECRET`: Your Quay Secret used to pull the Hiddenlayer Model Scanner Image.

`HL_LICENSE`: License provided by Hiddenlayer for use with Model Scanner CLI.

## Example Usage

```yaml
jobs:
  scan_model:
    runs-on: ubuntu-latest
    name: Scan a model
    steps:
      - uses: actions/checkout@v3
      - name: Scan model
        id: scan_model
        uses: hiddenlayerai/hiddenlayer-local-model-scan-github-action@v0.1.0
        with:
          model_path: ./models
          model_name: GH_Local_Scan_Test_Model
          model_version: 1
          upload_to_hiddenlayer: true
          upload_to_github_security: true
        env:
          HL_CLIENT_ID: ${{ vars.HL_CLIENT_ID }}
          HL_CLIENT_SECRET: ${{ secrets.HL_CLIENT_SECRET }}
          HL_CONTAINER_REGISTRY_USER: ${{ vars.HL_CONTAINER_REGISTRY_USER }}
          HL_CONTAINER_REGISTRY_SECRET: ${{ secrets.HL_CONTAINER_REGISTRY_SECRET }}
          HL_LICENSE: ${{ secrets.HL_LICENSE }}
```
