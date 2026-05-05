# Build + Patch Protocol (v1.3)

Use this protocol for all software changes unless explicitly overridden.

---

## 1. Version Every Change
* **Label:** Every patch, build, or release must have a clear version label (e.g., `fix-login-timeout`, `v012-dashboard-layout`).
* **Expose:** The running app must surface the active version (e.g., in debug logs, admin screens, footers, or `/health` endpoints).

## 2. Observability Over Guessing
* **Log Truth:** Do not guess root causes. Use or inject explicit logging, debug output, and runtime reports.
* **Expose State:** The app must report its current version, route, features, config, API status, and errors before executing risky code.

## 3. Patch Narrowly by Default
* **Scope Control:** Fix only the target bug or add the specific feature.
* **No Unrelated Changes:** Avoid unrelated UI tweaks, dependency changes, or unsolicited code cleanup.

## 4. Prevent Patch Stacking
* **Stop and Propose:** If a target area contains duplicated logic, stale fallbacks, or fragile fixes, do not add another layer. 
* **Cleanup First:** Propose a targeted cleanup plan to the user before applying more logic. Do not execute broad rewrites.

## 5. Just-In-Time Object Standardization
* **Start Simple:** Do not create premature abstractions, complex classes, or generic interfaces for early-stage or single-use features. Use raw dictionaries/JSON shapes first.
* **Standardize on Triangulation:** Stop and formalize explicit objects, types, or data transfer objects (DTOs) immediately when a data shape is:
  * Passed across **three** distinct boundaries (e.g., Database → Backend API → Frontend UI).
  * Read, mutated, or referenced in **three or more** separate files or modules.
  * Overloaded with **three or more** optional, nullable, or conditional properties.
* **No Speculative Architecture:** Do not build adapters, factories, or wrappers for future use cases that do not yet exist.

## 6. Self-Test & Verification Handover
* **Self-Test When Able:** Before claiming a patch is ready, verify the fix locally if the environment supports execution.
* **Clear Validation Steps:** If the environment cannot execute tests directly, provide the user with explicit, step-by-step instructions on what to test, the expected outcomes, and specific log flags or UI elements to inspect.

## 7. Protect Working Functionality
* **Isolate Impact:** Do not touch or modify existing, working systems (e.g., auth, data sources, deployment flows) while fixing unrelated components.

## 8. Align Related Systems
* **Cross-Boundary Checks:** When changing a feature, update its entire dependency chain: config schemas, database migrations, API routes, frontend states, and validation logic.

## 9. No Mocks Hiding Real Failures
* **Isolate Mocks:** Mocks must be explicitly labeled.
* **Surface Errors:** Never hide failed API calls or bad data behind mock fallbacks. Show visible error, loading, and unavailable states.

## 10. Respect Constraints & Code Style
* **Environment Limits:** Adhere strictly to existing screen sizes, server/client boundaries, and browser limits.
* **Match Style:** Maintain the existing project layout, linting rules, naming conventions, and design system of the language being used.

## 11. Build for the Intended Runtime
* **Target Environment:** If moving to web, avoid desktop assumptions. If server-rendered, avoid client-only hooks. Keep API endpoints clean.

## 12. Keep Artifacts Clean
* **Prune Bloat:** When creating builds, exclude stale generated files and unnecessary assets. Validate that the artifact can be installed or run.

## 13. Synchronize Documentation
* **Real-Time Updates:** Update `README`, `.env.example`, API documentation, and setup steps immediately when a patch alters behavior.

---

## 14. Required Response Format
For every task, return the response using this exact structure:
1. **Version Label:** `[Label]`
2. **What Changed:** Short summary of changes.
3. **Touched Files:** List of modified files/modules.
4. **Preserved Code:** What was intentionally left untouched.
5. **Verification Steps:** Explicit user validation actions or test scripts to run.
6. **Logs to Check:** Relevant debug/log outputs to watch.
7. **Risks:** Remaining issues or potential side effects.

---

## 15. Default Rule
When in doubt: **Instrument first. Patch small. Standardize objects only when used in three places. Self-test or provide exact user test instructions. Do not break working code.**
