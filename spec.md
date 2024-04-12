# Spec File Guide

The package spec file (`sqlpkg.json`) describes a particular package so that `sqlpkg` can work with it.

## Minimal example

Here is a minimal working spec:

```json
{
    "owner": "sqlite",
    "name": "stmt",
    "assets": {
        "path": "https://github.com/nalgeon/sqlean/releases/download/incubator",
        "files": {
            "darwin-amd64": "stmt.dylib",
            "darwin-arm64": "stmt.dylib",
            "linux-amd64": "stmt.so",
            "windows-amd64": "stmt.dll"
        }
    }
}
```

Together `owner` and `name` define the unique package identifier. These fields are required.

The `assets.path` is a base URL for the package assets. The assets themselves are listed in the `assets.files`. When `sqlpkg` downloads the package, it chooses the asset name according to the user's operating system, combines it with the `assets.path` and downloads the asset.

At least one file in `asset.files` is required. The `path` can be omitted if there is a `repository` (more on this later).

## Typical spec

Chances are, you host your project on GitHub and use GitHub releases to publish the binaries. In that case, use the following spec template:

```json
{
    "owner": "{github_username}",
    "name": "{extension_name}",
    "version": "{current_version}",
    "repository": "https://github.com/{github_username}/{project_name}",
    "authors": ["{your_name}"],
    "license": "{license}",
    "description": "{description}",
    "symbols": ["{function_1}", "{function_2}", "{function_3}"],
    "assets": {
        "files": {
            "darwin-amd64": "{extension_name}-{version}-macos-x86.zip",
            "darwin-arm64": "{extension_name}-{version}-macos-arm64.zip",
            "linux-amd64": "{extension_name}-{version}-linux-x86.zip",
            "linux-arm64": "{extension_name}-{version}-linux-arm64.zip",
            "windows-amd64": "{extension_name}-{version}-windows-x86.zip"
        }
    }
}
```

Be sure to replace the `{placeholders}` with actual values. For example:

```json
{
    "owner": "nalgeon",
    "name": "hello",
    "version": "0.1.0",
    "repository": "https://github.com/nalgeon/sqlite-hello",
    "authors": ["Anton Zhiyanov"],
    "license": "MIT",
    "description": "Greet the world in 72 languages.",
    "symbols": ["say_hi", "say_bye", "mumble"],
    "assets": {
        "files": {
            "darwin-amd64": "hello-{version}-macos-x86.zip",
            "darwin-arm64": "hello-{version}-macos-arm64.zip",
            "linux-amd64": "hello-{version}-linux-x86.zip",
            "linux-arm64": "hello-{version}-linux-arm64.zip",
            "windows-amd64": "hello-{version}-windows-x86.zip"
        }
    }
}
```

Note that the repo name (`sqlite-hello` in the example) can be different from the extension name (`hello`).

Also note that I left the `{version}` placeholders in the `assets` section. This is intended — `sqlpkg` will automatically replace them with the main `version` field.

## Using the spec

When the spec is ready, commit it to the root of your repository as `sqlpkg.json`. This will allow the [sqlpkg manager](https://github.com/nalgeon/sqlpkg-cli) tool to find and install it like this:

```
sqlpkg install {github_username}/{extension_name}
```

Using the above example:

```
sqlpkg install nalgeon/hello
```

## Listing the extension

If you want your extension to appear on the [sqlpkg.org](https://sqlpkg.org/) website, open a PR in the [sqlpkg](https://github.com/nalgeon/sqlpkg) project.

After the extension is added to the catalog, you can link to it like this:

```
https://sqlpkg.org/?q={github_username}/{extension_name}
```

For example:

<https://sqlpkg.org/?q=nalgeon/define>

If you are into badges, you can add one like this:

```html
<a href="https://sqlpkg.org/?q={github_username}/{extension_name}">
    <img src="https://img.shields.io/badge/sqlpkg-{github_username}/{extension_name}-blue">
</a>
```

For example:

```html
<a href="https://sqlpkg.org/?q=nalgeon/define">
    <img src="https://img.shields.io/badge/sqlpkg-nalgeon/define-blue">
</a>
```

<a href="https://sqlpkg.org/?q=nalgeon/define">
    <img src="https://img.shields.io/badge/sqlpkg-nalgeon/define-blue">
</a>

---

If you have any questions — open an [issue](https://github.com/nalgeon/sqlpkg/issues/new).
