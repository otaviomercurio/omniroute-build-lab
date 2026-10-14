# Manual Windows x64 laboratory build — text transport

This generic recipe builds public OmniRoute commit `c1e30b7676975feb298b49eff6ff58923c04b89e` with a reviewed NoLog patch, pinned dependency hardening, and an additional braces AST guard. This candidate also disables only the disposable production Webpack compilation cache and adds bounded numeric build diagnostics. The cache hypothesis is not a proven fix for heap exhaustion. It retains the complete dashboard and instrumentation. It is not an upstream release, a deployment tool, or evidence that the compiled runtime already passed acceptance.

This recipe uses exactly five active public files:

- `.github/workflows/manual-windows-build.yml`
- `recipe-inputs.zip.b64`
- `README.md`
- `LICENSE`
- `NOTICE`

The ZIP contains only four generic helpers, five generic patches, a pinned input manifest, laboratory release notes, and license/notice files. It contains no full source checkout, application-specific site code, images, database, credentials, operational domains or private configuration. The workflow strictly decodes the text file and validates the original ZIP SHA256 before extracting or executing helpers.

## Alternative transport, unchanged payload

The binary-upload UI could not retain the attachment. This alternative uses the normal GitHub text editor; it does not use authenticated APIs or a different download origin. `recipe-inputs.zip.b64` is one canonical RFC4648 Base64 line followed by exactly one LF. Its 35,801 ASCII bytes decode to the exact previously reviewed 26,849-byte ZIP with SHA256 `3e15efeca0c2748b7d06d233b2beb3dabc0d45a752c505b5ecc57633be55f3f5` and the same 13 entries. No helper, patch, build input, permission, resource gate or runtime behavior changes.

The workflow rejects whitespace, line wrapping, missing or extra newlines, BOM, invalid padding, noncanonical padding bits and a changed decoded ZIP. It then applies the existing hash, entry allowlist, CRC, extraction and build gates. Base64 is transport encoding, not encryption.

If an older `recipe-inputs.zip` is already in the repository, preserve it. It is an inactive sixth file: this workflow reads only `recipe-inputs.zip.b64` and cannot fall back to the older binary. The five active text files above remain the complete transport recipe. No new build should start until the reviewed text and workflow commits are present together.

## Upload and run through the GitHub website

Operators must have the owner's authorization for the public repository and free laboratory build. Existing authorization for those actions does not need to be requested again; the switches below record the deliberate run. No PAT, local `gh`, authenticated `git push`, or external secret is required.

1. Create the approved **public** repository with Actions enabled. Confirm that the account can use a standard GitHub-hosted Windows runner without charges. Do not select a larger runner or add a paid service.
2. Through **Add file → Create new file**, create `recipe-inputs.zip.b64` at the repository root and paste its entire canonical ASCII line, including its final LF. Create or update `README.md`, `LICENSE`, and `NOTICE` as normal text files. Do not wrap the Base64, add a BOM, or include Markdown fences.
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

## Controlled build-cache experiment and numeric diagnostics

The only new application-source change is `if (!dev) config.cache = false` inside the existing Webpack callback. This cache stores disposable compiler intermediates. It is **not** the gateway response cache, a site cache, a database, or model context. Pages, entrypoints, instrumentation, minification, plugins, aliases, models, quality and application timeouts remain present. A fresh build may take longer, and output hashes may differ. The experiment does not establish that caching caused the prior OOM or guarantee sufficient memory.

The source preimage and postimage of `next.config.mjs` and the new patch are explicitly pinned; it is no longer described as an unchanged source file. Existing NoLog, dependency, lock/libc, AST and complete-runtime gates remain enabled. Heap remains 8192 MiB and the physical resource minima are unchanged.

`memory-probe-preload.cjs` is enabled for a numeric Node preflight and the existing `npm run build` command (including its upstream pre/postbuild hooks), after installation and resource admission. It records fixed numeric JSON metadata: heap limit/usage, RSS, available memory, PID/PPID/thread and monotonic/UTC time. It never emits raw environment, arguments, paths, file contents, heap snapshots or credentials. The observer samples every five seconds, caps periodic samples at 180 per process, and uses an unreferenced timer. A fatal OOM may prevent its final event; missing measurements are not zero or proof of safety. It does not terminate builds or prevent OOM.

The original Node options are restored in `finally` before CLI assembly/packaging. The packager rejects the diagnostic file in `dist`. Compiler output and all acceptance requirements still apply. This candidate requires a reviewed deliberate trial; it does not automatically repeat any failed workflow.

Primary documentation: [Next.js memory guide](https://nextjs.org/docs/app/guides/memory-usage), [Webpack cache configuration](https://webpack.js.org/configuration/cache/). The standard memory optimizations and build worker were already enabled; this experiment retains them.

## Delivered assets and remaining acceptance

The draft contains `runtime-win-x64.zip` (the official staged `dist/` tree and notices), `runtime-files.json`, `provenance.json`, and `SHA256SUMS.txt`. It is not a complete npm publication or a Node installer. The provenance records the upstream commit, patch recipe commit, checksums, toolchain and runner image. It is **unsigned provenance**, not a signed attestation.

Before deployment, independently verify the generated bundle's NoLog behavior for direct and combo edits, parser diagnostics, DOMPurify server/browser behavior, KaTeX trust, AST guard behavior, Windows native loading, full dashboard, and destination-runtime compatibility. The build records copied unminified guard instances where present; this does not prove a minified/bundled guard's behavior. No existing production runtime is changed by this workflow.

An earlier VM execution completed the full Next build and CLI assembly, then stopped on the now-corrected MITM output contract. A subsequent run exhausted the V8 heap during Webpack before packaging. This candidate tests the cache hypothesis with numeric observations; a complete deliverable, draft-release retention, dependency license completeness and compiled-runtime acceptance remain unproven. Every run must stop on its actual failure rather than silently repair dependency versions, lose `libc`, change models, disable features or substitute a smaller build.
