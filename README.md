# tcloud-installer

Composite GitHub Action that installs the [tcloud](https://github.com/thalassa-cloud/cli) CLI for Linux, macOS, and Windows runners and prepends the install directory to `PATH` for subsequent steps. Release binaries are downloaded from [thalassa-cloud/cli releases](https://github.com/thalassa-cloud/cli/releases).

## Usage

```yaml
jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - uses: thalassa-cloud/tcloud-installer@v1
      - run: tcloud version
```

Pin the action to a tag or commit SHA in production workflows.

### Install a specific release

```yaml
- uses: thalassa-cloud/tcloud-installer@v1
  with:
    tcloud-release: v0.18.0
```

### Install from `main` with Go

Requires Go on the runner (for example via [`actions/setup-go`](https://github.com/actions/setup-go)):

```yaml
- uses: actions/setup-go@v5
  with:
    go-version: '1.25'
- uses: thalassa-cloud/tcloud-installer@v1
  with:
    tcloud-release: main
```

### Custom install directory

`install-dir` supports environment-style values; they are expanded with `envsubst` (for example `$HOME`).

```yaml
- uses: thalassa-cloud/tcloud-installer@v1
  with:
    install-dir: $HOME/.local/bin
```

### System-wide install (needs sudo)

```yaml
- uses: thalassa-cloud/tcloud-installer@v1
  with:
    install-dir: /usr/local/bin
    use-sudo: true
```

## Inputs

| Input | Description | Default |
| --- | --- | --- |
| `tcloud-release` | `latest` (default), a semver tag such as `v0.18.0`, or `main` to build with `go install` | `latest` |
| `install-dir` | Directory where the `tcloud` binary is placed | `$HOME/.tcloud` |
| `use-sudo` | Set to `true` if writes to `install-dir` require elevated permissions | `false` |

For `tcloud-release`, only semver-shaped tags are accepted when not using `latest` or `main` (for example `v1.2.3` or tags with a prerelease suffix).

## Supported platforms

Prebuilt archives are used for:

- Linux: `amd64`, `arm64`
- macOS: `amd64`, `arm64`
- Windows: `amd64`, `arm64`

## License

See [LICENSE](LICENSE) in this repository.
