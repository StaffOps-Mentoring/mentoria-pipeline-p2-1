# Handoff

Last updated: 2026-11-23 by Backstage docs-standard rollout · branch `main`

## State

**Lab template repo** (not a student's solution): state = what the P2.1 lab currently hands to students.
`main` holds the vitrine Go app (`main.go`, `config.go`, `rotas.go`, identical to
`mentoria-apps/apps/vitrine` today but without its tests), the docker track's multi-stage Dockerfile
(distroless, non-root), and `chamado.txt` with a placeholder for the student's token. There is no workflow:
the student writes the one that publishes the image to GHCR. The catalog marks it `type: template`,
`lifecycle: production`. GitHub Actions is disabled on this repo and it has no runs, tags or releases.
Per the mentoring repo's pipelines handoff, P2.1 is ready and was measured on real GitHub (12 verify items),
with amd64 plus arm64 images required by owner decision (2026-11-03). Nothing is in flight. The working log
is `docs/handoff/pipelines.md` in the private `StaffOps-Mentoring/mentoria-roadmap`.

## Done

- Backstage catalog and technology tags ([`8baa5fe`](https://github.com/StaffOps-Mentoring/mentoria-pipeline-p2-1/commit/8baa5fe), [`b7a4bb0`](https://github.com/StaffOps-Mentoring/mentoria-pipeline-p2-1/commit/b7a4bb0))
- The P2.1 template: vitrine with the docker track's Dockerfile, no workflow ([`2d55362`](https://github.com/StaffOps-Mentoring/mentoria-pipeline-p2-1/commit/2d55362))

## In progress

- No open pull requests and no open issues. Check with `gh pr list -R StaffOps-Mentoring/mentoria-pipeline-p2-1`.

## Next steps

1. None recorded for this repo. Changes to the lab start in `mentoria-roadmap` and are mirrored here
   (suggested, inferred from where the lab statement and verify live).

## Decisions

No ADRs here. Lab decisions (amd64 plus arm64, the SHA as image tag and `/version`) are recorded in
`mentoria-roadmap`.

## Risks and open questions

- The app is a copy of `mentoria-apps/apps/vitrine`; a fix there needs a deliberate copy here (and in
  `mentoria-pipeline-p2-2`).
- Without tests in the repo, a change to the app is checked only by `go vet`, a build and the lab's verify.

## How to verify

```bash
gh api repos/StaffOps-Mentoring/mentoria-pipeline-p2-1/actions/permissions --jq .enabled   # false
ls .github 2>/dev/null || echo "no workflow (expected)"
docker run --rm -v "$PWD:/src" -w /src golang:1.26-alpine sh -c 'go vet ./... && go build -o /tmp/app .'
docker build --build-arg VERSAO=dev -t vitrine:dev .
docker run --rm -p 8080:8080 -e LOJA_NOME=demo vitrine:dev   # then: curl localhost:8080/version
```
