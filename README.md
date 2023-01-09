# A github action for building a Writrside doc artifacts

This action creates zip-archive with HTMLs  from markdown or semantic markup topics.

## Environment variables

### `ARTIFACT`

The name of the archive is webHelpXX2-all.zip where XX gets replaced by the product id in caps.

### `PRODUCT`

The $PRODUCT should be name_of_module / product_id, for example sample_module/sd. No default value.

## Example usage with result in artifact

```yml
name: Build docs

on:
  push:
    branches: ["main"]

  workflow_dispatch:

env:
  PRODUCT: stardust-help/pd
  ARTIFACT: webHelpPD2-all.zip

jobs:
  build-job:
    runs-on: ubuntu-latest
    container: 
      image: registry.jetbrains.team/p/writerside/builder/writerside-builder:2.1.984
    steps:
      - name: Build docs
        uses: lananovikova10/writerside-actions@v2.4
```
