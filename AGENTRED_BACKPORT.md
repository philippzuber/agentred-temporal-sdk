# agentRed dispatcher correction

This branch starts at the unmodified Temporal Go SDK v1.50.0 tag,
`e43cbba8d80f064ca93e4a8ebd9cc2e6dd15d024`.

It backports only the dispatcher correction and regression from
[temporalio/sdk-go#2853](https://github.com/temporalio/sdk-go/pull/2853),
reviewed at `5f16e710f1928550896457306d87946486cf9f43`.
The upstream MIT licence remains unchanged. The upstream pull request was
still open on 2026-11-13; this is an agentRed-maintained backport, not an
upstream release or upstream approval claim.

The changes synchronise deadlock closure with late coroutine creation,
refuse registering a child of an already closed parent, and snapshot the
coroutine list before asynchronous shutdown. Deadlock detection, timeouts,
race checks and workflow history semantics remain enabled.

Validation: the added test reproduces the race on v1.50.0; the patch passes
30 race repetitions and the entire SDK module's race suite (22 tested
packages, seven without tests). The canonical upstream check and all-module
unit commands pass. Focused real-server deadlock, cancellation, timer replay
and shutdown tests pass with default cache and WORKFLOW_CACHE_SIZE=0; history
replay tests also pass. The canonical test builder uses private test-only
ports to avoid an existing local development server. Initial port-collision
setup failures are retained in agentRed's review evidence. agentRed's own regression, workflow replay,
worker registration/process core and merged-main CI are additional checks
in agentRed's DX33 pull request, not asserted by this fork's unit suite.

Remove the replacement when a supported upstream release contains this
correction and the same regression/replay checks pass. Review by 2026-12-01;
do not keep following the fork's upstream-tracking main branch implicitly.
GitHub Actions are disabled for this fork; no paid workflow is needed.
