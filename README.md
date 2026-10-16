# Manual Windows x64 laboratory build

This public recipe compiles OmniRoute commit `c1e30b7676975feb298b49eff6ff58923c04b89e`
with nine source patches: NoLog, pinned dependency hardening, minimal lock changes,
disposable Webpack-cache control, custom image cancellation, and native image
streaming R2, the dedicated Artes Stage1 profile, disabled asynchronous image jobs,
and the Jobs stream boundary. The installed braces AST guard and two exact approved Playwright
runtime backports are separate, for twelve patch files in the payload.
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
The Base64 file is 99,297 bytes: one canonical ASCII line plus LF.
It decodes to 74,471 bytes, 24 allowlisted entries, SHA256
`d6ac46b4bcd1f837b89d846f80d94ecd64ce41566d4dd13f68a90a1f23254043`. The workflow verifies the decoded hash, exact entry set,
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

The stream patch is 699756da4a7c8acb6d162c9aca0f46e842e21b609769681ab1367f506e8023a5. Its source manifest is eb9da322fc42677798453c04890490d36e67d098d4a4ac8f4457b2a61006644a. Those original R2 limits remain unchanged for the legacy native-v1 alias. The separate Stage1 profile below requires a new build and its own compiled acceptance; no provider invocation, production update or new paid resource is performed by this recipe.

## Dedicated Artes Stage1 source history

The seventh source stage applies `native-artes-stage1.patch` after the exact Build9 stream sources. Four Build9 preimages and all five final stream-related sources are byte/hash gated. The cumulative nine changed upstream sources remain in `build-inputs.json`; no earlier patch or vendor repair is replaced.

Only the exact alias `fernandes-native-images/realesrgan-x4plus-4x-png-artes-large-v1`, with restricted model access and NoLog, selects the new 128 MiB output envelope. The old native-v1 alias remains 16 MiB. The image model, x4 scale and 120 s deadline stay unchanged; this build neither changes Native/Site limits nor provisions connections or keys.

Stage1 patch SHA256: `1f430d8fb538986f5ec1bd74f2ff278976fcd7d30d40a02c929683f5e982e3b1`. Full source manifest SHA256: `8a6997bff54717032cc1b46c17a1256901e040423e8344d007b451f505e26ed6`. Candidate manifest SHA256: `8cae3a51504be1435508bba8c0499d453d2d938cd363710c1c79d6e7f4653598`. Nine focused tests passed before the final built-in-provider rejection guard; the final source has a static check only. The final focused suite, compiled HTTP acceptance and coordinated Site/Native capacity validation remain pending. Do not use prior Build9 acceptance as proof of Stage1 acceptance. The existing acceptance workflows and their payloads are preserved byte for byte.

## Disabled asynchronous image jobs Stage2 source history

The eighth source stage adds exactly three new files over the verified Build10 recipe commit `2b1821bdd573340f72cfc357afc6830b7d8a17cd`: the pure jobs handler, its TypeScript declarations and the Next optional catch-all route. Each path must be absent before applying the patch. Their intermediate bytes/hashes at this stage are verified together with every unchanged Stage1 source and previous dependency/vendor gate. The existing synchronous routes and their limits remain unchanged.

The feature stays disabled unless separately configured with an explicit allowed existing key, one existing provider connection and the existing restricted Images model/NoLog policy. This build creates no key, model, connection or environment file. Only the new source patch is included; local test images, credentials, logs, Site sources and runtime data are absent.

The source candidate passed pure transport tests and a synthetic loopback composition using the actual Build10 NextRequest adapter. That does not prove compiled-route behavior or maximum capacity. The recipe now requires the compiled jobs route manifest entry and nonempty output file, and records its hash. Compiled authenticated HTTP acceptance, the public proxy and any larger real inference remain separate gates. No production capacity guarantee or feature activation follows from a green build.

## Build12 Jobs stream boundary


Build12 source boundary (prepared separately; not deployment approval)
The three-file Jobs stream boundary retains the same authz admission pipeline
inside the Jobs route and excludes only that namespace from proxy matching.
All 11 previous patches and feature-default/limits/quality/deadlines remain.
Original nativeImageJobsStage2 provenance records its intermediate route hash;
jobsStreamBoundary and changedSources record the final route/proxy/pipeline.
Output gates pin the newly compiled route, Node middleware and both manifests,
compare all matchers and require the continuation marker. These static gates
do not prove authenticated HTTP, alias rewrites or public headers. Compiled
acceptance (including unchanged early replay deadline) remains PENDING.
