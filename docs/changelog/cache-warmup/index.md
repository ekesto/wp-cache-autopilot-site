---
layout: default
title: Cache Warmup Changelog
---

## Cache Warmup Changelog

> Auto-generated from the plugin readme. Source of truth lives in the plugin repository.


### 5.0.10 – 2026-05-08
* Fix: Widget-triggered warmup logs now show `Widget change` instead of a technical trigger key.

### 5.0.9 – 2026-05-07
* Fix: Fixed newly published pages occasionally being skipped during warmup before sitemap membership refreshed.

### 5.0.8 – 2026-04-30
* Fix: Warmup log rows now show `Skipped` with a readable sitemap-membership hint instead of internal skip codes.
* Fix: Async template changes now show `Template update` in warmup logs instead of a generic related-content label.

### 5.0.7 – 2026-04-29
* Fix: Uninstall now removes Cache Warmup settings, queue state, sitemap membership data, and tuning data for a cleaner database.
* Fix: Uninstall now removes the dedicated warmup run log table and transient cleanup state.
* Fix: The `Run Next Warmup Batch` button now returns a fast valid response on slow hosts while still starting warmup immediately when WP-Cron is available.

### 5.0.6 – 2026-04-28
* Fix: The manual "Run Next Warmup Batch" button is now available while a full warmup is preparing, so warmup can be triggered manually when WP-Cron is delayed or disabled.

### 5.0.5 – 2026-04-28
* Fix: Deleted posts are removed from pending warmup work before they can trigger repeated 404s.
* Fix: Trashed posts now use their original permalink for deleted URL filtering, avoiding homepage suppression.
* Fix: Warmup logs now label deleted content as deleted instead of post updates.
* Enhancement: Added focused diagnostics for deleted-post warmup flow.

### 5.0.4 – 2026-04-27
* Fix: Sitemap membership now refreshes automatically after content, taxonomy, option, and post type structure changes.
* Fix: Internal plugin option updates no longer trigger unnecessary sitemap rebuild scheduling.
* Fix: Recent runs now refresh automatically when returning to a hidden Log tab.
* Enhancement: Sitemap rebuild scheduling logs now show the trigger source only when a rebuild is actually queued.

### 5.0.3 – 2026-04-27
* Fix: Manual full refresh now starts warming sooner after queue preparation begins.
* Fix: Recent runs now refresh to the final result automatically after a warmup completes.
* Enhancement: Added a visible completion log for finished runs without enabling verbose logging.

### 5.0.2 – 2026-04-24
* Fix: Queue-aware run button.
* Fix: Active-only log refresh.

### 5.0.1 – 2026-04-23
* Enhancement: Added explicit run-total transition logging (`ecw_run_total_updated`) whenever active runs merge additional URLs.
* Fix: Queue diagnostics now show total URL growth clearly after single-run initialization.
* Fix: `Run Next Warmup Batch` now refreshes Recent runs immediately after completion, including when a previous log fetch is still in progress.

### 5.0.0 – 2026-04-21
* Breaking: Access control is now binary; users can either manage Cache Warmup fully or not access it.
* Fix: Admin bar actions and settings visibility now require manage access only.
* Enhancement: Cache Warmup now uses the shared manage-only access shim with Cache Invalidator-compatible behavior.

### 4.1.1 – 2026-04-20
* Fix: Settings JavaScript and styles now always include a version hash to prevent stale browser cache after updates.
* Enhancement: Settings assets now use per-file version metadata with deterministic fallback for cache busting reliability.

### 4.1.0 – 2026-04-20
* Enhancement: Cache Warmup now consumes only the Cache Invalidator emission pipeline as its trigger source.
* Fix: Removed legacy purge and lifecycle trigger paths to keep warmup execution deterministic and avoid duplicate run paths.
* Enhancement: Simplified cache integration boundaries by removing the internal adapter layer and relying on Cache Invalidator adapter authority.

### 4.0.8 – 2026-04-17
* Fix: Full-run intake now coalesces PREPARING overlaps instead of restarting, preserving atomic prepare ownership and stopping restart churn.
* Enhancement: Added one-shot coalesced replay flow after full-run finish or prepare failure so trigger bursts collapse deterministically without signal loss.
* Enhancement: Added lifecycle observability markers for coalesced overwrite/replay and PREPARING completion boundaries in support logs.
* Fix: WP Rocket adapter now listens to `rocket_after_clean_domain` for full flush and removes `after_rocket_clean_post` intake, keeping URL intake on `after_rocket_clean_files`.
* Enhancement: Cache Enabler adapter now hooks clear-all events (`cache_enabler_site_cache_cleared`, `cache_enabler_complete_cache_cleared`, `ce_action_cache_cleared`) into full warmup triggers.
* Fix: System-origin trigger identity is now preserved (`cache_plugin_cleared`) so later merges do not relabel system clear runs as generic content updates.
* Enhancement: FlyingPress adapter runtime is transport-only in ECW; deterministic warmup intake now relies on ECI emissions instead of direct FlyingPress purge hooks.
* Enhancement: Replaced the fixed `100` batch ceiling with centralized dynamic batch sizing (UI/system default `1000`, filterable runtime cap via `ekesto_cw_batch_max_urls`, soft clamp `5000`).
* Enhancement: Advanced settings now receive server-side batch preview (`configured`, `effective`, `dynamic_system_cap`, `basis`) and show configured/effective batch counts without client-side pacing recomputation.
* Fix: `pages_per_batch` UI control now uses the new schema cap (`1000`) and labels guidance as `Recommended ceiling` when telemetry recommendations hit configured/runtime caps.
* Fix: Manual full warmup trigger keys now normalize to `manual:settings_general` and `manual:admin_bar`, and both resolve consistently to `Manual warmup` labels.
* Enhancement: Trigger label lookup now resolves primary system events through `TriggerKeyLabelMap`, including explicit `Cache plugin cleared` mapping for system-triggered runs.
* Enhancement: Runtime trigger ownership is now deterministic: ECW intake registers only `ekesto_ci_invalidation_urls` (plus internal housekeeping) and no longer owns lifecycle or adapter-signal ingress.
* Fix: Added fail-closed operational adapter authority bridge (`InvalidatorAdapterGate`) with canonical context and optional override via `ekesto_ci_has_operational_adapter`.
* Fix: Manual full refresh no longer queues directly; it now dispatches `ekesto_ci_manual_full_refresh` and flows through ECI global emission intake.
* Fix: Intake contract is now strict: missing `context.urls` payloads are rejected, while `GLOBAL_INVALIDATION` with `context.urls=[]` is accepted as canonical full warmup intent.
* Enhancement: Correlation IDs now propagate from ECI emissions through ECW intake and queue metadata for end-to-end trigger tracing.

### 4.0.7 – 2026-04-16
* Fix: Cache Enabler full-cache purge now calls the global `\Cache_Enabler::clear_total_cache()` method explicitly for namespace-safe execution.

### 4.0.6 – 2026-04-16
* New: Added Cache Enabler and Breeze adapter support for runtime cache-plugin detection and warmup integration.
* Enhancement: Adapter selection now prioritizes deterministic adapters only (LiteSpeed, WP Rocket, FlyingPress, Cache Enabler, Breeze).
* Fix: Removed non-deterministic adapters from active registry while keeping legacy adapter files for compatibility-safe code retention.

### 4.0.5 – 2026-04-13
* Fix: Added `TriggerKeyLabelMap` as a dedicated system-event label map so maintenance and upgrade trigger keys resolve to speaking labels without mixing post-type identity domains.
* Fix: Trigger key normalization now preserves `:` and trims whitespace so broad `upgrade:*` grouping consistently resolves to `System update`.
* Enhancement: Hardened trigger label lookup with class constants (`EXACT`, `PREFIX`) to avoid per-call map allocation.

### 4.0.4 – 2026-04-13
* Fix: Sitemap notices now use short, user-facing reason lines and include `Last build` context for membership warnings.
* Enhancement: Added a consistent troubleshooting CTA (`What this means ↗`) across sitemap notice variants.
* Fix: Missing sitemap notice now uses a dedicated `Configure` action to open Warmup settings.

### 4.0.3 – 2026-04-13
* Enhancement: Recent runs now support explicit loading beyond the default 10 entries with cap-aware button labels.
* Fix: `/logs` now returns list metadata (`logs_total`, `logs_shown`, `logs_overflow`, `logs_truncated`) and `all=1` loads up to 210 newest runs (default 10 + additional 200).
* Enhancement: Sitemap admin notices now use a consistent 3-line format with bold plugin prefix, short reason line, and docs CTA (`What this means ↗`).
* Fix: Missing sitemap notice now uses a dedicated `Configure` button and removes SEO-plugin mention text.
* Fix: Membership sitemap notices now retain `Last build` timestamp context in the short reason line.

### 4.0.2 – 2026-04-13
* Fix: Membership attention admin notice now renders a bold plugin prefix (`Cache Warmup:`) for consistent wp-admin warning formatting.

### 4.0.1 – 2026-04-12
* Removed deprecated `Queue::enqueueFull()` shim (added in 3.6.0 for 3.x upgrade safety).
* Fix: Replaced transient-based sitemap membership caching with a persistent option-backed membership index for intake and runner fallbacks.
* Enhancement: Added async sitemap membership rebuild orchestration (`ekesto_ecw_rebuild_membership`) with hash skip, lock protection, and structured support-log observability.
* Fix: Restored membership access for wp-admin AJAX requests so intake can load cached membership state during invalidation handling.
* Enhancement: Added membership health admin notices for degraded/error/corrupt states with per-user dismissal reset on state changes.
* Fix: Simplified intake summary logging by removing source-verification suffix; `membership_source_used` is now the authoritative membership state log.
* Enhancement: Added a persistent oldschool wp-admin warning on all admin screens when no sitemap URL is configured, with direct Warmup settings CTA.
* Fix: Settings save now updates missing-sitemap warning visibility immediately (no page reload) via a sitemap-presence event bridge.

### 4.0.0 – 2026-04-12
* New: Immutable primary identity system — run labels are locked at birth (or on first ECI claim) and stay stable through merges and promotions.
* New: Extensible `TriggerLabelMap` with `ekesto_cw_trigger_label_map` filter for custom post-type label resolution.
* New: Enforced strict invalidation intake contract; `context.urls` is now mandatory and authoritative, with canonical URL-set processing and stable priority-first enqueue ordering.
* Enhancement: Per-URL trust handling with strict `trusted_source` gating; trusted URLs bypass sitemap checks, non-trusted require cached membership.
* Enhancement: Adapter single-URL intake now captures trigger metadata from post context for richer log labels.
* Enhancement: `GLOBAL_INVALIDATION` now passes trigger metadata through to the full warmup path.
* Enhancement: Full run labels now append "(full)" instead of replacing with a generic label.
* Enhancement: Early ECI identity claim on active adapter-originated runs before URL enqueue, so labels reflect the authoritative source immediately.
* Fix: Replaced high-volume per-URL intake skip logs with aggregated counters; per-URL diagnostics are verbose-only.
* Breaking: Removed legacy trust flag compatibility bridge (`resolver_confirmed_target`, `synchronous_targeted_resolution`); requires ECI >= 9.0.0.

### 3.7.6 – 2026-04-11
* Enhancement: Warmup Transport diagnostics now show a human-readable fallback explanation while keeping the raw fallback code visible for support.
* Enhancement: Recent Log failed URL rows now include fallback trigger, effective transport, and raw transport attempt context.
* Enhancement: Log event details now persist additive fallback diagnostics fields (`fallback_used`, `fallback_reason`, `brotli_attempted`, `brotli_error`, `brotli_skip_reason`) and expose them through log detail responses.
* Fix: Diagnostics label and helper copy now clarify that the fallback trigger is telemetry-wide and not necessarily the latest failed URL cause.

### 3.7.5 – 2026-04-08
* Fix: Shared support debug log timestamps now include explicit UTC and WordPress local time (`YYYY-MM-DD HH:MM:SS UTC (YYYY-MM-DD HH:MM:SS local)`) to prevent timezone ambiguity during invalidation-to-warmup tracing.
* Fix: Adapter single-URL intake now rejects non-public post contexts via a shared post warmability helper.
* Fix: Targeted canonical URL admission now rejects query-ID targets (`page_id`, `p`, `attachment_id`) when the resolved post is not publicly warmable.

### 3.7.4 – 2026-04-07
* Enhancement: `TriggerReasonResolver` now resolves adapter-originated purge keys (`purge_single_url`, `purge_front_page`, `purge_pages`) and builder-specific events (`elementor_template_change`) into user-meaningful labels.
* Enhancement: `save_post` targeted warmups now persist `trigger_class`, `trigger_event`, and `trigger_post_type` metadata so log labels resolve content change reasons without ECI.
* Enhancement: When ECI merges into an adapter-initiated single run, `Logger::promoteRunTrigger()` promotes the trigger key and metadata so the log row reflects the higher-fidelity source; `primary_trigger_event` locks the first ECI event as permanent label owner.
* Fix: Merged indicator no longer flags multi-URL ECI invalidation batches that started as a single intake; compares `total_urls` against `incoming_url_count` to distinguish genuine merges from batch starts.

### 3.7.3 – 2026-04-06
* Enhancement: Warmup log trigger labels now show user-meaningful content change reasons (e.g. "Product update", "Post update") instead of technical WordPress hook names.
* Enhancement: Invalidation trust now accepts `synchronous_targeted_resolution` as a secondary trust signal so resolver-driven URLs (archives, integration pages) are warmed even when sitemap membership is temporarily unavailable.

### 3.7.2 – 2026-04-05
* Fix: Recent Logs preview counters now derive shown/total/overflow from one canonical preview candidate set so values stay internally consistent.
* Enhancement: Settings now warns before browser unload when unsaved changes are present.
* Enhancement: Invalidation trust now accepts `confirmed_resolution_target` with legacy `resolver_confirmed_target` fallback for mixed-version compatibility.

### 3.7.1 – 2026-04-02
* Fix: Added strict queue run-ownership guards so stale workers cannot overwrite or clear newer restarted runs.
* Fix: Allowed resolver-confirmed invalidation targets to enqueue when cached sitemap membership is temporarily unavailable.
* Fix: Extended `onSavePost()` structural fallback exclusions to skip direct targeted enqueueing for `wp_pattern` and `wp_global_styles`.
* Fix: Adapter post-purge single-URL intake now applies post-aware structural admission so structural editor entities are rejected before targeted queue insertion.

### 3.7.0 – 2026-04-02
* Fix: Added the missing `Debug` class import in manual run-now handling so diagnostics logging resolves the correct namespace during immediate batch execution.

### 3.6.0 – 2026-04-02
* Enhancement: Added upgrade-sensitive trigger suppression so shutdown-time adapter events defer warmup enqueueing to maintenance cron during in-request plugin updates.
* Fix: Restored backward compatibility for mixed-version update windows with a deprecated `Queue::enqueueFull()` forwarding shim to intent-based enqueueing.
* Fix: Updated all settings tab docs links to the public docs host (`wpcacheautopilot.com`).

### 3.5.14 – 2026-04-01
* Fix: Restored changelog mirroring to the standalone `sync-changelog.yml` workflow after release builds complete.
* Fix: Aligned readme changelog headings with the generated public changelog markdown structure.

### 3.5.13 – 2026-04-01
* Fix: Release pipeline now runs changelog mirroring from the tag-based release workflow after build completion.
* Fix: Removed the separate sync-changelog workflow to avoid split release-path behavior.

### 3.5.12 – 2026-04-01
* Enhancement: Changelog sync workflow now publishes to docs/changelog/<plugin>/index.md with rebase-retry push handling.
* Fix: Readme changelog headings now stay markdown-ready for direct public-doc mirroring.

### 3.5.11 – 2026-04-01
* Fix: Full warmup requests now persist intent-only `preparing` runs and move sitemap planning into the runner.
* Fix: Targeted invalidations now buffer losslessly during full-run preparation and merge before sitemap tail capping.
* Fix: Logs, run-now, and reset handling now treat `preparing` as active work and transition the same run row to `running`.

### 3.5.10 – 2026-04-01
* Fix: Async full warmup preparation now keeps targeted URLs queued during `preparing` in a run-scoped buffer until the running queue is committed.
* Fix: Invalidation intake now uses cached-only sitemap membership and skips quickly when cached membership is unavailable.
* Fix: Targeted enqueue admission now rejects preview, admin, REST-style, and Elementor library query URLs before queueing.
* Fix: Recent Logs now shows em dashes for preparing totals instead of misleading zero values.

### 3.5.9 – 2026-03-31
* Enhancement: Logging tab manual batch action now runs the next queued warmup batch directly in the current request.
* Fix: Run-now REST responses now return explicit manual batch outcome states and queue metadata, including cache-disabled skips.
* Fix: `save_post` now asks ECI frontend eligibility before queueing targeted warm URLs and fails open when ECI is unavailable.
* Fix: `Triggers` now trusts ECI structural detection for `elementor_library` and fails open on ambiguity instead of suppressing by post type alone.
* Fix: Warmup now skips or stops runs when the selected cache adapter reports baseline page caching as disabled.
* Enhancement: `GLOBAL_INVALIDATION` intake now reuses the existing full warmup path instead of enqueueing per-URL targeted work.

### 3.5.8 – 2026-03-30
* Fix: Clarified Priorities tab copy to state post type ordering applies to full warmup runs.

### 3.5.7 – 2026-03-30
* Enhancement: Added on-demand full URL detail loading in Recent Logs via a dedicated per-run details endpoint.
* Enhancement: Added concise running-state detail hint when warmed URLs exist but only queued rows are visible.
* Fix: Clarified URL detail preview copy with explicit shown/total entry counts.
* Fix: Switched load-all error rendering to the native WordPress `Notice` component.
* Fix: Updated Freemius bootstrap flags for premium-only/anonymous mode and disabled plugin submenu rendering.
* Enhancement: Updated dist packaging excludes to omit Composer manifests and testing directories from release archives.

### 3.5.6 – 2026-03-29
* Fix: Updated Freemius SDK initialization to use direct associative-array parameters for deployment validation compatibility.

### 3.5.5 – 2026-03-29
* Enhancement: Moved Freemius SDK updates to a Composer-managed vendor/freemius/wordpress-sdk workflow.
* Enhancement: Switched runtime class loading to Composer PSR-4 only (vendor/autoload.php) and removed legacy src/autoload.php.
* Enhancement: Updated the admin bar quick link from `Open settings` to `Show log`, targeting the Logging tab anchor directly.
* Fix: Added gated Freemius bootstrap wrappers with Git Updater-aware auto-disable and explicit update-system separation.
* Fix: Release packaging now installs Composer dependencies before building the dist artifact.
* Fix: Release verification now enforces lifecycle uninstall wiring and fails if legacy root uninstall.php is present.

### 3.5.4 – 2026-03-28
* Fix: Moved uninstall cleanup to a lifecycle uninstall hook for Freemius-compatible packaging.
* New: Added a manual HTTP self-check endpoint for diagnostics connectivity checks.
* Enhancement: Moved connectivity diagnostics into Warmup Transport and added server-time context to log refresh actions.
* Fix: Reduced WP-Cron diagnostics to configuration and queue activity details.
* Enhancement: Consolidated admin/settings SCSS colors into semantic design tokens.

### 3.5.3 – 2026-03-25
* Enhancement: Added shared `wp_cache_autopilot_capability` capability filter support (default `manage_options`) across settings, admin bar controls, and protected warmup actions.
* Enhancement: Added expandable per-run URL details in Recent Logs with inline copy/open URL actions.
* Enhancement: Added additive `/logs` event payload fields (`event_rows`, `event_counts`, preview totals) with active-queue aware detail rows.
* Enhancement: Added PriorityFilter diagnostics for configured post-type order plus unresolved/excluded URL samples in debug output.
* Fix: Persisted in-progress `warmed_urls` and `failed_urls` counters during running batches so manual reload shows live run progress.
* Fix: Normalized per-URL log event fields from runner payloads (`result`, `status_code`, `duration_ms`) to keep detail rendering stable.

### 3.5.2 – 2026-03-24
* Enhancement: Reduced baseline debug noise by moving hook-registration and high-frequency transport traces to verbose mode.
* Enhancement: Added concise adapter-trigger and runner batch outcome summaries for support diagnostics.
* Fix: Reclassified `WPFC hooking wpfc_delete_cache` as verbose-only.

### 3.5.1 – 2026-03-24
* Fix: Shared support log filename validation now accepts `.txt` files (and keeps legacy `.log` support).
* Fix: `Debug::log()` (`DEBUG`) entries now write to the shared support log whenever support mode is enabled.

### 3.5.0 – 2026-03-24
* Enhancement: Replaced the General tab with a Warmup tab and added docs help links across settings panels.
* Enhancement: Added snackbar success/error feedback for warmup probe and manual refresh actions in Priorities and Logging.
* Enhancement: Added dual-destination debug logging (`error_log` + support log file) with level routing and verbose guards.
* Fix: Registered admin bar controls via `wp_before_admin_bar_render` to stabilize late toolbar rendering.
* Fix: Switched cache invalidation intake to `ekesto_ci_invalidation_urls`.

### 3.4.0 – 2026-03-18
* New: Added an operational admin bar control with icon-only root label, `Warmup: Active/Paused` state row, `Pause warmup`/`Enable warmup`, and direct settings access.
* New: Added `Run full warmup` actions in both admin bar and General settings, routed through the existing full warmup enqueue flow.
* Enhancement: Added `POST /wp-json/ekesto-cache-warmup/v1/run-full` with `manage_options` protection and paused-state guardrails.
* Enhancement: Added server-side cache invalidation and short-lived caching for priority pages REST payloads to reduce repeated query load.
* Fix: Moved admin bar styling from inline head output into compiled admin stylesheet and aligned toolbar icon rendering with pseudo-element-based markup.
* Fix: Suppressed `save_post` debug entry logs for autosave/revision posts to reduce misleading targeted warmup traces.
* Enhancement: Switched invalidation intake hook to `ekesto_ci_invalidation_urls` for cache-invalidator integration.

### 3.3.1 – 2026-03-17
* Enhancement: Added fuzzy search for Priority Pages selection (title and URL path matching).
* Enhancement: Added inline match highlighting for title and URL columns in priority page rows.
* Fix: Preserved select-all and row toggle behavior for visible filtered rows while search metadata flows through selector components.

### 3.3.0 – 2026-03-17
* Enhancement: Added Brotli-first warm transport with safe `wp_remote_get()` fallback when raw transport is unavailable or fails.
* Enhancement: Added dual transport telemetry (`effective`, `brotli`, `fallback`) while keeping auto-pacing based on effective request timing.
* Fix: Replaced outdated compression probe diagnostics with transport capability and runtime telemetry diagnostics.
* Enhancement: Improved Advanced tab auto-pacing UX copy and aligned admin recommendation math with runtime Runner pacing logic.

### 3.2.2 – 2026-03-15
* Fix: Aligned invalidation and run-now wording with runtime guarantees (best-effort scheduling and sitemap-eligible URL intake).
* Enhancement: Standardized Cache Warmup terminology across settings UI, README, and internal docs.
* Enhancement: Synced CLAUDE.md technical identifiers with runtime option keys, table names, and settings groups.

### 3.2.1 – 2026-03-12
* Fix: Simplified no-sitemap messaging in Priorities to a single fallback explanation.
* Enhancement: Added first-time sitemap onboarding guidance with a clear refresh action.
* Enhancement: Reduced duplicate status text in Priorities and only shows post type ordering once types are detected.

### 3.2.0 – 2026-03-12
* New: Added first-party multilingual adapters for Polylang and TranslatePress.
* Enhancement: Moved plugin-specific multilingual classes under `src/Multilingual/Adapters` and kept a backward-compatible WPML shim at `src/Multilingual/WpmlAdapter.php`.
* Enhancement: Generalized multilingual URL-language resolution for default-language-first priority ordering and sitemap snapshot post-type counting.
* Enhancement: Replaced WPML-only save-post permalink localization with adapter-driven permalink resolution.

### 3.1.1 – 2026-03-12
* Fix: Registered available cache adapter purge hooks deterministically when `plugins_loaded` is running or already completed
* Enhancement: Added adapter manager idempotency guard to prevent duplicate hook registration in the same request

### 3.1.0 – 2026-03-12
* New: Added cache adapters for WP Rocket, W3 Total Cache, and FlyingPress
* Enhancement: Added targeted URL/post purge hook wiring for WP Rocket, W3TC, and FlyingPress
* Enhancement: Extended maintenance `purgeAll()` support to newly supported adapters without changing existing adapter precedence

### 3.0.2 – 2026-03-09
* Fix: Deferred settings option registration to `admin_init` so translation loading is not triggered before `init`
* i18n: Guarded schema and adapter fallback labels to avoid early textdomain loading notices in WordPress 6.7+

### 3.0.1 – 2026-03-06
* Fix: Restored settings page registration while keeping lazy admin bootstrap hardening
* Fix: Hardened bootstrap and deferred maintenance/update flows against partial plugin updates
* Enhancement: Added local release verification for runtime files, built assets, version sync, packaging exclusions, and release zips

### 3.0.0 – 2026-03-05
* New: Added dedicated `Config` and `Debug` core classes for centralized plugin constants and structured diagnostic logging
* Enhancement: Refactored runtime consumers (`Runner`, `Queue`, `Triggers`, `Sitemap`, `Logger`, `PriorityFilter`) to use `Settings\Repository` directly and `Debug` logging with structured context
* Enhancement: Simplified `Settings` to constants-only and moved settings/admin boot wiring into focused settings classes and self-bound REST callbacks
* Enhancement: Moved JS/SCSS source tree from `src/` to `assets/src/` and updated webpack entrypoints/documentation accordingly
* Enhancement: Improved settings UI behavior and styles (tab-aware responsive table shadow re-init, modal layout system, table/status code styling)
* Fix: Added full release packaging support with `.distignore` and regenerated build assets from the new source paths

### 2.0.14 – 2026-03-01
* Enhancement: Recent Logs now display ECI trigger events as `ECI:<trigger_event>` for cache invalidation runs
* Enhancement: Logging mode label `single` now renders as `targeted` in the admin logs table
* Enhancement: Recent Logs now show a `merged` indicator for targeted runs that expanded beyond one URL

### 2.0.13 – 2026-03-01
* Fix: Centralized responsive table shadow initialization in the settings app shell so table shadows remain active across tab switches and async table rendering
* Fix: Standardized responsive table class contract to `ekesto-responsive-table-container` and `ekesto-responsive-table-inner` for Recent Logs and Diagnostics tables

### 2.0.12 – 2026-02-28
* Enhancement: Prioritized single invalidation URLs to the front of active full runs for faster post-edit warming
* Enhancement: Added WPML-aware default-language-first ordering inside existing warmup priority buckets for full runs
* Fix: Suppressed redundant same-request `purge_all` full warmups during targeted invalidation flows
* Enhancement: Improved sitemap membership cache matching and added focused WP_DEBUG diagnostics for membership misses
* Fix: Reduced WP Fastest Cache hook registration noise via duplicate-registration guards

### 2.0.11 – 2026-02-28
* Enhancement: Refactored Settings into a stable facade with internal service classes for repository, REST, snapshot, admin page, and run-now actions
* Enhancement: Added multilingual adapter-based sitemap post type counting (WPML), with safe fallback to count all posts when language resolution is unavailable

### 2.0.10 – 2026-02-20
* improvements: UI

### 2.0.9 – 2026-02-20
* Enhancement: Term edits now suppress broad cache purge events, allowing targeted invalidation via ekesto-cache-invalidator
* Enhancement: Added sitemap membership check to filter warming URLs, reducing unnecessary requests
* Fix: Post type filtering now uses `publicly_queryable` instead of `public` for more accurate content detection

### 2.0.8 – 2026-02-18
* Enhancement: Reset Tuning Data button now uses snackbar notifications

### 2.0.7 – 2026-02-18
* Fix: WP Super Cache purge no longer passes blog ID on single-site installs, preventing fatal get_blog_option() error during plugin updates

### 2.0.6 – 2026-02-18
* Fix: Skip cache adapter purge during maintenance/upgrade to prevent early bootstrap errors

### 2.0.5 – 2026-02-18
* Improved: Reduced update block window to 30s
* Improved: Automatic telemetry-based tuning with p90 response time analysis and safe PHP execution limit integration.
* Improved: Wording

### 2.0.4 – 2026-02-18
* Added lease-based cron lock to prevent overlapping warmup runs
* Introduced Automatic and Manual tuning modes
* Added telemetry-driven adaptive pacing (rolling p90)
* Implemented safe bootstrap mode for insufficient data
* Added backend host-aware safety caps for manual limits
* Improved stability and restart safety
### 2.0.3 – 2026-02-17
* Fix: Log total_urls field now updates when URLs are merged into active runs, fixing log display confusion
* Enhancement: Moved "Run Warmup Now" button to Logging tab (right above recent runs) for better UX

### 2.0.2 – 2026-02-17
* Fix: Logging retention_days field now properly shows empty with placeholder on restore instead of pre-filled with default

### 2.0.1 – 2026-02-17
* Fix: Add maintenance mode guard to prevent race condition during upgrades where Runner::run() could access undefined constant from cached bytecode

### 2.0.0 – 2026-02-17
* New: Advanced settings tab with configurable throttling and batch timeout control
* Enhancement: Eager queue state persistence after each URL prevents concurrent cron conflicts
* Enhancement: Per-request warmup timeouts optimized for local vs. production environments
* Enhancement: Settings refactored to "advanced" namespace for clarity
* Breaking: Pacing and Queue tabs consolidated into Advanced and Diagnostics tabs

### 1.3.1 – 2026-02-15
* Fix: Manual URL Exclusions issue resolved.

### 1.3.0 – 2026-02-15
* New feature: Manual URL exclusions — exclude specific URLs from warmup directly in Settings
* New feature: Priority post types — assign priority order to post types so priority pages warm first
* New feature: Priority pages — pin individual pages to warm before other content
* New: REST endpoint to detect post types/pages from configured sitemap
* Improved: Fallback warmup now queries published pages instead of just homepage when sitemap unavailable
* UI improvements: Post type pills and priority management enhancements
* Improved: Attachment post type excluded from UI (not relevant for cache warmup)

### 1.2.8 – 2026-02-15
* Improved UI

### 1.2.7 – 2026-02-14
* Added: Settings link on plugin page

### 1.2.6 – 2026-02-14
* Fix: Make preventLscCaching method public for filter callback

### 1.2.5 – 2026-02-14
* Fix: Prevent LiteSpeed Cache from caching plugin REST API responses — uses rest_pre_dispatch filter for reliable cache suppression

### 1.2.4 – 2026-02-14
* Fix: Prevent LiteSpeed Cache from caching REST API responses

### 1.2.3 – 2026-02-14
* UI improved

### 1.2.2 – 2026-02-13
* Fix: Default settings related to post type order
* Improved: UX of required `page` post type clearer in post type order

### 1.2.1 – 2026-02-13
* Added: CLAUDE.md file

### 1.2.0 – 2026-02-13
* New feature: Settings page goes REACT!
* New feature: Priority post types and pages

### 1.1.9 – 2026-02-10
* Re-added full warmup when deleting a plugin, as LiteSpeed Cache purges the entire cache.

### 1.1.8 – 2026-02-10
* Fixed admin log time display by converting stored UTC timestamps to the WordPress timezone consistently.
* Code cleanup

### 1.1.7 – 2026-02-10
* Hardened full-warmup enqueue logic against overlapping maintenance, upgrade, and cache-plugin purge triggers.
* Added stricter suppression of purge_all events during upgrade and plugin activation/deactivation flows.
* Reduced restart noise in the run log by preventing immediate back-to-back full-run restarts.
* Internal cleanup and additional guard rails around full-run lifecycle handling.

### 1.1.6 – 2026-02-10
* Simplified warmup execution model: exactly one warmup batch is processed per wp-cron execution.
* Renamed pacing setting to **pages per batch** and removed unreliable internal tick-based scheduling.
* Fixed duplicate full warmup restarts caused by overlapping upgrade and cache-plugin purge triggers.
* Added upgrade/maintenance de-duplication window to suppress purge storms during updates.
* Persisted queue activity timestamps consistently and simplified Cron diagnostics to reflect real wp-cron behavior.

### 1.1.5 – 2026-02-10
* Simplified warmup execution model: exactly one warmup batch is processed per wp-cron execution.
* Renamed pacing setting from “pages per minute” to **pages per batch** to reflect real behavior on server-cron-driven hosts.
* Removed internal tick-based scheduling logic that could not be reliably executed on shared hosting.
* Fixed duplicate “purge_all” → “upgrade” double-run race by hardening full-run enqueue logic.
* Persisted queue `last_activity` timestamp consistently, including after successful run completion.
* Improved Cron diagnostics to reflect real execution guarantees and removed misleading “next due time” signals.
* Clarified admin UI descriptions to explicitly document hosting-dependent wp-cron behavior.

### 1.1.4 – 2026-02-10
* Bug fix: Now additional cron events are correctly scheduled.

### 1.1.3 – 2026-02-10
* Improved UI

### 1.1.2 – 2026-02-10
* Simplified the warmup execution model to align strictly with real WP-Cron behavior on shared hosting.
* Replaced time-based “tick” logic with a deterministic **per-cron batch execution** model.
* Renamed pacing setting to **Pages per batch**, reflecting actual execution semantics.
* Removed misleading timing settings (tick interval, per-request timeout) that could not be reliably enforced.
* Clarified admin UI copy to explain how warmups behave under page-load-driven vs server-cron-driven WP-Cron.
* Fixed **Run Warmup Now** action to reliably schedule a batch and trigger a non-blocking `wp-cron.php` request.
* Improved cron diagnostics clarity, making hosting limitations immediately visible.
* Reduced internal complexity and state transitions to minimize stalled or partial runs across environments.

### 1.1.1 – 2026-02-09
* Replaced the broken “Reset queue” control with a safer **Run Warmup Now** button.
* Added a live **server-time clock** to the Cron diagnostics panel (single sync + per-second ticking).
* Fixed admin server-time endpoint issues in namespaced PHP.
* Hardened nonce and capability checks for all admin-side AJAX endpoints.
* Simplified admin JavaScript by removing redundant polling logic.

### 1.1.0 – 2026-02-08
* Stabilized warmup behavior during WordPress core, theme, and plugin maintenance events.
* Prevented duplicate full warmups during upgrades by suppressing adapter purge triggers inside the upgrade flow.
* Treated forced plugin/theme installs (e.g. WP-CLI `--force`) as upgrade events.
* Improved WP-Cron kick reliability on shared hosting with hardened loopback requests.
* Added Cron diagnostics panel to aid debugging delayed or blocked cron execution.
* Reduced false “stale” run states on hosts with delayed cron execution.

### 1.0.1 – 2026-02-05
* Improved restart-safe warmup scheduling and debouncing.
* Added configurable warmup pacing.
* Enhanced logging with runtime duration and clearer status reporting.
* Improved compatibility with external invalidation sources.
* Documentation updates and cross-reference to Cache Invalidator by ekesto.

### 1.0.0 – 2026-02-05
* Initial release with sitemap-based, restart-safe cache warmup engine.
