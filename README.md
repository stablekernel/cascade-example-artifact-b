# cascade-example-artifact-b

Example source repository that demonstrates external artifact tracking with [cascade](https://github.com/stablekernel/cascade).

This repo builds a widget-b artifact and notifies the primary pipeline (`stablekernel/cascade-example-primary`) after each successful build, enabling the primary to track widget-b as an external source alongside other artifact sources.

## How it works

1. A push to `src/` triggers the cascade orchestrate workflow.
2. The orchestrate workflow calls the `build-widget.yaml` reusable callback, which builds and uploads `widget-b`.
3. After the build, cascade dispatches an External Update event to the primary repository, writing the new sha and version into the shared manifest.

## Reusable callbacks

| Workflow | Purpose |
|----------|---------|
| `.github/workflows/build-widget.yaml` | Build and upload the widget-b artifact. Accepts `environment`, `sha`, `version` inputs; emits `artifact_name` output. |
| `.github/workflows/deploy-artifact-b.yaml` | Deploy widget-b to a target environment. Referenced by the primary for external deploys. |

## Secrets

`CASCADE_STATE_TEST_TOKEN` is a fine-grained PAT with Contents RW and Actions RW on the cascade-example fleet repos.

## Scenario suite

`.github/workflows/scenario-suite.yaml` validates the build and dispatch path. Trigger manually via `workflow_dispatch` or via `repository_dispatch` with type `run-scenario-suite`.
