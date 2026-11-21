# Secure AI Agent Platform (archived)

> **Status: archived, November 2026.** The cloud cluster is deleted and nothing here is running. The code, the docs and the recorded evidence stay as a portfolio record.

A private platform on Google Kubernetes Engine where AI agents run in separate, locked-down spaces. Each agent can reach only what it is allowed to reach: no internet, its own data rows only, personal data masked before it reaches a model, and changes only after the owner approves them.

![The platform map in the Console: three agents, each in its own space, behind one model gateway and redactor](docs/evidence/console-layout-2026-10-18/after/1-map.png)

## What it did

| Part | What it did | Where to look |
| --- | --- | --- |
| Model gateway | Every model request goes through one gateway, then a redactor, then the private model path. Workers cannot bypass the masking. | [architecture](docs/architecture.md) |
| Masking | Supported identifiers (emails, phone numbers, card numbers, IBANs, Saudi national IDs) are masked. Person names and passport numbers are **not** masked. | [masking scope](docs/security/masking-scope.md) |
| Data access | The Financial agent reads only the signed-in user's rows (PostgreSQL row-level security). Workers hold no database or signing credentials. | [claims matrix](docs/agent-platform/claims-matrix.md) |
| Approvals | The Ops agent only observes and proposes. A separate executor applies a change only after the owner's signed approval, which cannot be replayed. | [claims matrix](docs/agent-platform/claims-matrix.md) |
| Attack tests | A suite of real attacks against the running platform. The saved record is [attack-suite.json](docs/evidence/attack-suite.json) (19 of 19 stopped, 19 October 2026). | [evidence](docs/evidence/) |

What it did not do is listed in [known limitations](docs/security/known-limitations.md).

## Why it is archived

Every safeguard was a good idea on its own: a hash for each config file, seven container images bound to one commit, a release lock, signed digests. I approved each one. Together they made the project so rigid that a one-line change meant rebuilding, signing and re-qualifying all seven images. The cost of keeping it running was higher than what it gave back, so I stopped.

What I would do differently:

1. Apply strict locks only to the parts that are finished and stable, not to a project that is still changing.
2. Before adding any new mechanism, ask what it will cost every time something changes.
3. Build what I can explain and change myself.

## A note on the numbers

The story video shows a later **Break it** run with 17 attacks and 6 controls. That run was recorded on screen, but its results file was not saved to this repository. The saved suite record is the 19-of-19 run above. Earlier runs, including ones that did not fully pass, are kept in [docs/evidence](docs/evidence/) as they were recorded.

## Repository map

| Folder | Contents |
| --- | --- |
| `platform/` | The Console, the Financial agent, the Platform Guide, and the GKE deployment code |
| `runtime/`, `contracts/`, `policy/`, `providers/` | The agent runtime, its contracts and policies |
| `tests/`, `evaluation/` | Offline tests and qualification records |
| `docs/` | Architecture, security, evidence, and [history](docs/history/) of finished operations |

More detail is in the [repository map](docs/REPO-MAP.md). Removed tooling (`assessment_overlay/`, `infra/gcp/`, `archive/`) is in git history at commit `2e2312b`.

## Running the tests

`make test` runs the offline suite (Python 3.12, Go, Node, Docker for Postgres). Deployment commands (`make up` and the rest) need the deleted cloud project and will not work.

## Credits and license

The Python runtime, contracts, infrastructure manifests and qualification fixtures were adapted from my contribution to the MASAR AI Infrastructure team project. Its product frontend and backend remain with their original contributors. Third-party projects keep their own names and licenses. Released under the [MIT License](LICENSE). Examples and tests use synthetic data only.
