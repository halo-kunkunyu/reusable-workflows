# reusable-workflows

Reusable workflows for halo plugin and theme

## How to use?

### Plugin

#### `.github/workflows/plugin-ci.yaml`

Used to test whether the plugin can be built normally.

```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  ci:
    # Suggest using stable branch, tag or sha.
    uses: halo-kunkunyu/reusable-workflows/.github/workflows/plugin-ci.yaml@v1
```

inputs:

- `node-version`: (Optional) Version of Node.js, default is 18.
- `pnpm-version`: (Optional) Version of pnpm, default is 8.
- `java-version`: (Optional) Version of Java, default is 17.
- `ui-path`: (Optional) Path of UI project, default is "console".
- `skip-node-setup`: (Optional) Indicates if the node setup should be skipped, default is false.
- `artifacts-path`: (Optional) Artifacts path, default is build/libs. Must be a folder.
- `npm-registry-url`: (Optional) NPM registry URL.

secrets:

- `npm-auth-token`: (Optional) NPM auth token.

#### `.github/workflows/plugin-cd.yaml`

Used to build plugin and upload them to the Release and [Yunext app store](https://yunext.cn/store/apps).

> [!IMPORTANT]
> Currently, the developer center of the Yunext app store is not open to everyone, and only some developers can manage their own apps.

```yaml
name: CD

on:
  release:
    types:
      - published

jobs:
  cd:
    # Suggest using stable branch, tag or sha.
    uses: halo-kunkunyu/reusable-workflows/.github/workflows/plugin-cd.yaml@v1
    secrets:
      yunext-pat: ${{ secrets.YUNEXT_PAT }}
    permissions:
      contents: write
    with:
      # This is required for releasing to Yunext App Store.
      app-id: app-Qxhpp
```

inputs:

- `artifacts-path`: (Optional) Artifacts path, default is build/libs. Must be a folder.
- `node-version`: (Optional) Version of Node.js, default is 18.
- `pnpm-version`: (Optional) Version of pnpm, default is 8.
- `java-version`: (Optional) Version of Java, default is 17.
- `ui-path`: (Optional) Path of UI project, default is "console".
- `skip-node-setup`: (Optional) Indicates if the node setup should be skipped, default is false.
- `skip-appstore-release`: (Optional) Indicates if the appstore release should be skipped, default is false.
- `app-id`: (Optional) Application ID from Yunext App Store, default is "not-configured-app-id".
- `yunext-backend-baseurl`: (Optional) Base URL of Yunext App Store, default is "<https://yunext.cn>".
- `npm-registry-url`: (Optional) NPM registry URL.

secrets:

- `yunext-pat`: Personal Access Token for Yunext App Store, required for publishing.
- `npm-auth-token`: (Optional) NPM auth token.

### Theme

#### `.github/workflows/theme-cd.yaml`

Used to package theme and upload them to the Release and [Yunext app store](https://yunext.cn/store/apps).

> [!IMPORTANT]
> Currently, the developer center of the Yunext app store is not open to everyone, and only some developers can manage their own apps.

```yaml
name: CD

on:
  release:
    types:
      - published

jobs:
  cd:
    # Suggest using stable branch, tag or sha.
    uses: halo-kunkunyu/reusable-workflows/.github/workflows/theme-cd.yaml@v1
    secrets:
      yunext-pat: ${{ secrets.YUNEXT_PAT }}
    permissions:
      contents: write
    with:
      # This is required for releasing to Yunext App Store.
      app-id: theme-Abcde
```

inputs:

- `node-version`: (Optional) Version of Node.js, default is 20.
- `pnpm-version`: (Optional) Version of pnpm, default is 10.
- `skip-appstore-release`: (Optional) Indicates if the appstore release should be skipped, default is false.
- `app-id`: (Optional) Application ID from Yunext App Store, default is "not-configured-app-id".
- `yunext-backend-baseurl`: (Optional) Base URL of Yunext App Store, default is "<https://yunext.cn>".

secrets:

- `yunext-pat`: Personal Access Token for Yunext App Store, required for publishing.

Please note that if you need to use this workflow to publish themes, your theme repository must use pnpm for package management and include a build script. The build script must include `npx @halo-dev/theme-package-cli`. For information about npx @halo-dev/theme-package-cli, please visit: [halo-dev/theme-package-cli](https://github.com/halo-dev/theme-package-cli), for example:

```json
{
  "name": "@halo-dev/theme-foo",
  "scripts": {
    "build": "npx @halo-dev/theme-package-cli"
  },
}
```

If your theme includes other build processes, you need to put them before `npx @halo-dev/theme-package-cli`, for example:

```json
{
  "name": "@halo-dev/theme-foo",
  "scripts": {
    "build": "vite build && npx @halo-dev/theme-package-cli"
  },
}
```
