# Build Action

This GitHub Action is designed to build a Docker image from a specified Dockerfile and, optionally, push it to a Docker registry.

## Usage

To use this action in your workflow, add the following step to your GitHub Actions workflow file (e.g., `.github/workflows/build.yml`):

```yaml
- name: Build and Push Docker Image
  uses: paritytech/build-n-push-action@main
  with:
    push_to_registry: "true" # or 'false' if you do not wish to push (for example if you testing build without pushing to registry)
    registry_user: ${{ secrets.REGISTRY_USER }}
    registry_password: ${{ secrets.REGISTRY_PASSWORD }}
    registry_url: "registry.parity.io/parity-internal/" # optional, defaults to 'registry.parity.io/parity-internal/'
    image_name: "your-image-name" # optional, defaults to GitHub repository
    dockerfile_path: "./path/to/Dockerfile" # optional, defaults to './Dockerfile'
    build_args: | # optional
      ARG1=value1
      ARG2=value2
    context: "./path/to/context" # optional, defaults to '.'
```

## Inputs

| Name                | Description                                                          | Required | Default                                                                  |
| ------------------- | -------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------ |
| `push_to_registry`  | Whether to push the image to registry ('true' or 'false' as string). | Yes      | N/A                                                                      |
| `registry_user`     | Registry username (if push_to_registry is 'true').                   | No       | N/A                                                                      |
| `registry_password` | Registry password (if push_to_registry is 'true').                   | No       | N/A                                                                      |
| `registry_url`      | Registry url.                                                        | No       | 'registry.parity.io/parity-internal/'                                    |
| `image_name`        | Image name.                                                          | No       | GitHub repository name                                                   |
| `dockerfile_path`   | Dockerfile path.                                                     | No       | './Dockerfile'                                                           |
| `build_args`        | Build arguments. Default includes VCS_REF and BUILD_DATE.            | No       | VCS_REF=${{ github.sha }}<br>BUILD_DATE=$(date -u '+%Y-%m-%dT%H:%M:%SZ') |
| `context`           | Build context.                                                       | No       | '.'                                                                      |

## Outputs

- `tag`: The tag of the built image.

## Example Workflow

Here's a complete example of a workflow using this action:

```yaml
name: Build and Deploy

on:
  push:
    branches: - main

jobs:
build_and_push:
runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build and Push Docker Image
        uses: paritytech/build-n-push-action@main
        with:
          push_to_registry: "true"
          registry_user: ${{ secrets.REGISTRY_USER }}
          registry_password: ${{ secrets.REGISTRY_PASSWORD }}
```
