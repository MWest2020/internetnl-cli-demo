# internetnl-cli-demo

A tiny, runnable demo of [**internetnl-cli**](https://github.com/MWest2020/internetnl-cli) —
a command-line client for the [Internet.nl](https://internet.nl) batch API,
and **netnl**, a multi-tenant facade in front of a self-hosted batch instance.

## How it works

**▶ [Live measurements (updated daily)](https://mwest2020.github.io/internetnl-cli-demo/)** —
three real hosts, measured every day by a GitHub Action running the unmodified
CLI against `https://api.westerweel.work`; the page renders the CLI's own
`--json` output, committed to [`docs/data/latest.json`](docs/data/latest.json).

An illustrated walk-through of the whole flow — host list → CLI → netnl facade
→ batch instance → verdicts, plus the commands and the deploy topology:

**▶ [How internetnl-cli works (live page)](https://claude.ai/code/artifact/82279ff8-2a68-43ee-a7b5-2d8fa0ebfa12)**

## Quickstart

```sh
# 1. Install the CLI (Python via uv)
uv tool install git+https://github.com/MWest2020/internetnl-cli

# 2. Point it at an endpoint — the hosted API or a netnl facade
export INTERNETNL_ENDPOINT=https://netnl.<tailnet>.ts.net
export INTERNETNL_USERNAME=you INTERNETNL_PASSWORD=…

# 3. Measure your fleet
internetnl submit --file hosts.example.txt          # prints a request-id, then polls
internetnl results <request-id> --json > out.json   # machine-readable
```

There is **no default endpoint** — you configure where you measure. Switching
between the hosted instance and your own is a change of `INTERNETNL_ENDPOINT`,
nothing else.

## As a CI gate

```sh
internetnl submit --file hosts.example.txt --json --fail-on-scored
# exit 3 when a scored subtest fails; informational findings never gate.
```

| exit | meaning |
|---|---|
| 0 | success |
| 1 | configuration error |
| 2 | transport / API error |
| 3 | `--fail-on-scored` gate tripped |
| 4 | poll timeout |

## The rule

**Only measure hosts you operate or have explicit permission to test.**
The tool does not enforce this; you do. Batch results are not identical to the
website's — the endpoint is named on every result, and a batch verdict is never
"the internet.nl score" on its own.

## Links

- Client + facade source: <https://github.com/MWest2020/internetnl-cli>
- Self-hosting & deploy: see `docs/` in the main repo
- Independent instance — not affiliated with Internet.nl / Platform Internetstandaarden.

_MIT licence._
