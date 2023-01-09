# A github action for building a Writerside doc artifacts

This action creates a zip-archive with HTMLs from markdown or semantic markup topics.

## Environment variables

### `ARTIFACT`

The name of the archive is webHelpXX2-all.zip where XX gets replaced by the product id in caps.

### `PRODUCT`

The $PRODUCT should be name_of_module / product_id, for example sample_module/sd. No default value.

## Example usage with the build HTMLs only

```yml
name: Build docs

on:
  push:
    branches: ["main"]

  workflow_dispatch:

env:
  PRODUCT: module/instance-id
  ARTIFACT: webHelpXX2-all.zip

jobs:
  build-job:
    runs-on: ubuntu-latest
    container: 
      image: registry.jetbrains.team/p/writerside/builder/writerside-builder:2.1.984
    steps:
      - name: Build docs
        uses: lananovikova10/writerside-actions@v2.4
```


## Example usage with build and publish to GitHub pages

```yml
name: Build and publish docs

on:
  push:
    branches: ["main"]

  workflow_dispatch:
  
# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

env:
  PRODUCT: module/instance-id
  ARTIFACT: webHelpXX2-all.zip

jobs:
  build-job:
    runs-on: ubuntu-latest
    container: 
      image: registry.jetbrains.team/p/writerside/builder/writerside-builder:2.1.984
    steps:
      - name: Build docs
        uses: lananovikova10/writerside-actions@v2.4
          
  deploy:
    environment:
        name: github-pages
        url: ${{ steps.deployment.outputs.page_url }}
    needs: build-job
    runs-on: ubuntu-latest
    steps:
      - name: Download artifact
        uses: actions/download-artifact@v3
        with:
          name: artifact
      - name: Unzip artifact
        uses: montudor/action-zip@v1
        with:
          args: unzip -qq ${{ env.ARTIFACT }} -d dir
      - name: Setup Pages
        uses: actions/configure-pages@v2
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: dir
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
          
```
