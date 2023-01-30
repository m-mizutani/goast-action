# Goast Action

Run [goast](https://github.com/m-mizutani/goast) for static analysis of your Go source code with policy language [Rego](https://www.openpolicyagent.org/docs/latest/policy-language/).

## Usage

```yaml
name: goast

on:
  pull_request:

jobs:
  eval:
    runs-on: ubuntu-latest
    steps:
      - name: checkout
        uses: actions/checkout@v2
      - uses: reviewdog/action-setup@v1
      - name: goast
        uses: m-mizutani/goast-action@main
        with:
          policy: ./.goast
          format: json
          output: fail.json
          source: ./pkg
      - name: report
        env:
          REVIEWDOG_GITHUB_API_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: cat fail.json | reviewdog -reporter=github-pr-review -f rdjson
```

## Input

- `policy`: Directory of Rego policy file(s)
- `format`: Output format. Choose one from `text` and `json`. `json` schema is according to [reviewdog](https://github.com/reviewdog/reviewdog)
- `output`: Output file
- `source`: Directory or file to be analyzed
- `root_only`: Disable recursive inspection of Node. Check only top level Node (basically `ast.File`)
- `fail`: Exit with non-zero code when violation is detected
- `ignore_auto_generated`: Ignore auto generated codes if 'true' value passed.

## License

Apache License v2.0
