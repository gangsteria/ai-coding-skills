---
name: e2e-test
description: Browser end-to-end (e2e) test conventions. Use when choosing browser coverage, changing browser specs or their setup, or diagnosing suite failures or slowdowns.
---

# E2E Test

Browser tests are the slowest and least stable layer. Every step keeps the suite **fast** (the browser drives the action under test, faster seams prepare the rest) and **stable** (a scenario passes or fails the same way in any order, at any concurrency).

1. **Read the project's conventions.** Read the shared fixtures, nearby specs, and any guide beside the specs. Resolve conflicts in this order: documented project rules, then these steps, then agent-wide defaults. Surface a conflict with a mandatory instruction to the user. Done when you can name how to prepare prerequisites and the narrowest supported test command. Use browser setup when the project lacks a setup seam. Use the full suite when the runner offers no narrower command.
2. **Choose the test layer.** Name the observable outcome and cover it at the lowest layer that exercises it. Use a browser when the outcome depends on browser interaction or state. Use HTTP-level tests for outcomes that can be exercised through HTTP alone, such as authorization rules or timing limits. Done when every outcome is assigned to one layer and each browser outcome names the interaction it depends on.
3. **Prepare and isolate.** Prepare prerequisites through the setup seam, including sign-in when signing in is not the action under test. Fixture navigation and readiness checks may use the browser; drive the action under test through it. Give each scenario its own mutable data and identities. Scope mutable infrastructure to the scenario or worker, or serialize access when isolation is unavailable, and restore shared state between scenarios. Release fixture-owned resources at the end of their scope, including when the scenario fails, and have fixture teardown discard in-flight work (request interceptors, polling, timers) before closing what that work runs against. Wait for observable transitions such as a visible element, a URL, or a response, instead of fixed delays. Done when each scenario uses the setup seam for its prerequisites where one exists, its data and results do not depend on other scenarios, and every resource it acquires is released whether it passes or fails, and fixture teardown discards in-flight work before closing what that work runs against.
4. **Diagnose observed failures and slowdowns.** Run this step when a scenario fails or runs slower than before. Locate the failing or slow phase among setup, action, assertion, and cleanup. Compare a focused run of the scenario with a full-suite run and vary one condition at a time, such as order, concurrency, or environment. Done when you have recorded the phase, the conditions that reproduce the problem, and the conditions that remain uncertain.
5. **Verify.** Run each changed scenario with the narrowest command, then the project's full check. When changing shared setup or parallelism or fixing an intermittent failure, also:
   - Record full-suite durations before and after under comparable conditions.
   - Plan repeated full-suite runs before verification and run them with retries disabled. Use the observed failure rate to choose the run count when available. Include known triggering conditions even when the failure rate is unknown.

   Done when every planned run passes and you have reported the run count, results, and durations. Report any remaining failure as unresolved.
