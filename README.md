# diffyml-action

GitHub Action to compare YAML files structurally using [diffyml](https://github.com/szhekpisov/diffyml).

## Usage

```yaml
- uses: szhekpisov/diffyml-action@v1
  with:
    from: old.yaml
    to: new.yaml
```

Detect drift without failing the workflow:

```yaml
- uses: szhekpisov/diffyml-action@v1
  id: diff
  with:
    from: expected.yaml
    to: actual.yaml
    fail-on-diff: 'false'

- if: steps.diff.outputs.has-differences == 'true'
  run: echo "Configuration drift detected"
```

Pass any CLI flags via `extra-args`:

```yaml
- uses: szhekpisov/diffyml-action@v1
  with:
    from: manifests-main/
    to: manifests-pr/
    extra-args: '--ignore-api-version --filter spec.replicas'
```

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `from` | *(required)* | Source YAML file or directory |
| `to` | *(required)* | Target YAML file or directory |
| `version` | `latest` | diffyml version (e.g. `1.5.10`) |
| `output` | `github` | Output format (`github`, `json`, `compact`, etc.) |
| `fail-on-diff` | `true` | Fail the step if differences found |
| `cache` | `true` | Cache the binary across runs |
| `extra-args` | | Additional CLI flags |

## Outputs

| Output | Description |
|--------|-------------|
| `exit-code` | `0` (no diff), `1` (diff found), or `>1` (error) |
| `has-differences` | `true`, `false`, or empty on error |
| `diff` | Raw diff output (truncated to 1 MB) |

## License

MIT
