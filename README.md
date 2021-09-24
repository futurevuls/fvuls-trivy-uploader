# Trivy Uploader Container Action Template

## Usage

Describe how to use your action here.

### Inputs

| Name                 | default | required | description |
|----------------------|---------|----------|-------------|
| VULS_VERSION         | latest  | false    |Vulsの最新バージョンを指定する|
| TRIVY_VERSION        | latest  | false    |Trivyの最新バージョンを指定する|
| DOCKERFILE_PATH      | "" | true     |スキャン対象の `Dockerfile` への相対パス|
| IMAGE_NAME           | "" | true     | `Dockerfile` のイメージ名|
| FVULS_TOKEN          | "" | true     |FutureVulsのスキャナトークン|
| FVULS_SERVER_UUID    | "" | true     |FutureVulsのサーバUUID|
| FVULS_GROUP_ID       | "" | true     |FutureVulsのグループID|
| TRIVY_GLOBAL_OPTIONS | "" | false    |trivy のコマンドにオプションが渡せます(-d)|
| TRIVY_IMAGE_OPTIONS  | "" | false    |trivy image のコマンドにオプションが渡せます(--timeout=60m)|

## Examples

### Using the optional input

This is how to use the optional input.

```yaml
with:
  VULS_VERSION: world
  TRIVY_VERSION: optional
  TRIVY_GLOBAL_OPTIONS: '-d'
  TRIVY_IMAGE_OPTIONS: '--timeout=60m'
```

### Working Example

The example below will scan and upload go.sum and `./path/to/Dockerfile`
and the Action will only run when changes are applied to these files and pushed to release.

```yaml
name: FutureVuls Docker Image Scan
on:
  push:
    paths:
      - './path/to/Dockerfile'
    branches:
      - release

jobs:
  docker-test:
    name: FutureVuls Docker Image Scan
    runs-on: ubuntu-latest
    steps:
    - name: Scan Dockerfile by trivy
      uses: futurevuls/fvuls-trivy-uploader@main 
      with:
        DOCKERFILE_PATH: './path/to/Dockerfile'
        IMAGE_NAME: 'my-docker-image-name'
        FVULS_TOKEN: ${{ secrets.FVULS_TOKEN }}
        FVULS_GROUP_ID: ${{ secrets.FVULS_GROUP_ID }}
        FVULS_SERVER_UUID: ${{ secrets.FVULS_SERVER_UUID }}
```