# Manual Windows x64 laboratory build

This public recipe compiles OmniRoute commit `c1e30b7676975feb298b49eff6ff58923c04b89e`
with six source patches: NoLog, pinned dependency hardening, minimal lock changes,
disposable Webpack-cache control, custom image cancellation, and native image
streaming R2. The installed braces AST guard and two exact approved Playwright
runtime backports are separate, for nine patch files in the payload.
The full dashboard, instrumentation, models and application timeouts are retained.
This is a laboratory candidate, not an upstream release or a deployment.

## Repairs in this candidate

The prior complete build produced a byte-verified package. Offline review found
that the compiled docs page requires JSDOM, but its package was not staged, and
that Monaco embeds DOMPurify 3.4.8 despite the root dependency being 3.4.16.

The new helper stages the exact lock-resolved JSDOM runtime closure needed by the
compiled docs consumer. It fills only missing files, rejects conflicting files
and links, and checks that dependency resolution remains inside the distribution.
It does not add packages merely to satisfy a test harness. The existing selfsigned
closure repair and MITM checks remain active and unchanged.

Before Next compilation, a separate helper replaces only the pinned Monaco vendor
ESM with the official installed DOMPurify 3.4.16 ESM and its source map. The Monaco
import path remains unchanged. No alias shares a sanitizer singleton with docs.
Pinned preimages, postimages, package manifests, importer and lock must match.

After CLI assembly, an AST-only gate inspects compiled server and browser outputs,
identifies sanitizer factories and rejects unexpected versions or ambiguous
implementations. It does not execute the bundles. Receipts include file/slice
hashes and explicitly leave behavioral acceptance pending.

## Exact public transport

Five active files: `.github/workflows/manual-windows-build.yml`,
`recipe-inputs.zip.b64`, `README.md`, `LICENSE`, and `NOTICE`.
The Base64 file is 75,329 bytes: one canonical ASCII line plus LF.
It decodes to 56,495 bytes, 21 allowlisted entries, SHA256
`1eb99c63d2ba33a5a80c106d7ad7cc9698a26afeb01948ed55214c947c4facff`. The workflow verifies the decoded hash, exact entry set,
CRC, extraction paths and bounds before running any helper. No private source,
site files, images, database, credentials or production configuration is present.
An older binary ZIP in the repository is inactive and is not used as a fallback.

## Deliberate free laboratory execution

Only manual `workflow_dispatch` is enabled, with the owner-authorized input and
repository variable `PUBLIC_BUILD_ENABLED=true`. There are no push, pull-request,
schedule or retry triggers. Commit all reviewed transport files together before
dispatch. Use only the standard public-repository `windows-latest` runner.

The existing resource gates remain: 15 GiB physical / 12 GiB available RAM,
12 GiB free disk before install, 8 GiB before the full build; three consecutive
adequate readings five seconds apart within 60 seconds. Node 24.21.0 is pinned
by official-archive SHA256. The full Webpack build keeps an 8192 MiB heap and is
followed by the official build:cli. The gate failures stop the job. They do not
reduce the application, change models, increase timeouts or activate billing.

The root lock, source patches, 71 libc fields, preserved dashboard/instrumentation,
native file bytes and static/public assets are verified. Build diagnostics are
numeric only and are excluded from the runtime. Dependency lifecycle scripts run
inside the disposable runner. No app server, production database or provider is
used. `contents:write` is job-scoped; the ephemeral token is exposed explicitly
only in the final draft-release upload step. No PAT, Actions cache, artifact
storage, Packages upload, signing secret or deployment step is used.

## Results and acceptance

The output is a new draft prerelease tagged
`lab-nolog-c1e30b7-<run>-<attempt>` with `runtime-win-x64.zip`,
`runtime-files.json`, `provenance.json`, and `SHA256SUMS.txt`. Keep it as a draft.
Provenance is unsigned. A green build and correct hashes prove packaging only.
Compiled NoLog behavior, sanitizer/XSS behavior, native loading, full dashboard
and destination-runtime compatibility must pass separately before installation.
Build-time dependencies not shipped in the runtime must not be inserted just to
make acceptance tests pass; their source/build coverage is reported separately.
The original auxiliary MITM TypeScript warning remains visible; this recipe
verifies its actual standalone consumer rather than substituting a stub.

Official runner reference: https://docs.github.com/en/actions/reference/runners/github-hosted-runners

## Custom image edit cancellation

The reviewed cancellation patch follows the earlier source patches. It forwards the client AbortSignal into the custom image edit request and stops canceled combo execution before another connection is resolved or dispatched. Existing120s timeout, model selection, NoLog and build gates remain. Eleven source-level tests passed with real loopback sockets; compiled HTTP disconnect acceptance remains pending. This is not a Site deployment or a change to any live provider.

## Native image stream R2 proposal

The sixth source patch adds the opt-in native image stream on top of Build8 cancellation. The five previous source patches, installed braces guard, dependency closures and sanitizer checks remain. Two exact approved Playwright vendor backports are preserved during final packaging with pre/postimage gates. Unknown vendor bytes stop the job; the recipe never copies a local runtime wholesale.

The stream patch is 699756da4a7c8acb6d162c9aca0f46e842e21b609769681ab1367f506e8023a5. Its source manifest is eb9da322fc42677798453c04890490d36e67d098d4a4ac8f4457b2a61006644a. The full build and runtime acceptance are still pending; no increase in image limits, provider invocation, production update or new paid resource is part of this recipe.
