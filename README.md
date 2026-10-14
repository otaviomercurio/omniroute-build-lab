# Manual Windows x64 laboratory build

This generic recipe builds public OmniRoute commit `c1e30b7676975feb298b49eff6ff58923c04b89e` with a reviewed NoLog patch, pinned dependency hardening, and an additional braces AST guard. It retains the complete dashboard and instrumentation. It is not an upstream release, a deployment tool, or evidence that the compiled runtime already passed acceptance.

The repository is intended to contain exactly five files:

- `.github/workflows/manual-windows-build.yml`
- `recipe-inputs.zip`
- `README.md`
- `LICENSE`
- `NOTICE`

The ZIP contains only three generic helpers, four generic patches, a pinned input manifest, laboratory release notes, and license/notice files. It contains no full source checkout, application-specific site code, images, database, credentials, operational domains or private configuration. The workflow pins and validates the ZIP SHA256 before extracting or executing helpers.

## Upload and run through the GitHub website

Operators must have the owner's authorization for the public repository and free laboratory build. Existing authorization for those actions does not need to be requested again; the switches below record the deliberate run. No PAT, local `gh`, authenticated `git push`, or external secret is required.

1. Create the approved **public** repository with Actions enabled. Confirm that the account can use a standard GitHub-hosted Windows runner without charges. Do not select a larger runner or add a paid service.
2. Upload `recipe-inputs.zip`, `README.md`, `LICENSE`, and `NOTICE` to the repository root through **Add file → Upload files**.
3. Use **Add file → Create new file**, name it `.github/workflows/manual-windows-build.yml`, and paste the reviewed YAML unchanged. Commit to the default branch.
4. Within the authorized build task, create the Actions repository variable `PUBLIC_BUILD_ENABLED` with value `true`. This is an enable switch, not a secret or proof of owner consent.
5. Open **Actions → Manual Windows x64 lab build → Run workflow** and select the explicit approval input. Without both controls, the job is skipped. No push, pull-request, schedule or automatic retry trigger exists.
6. On success, open the new **draft prerelease** tagged `lab-nolog-c1e30b7-<run>-<attempt>` under Releases. Repository maintainers can retrieve its assets while it remains a draft. Do not click Publish or deploy it before the separate runtime review.

Both build and draft upload occur in the same job, so the files are retained before the ephemeral runner disappears. Drafts are not confidential storage for secrets: only the generic runtime and provenance are uploaded. Actions logs and the source recipe in this public repository are public.

## Controls and resource boundaries

- Standard `windows-latest`, x64 Node **24.21.0** downloaded from the official origin with fixed SHA256. No larger/self-hosted runner and no third-party Actions.
- `workflow_dispatch` only; one concurrent job; 90-minute ceiling; no automatic build retry.
- At least **15 GiB physical / 12 GiB available RAM**, **12 GiB free disk before download/install**, and **8 GiB free disk before Next build**. These are admission gates, not a proof of peak resource sufficiency. A failure stops the job; it does not reduce the application or increase billing.
- Each admission gate requires **three consecutive adequate measurements, five seconds apart**, within a **60-second observation window**. Low available RAM resets the sequence. Insufficient total RAM, CPU or disk fails immediately; invalid/late measurements cannot grant admission. The same minima apply throughout. This observes post-install fluctuations without restarting installation or the build, clearing caches, or stopping other processes.
- Official `npm run build` with Webpack and 8192 MiB Node heap, then `build:cli` only after a full standalone exists. No backend-only/contributor/minimal profile, dashboard stubs, instrumentation stubs or silent Next fallback.
- Source ZIP, patch bytes, source preimages/postimages, preserved dashboard/instrumentation files, root/plugin lock hashes, **71 libc fields**, installed hardened versions and braces guard bytes are checked. Any divergence stops the run.
- The staged output must contain the dashboard, instrumentation, MCP and compression workers, and health/call-log workers. MITM management is compiled inside Next; its standalone child uses `server.cjs`, seven CJS shims and their real dependencies. The gate verifies those CJS bytes/imports, dependency entries/manifests, and five compiled Next entries. Static/public assets and required Windows native files must match the build inputs byte for byte. These checks do not prove ABI loading or runtime behavior.
- The upstream auxiliary per-file MITM TypeScript compilation can warn and copy TS sources. That compiler issue is not fixed here. The standalone consumer path above does not load those per-file `.js` outputs; the warning remains visible in logs and provenance. The recipe does not disable TypeScript or substitute a MITM stub.
- `npm ci` may execute dependency lifecycle scripts inside the isolated VM. Native dependency preflight and a synthetic SQLite `:memory:` load must pass. There are no production keys/data, provider calls, app-server startup or service deployment steps.
- No Actions artifact upload, persistent Actions cache, Packages registry, signing secret or PAT. npm uses only a transient runner-local cache. The final assets must each be smaller than **2 GiB**.
- GitHub requires `contents:write` at job scope for the draft release. The ephemeral token is passed as `GH_TOKEN` **only to the final upload step**; git fetch is anonymous and build/npm/helper steps receive no repository token explicitly.

Official runner documentation: https://docs.github.com/en/actions/reference/runners/github-hosted-runners

## Delivered assets and remaining acceptance

The draft contains `runtime-win-x64.zip` (the official staged `dist/` tree and notices), `runtime-files.json`, `provenance.json`, and `SHA256SUMS.txt`. It is not a complete npm publication or a Node installer. The provenance records the upstream commit, patch recipe commit, checksums, toolchain and runner image. It is **unsigned provenance**, not a signed attestation.

Before deployment, independently verify the generated bundle's NoLog behavior for direct and combo edits, parser diagnostics, DOMPurify server/browser behavior, KaTeX trust, AST guard behavior, Windows native loading, full dashboard, and destination-runtime compatibility. The build records copied unminified guard instances where present; this does not prove a minified/bundled guard's behavior. No existing production runtime is changed by this workflow.

The reviewed VM execution completed dependency installation, native preflight, the full Next build and CLI assembly; packaging stopped on the now-corrected MITM output contract. A complete deliverable, draft-release retention, dependency license completeness and compiled-runtime acceptance remain unproven. Every run must stop on its actual failure rather than silently repair dependency versions, lose `libc`, change models, disable features or substitute a smaller build.
