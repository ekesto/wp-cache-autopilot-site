---
layout: default
title: Cache Invalidator Changelog
---

## Cache Invalidator Changelog

> Auto-generated from the plugin readme. Source of truth lives in the plugin repository.


### 12.0.0 – 2026-05-08
* New: Fixed widget content updates not always triggering cache invalidation in both Classic Widgets and Gutenberg block widgets. Widget invalidation now reliably detects content changes while preserving existing sidebar-based targeting and fallback behavior.
* Enhancement: Updated `ekesto_ci_widget_selectors` to receive only selectors and changed sidebar IDs.

### 11.1.2 – 2026-05-07
* Fix: Removed first-publish fan-out behavior to keep invalidation scoped to actual changed content.
* Enhancement: Improved content lifecycle handling to ensure publish, restore, and private status changes trigger correct targeted cache refreshes without duplicates.
* Enhancement: Added passive lifecycle probe diagnostics to improve troubleshooting without changing runtime behavior.

### 11.1.1 – 2026-04-30
* Fix: Bare `single` site-editor template no longer targets pages; pages resolve through `page`/`singular` per WordPress hierarchy. Use the `ekesto_ci_single_template_post_types` filter to opt back in.

### 11.1.0 – 2026-04-29
* New: Timed invalidation now includes a “Use post type settings” mode for replaying configured host targets.
* Enhancement: Timed rules can now include post type archives and public taxonomy archives from selected host settings.
* Enhancement: Timed invalidation now logs clearer target counts for configured host replay runs.
* Fix: Uninstall now removes the plugin's remaining settings and temporary cache data for a cleaner database.
* Fix: Uninstall now also removes support debug log files created by the plugin.

### 11.0.7 – 2026-04-27
* Fix: WooCommerce variation changes now refresh the parent product page.
* Fix: Deterministic relationship targets now keep trusted URL status in sync emissions, so valid grouped-parent targets are no longer dropped downstream.
* Fix: Relationship trust scope remains strict to internally resolved post IDs mapped to permalinks; archive and extension-added URLs stay untrusted.

### 11.0.6 – 2026-04-24
* Fix: Pick up non-Elementor editor saves.

### 11.0.5 – 2026-04-24
* Fix: Restore venue host resolution.

### 11.0.4 – 2026-04-23
* Enhancement: Async invalidation now uses one global switch with reliable sync fallback, so cache updates are always reflected.
* Fix: Elementor template saves now keep their previous condition state persistently, so invalidation remains reliable even after long idle periods.

### 11.0.3 – 2026-04-22
* Fix: Elementor presentation template condition changes now invalidate both previous and current page targets in one deterministic pass.
* Enhancement: Elementor condition-change diagnostics now focus on final-state transition and effective target signals with reduced debug noise.

### 11.0.2 – 2026-04-21
* Fix: Support debug log view works again in a new browser tab without breaking token-based security checks.
* Fix: Administrators now always see Access Control settings again.

### 11.0.0 – 2026-04-21
* Breaking: Access control is now binary; users can either have full manage access or no access.
* Enhancement: Roles now use a simplified None/Manage model and user overrides are managed as an explicit selected-users list.
* Fix: Settings and access control now save through one flow with a unified dirty-state signal.

### 10.3.6 – 2026-04-20
* Fix: Settings JavaScript and styles now always include a version hash to prevent stale browser cache after plugin updates.
* Enhancement: Asset version resolution is now hardened with per-file metadata and deterministic fallback behavior.

### 10.3.5 – 2026-04-20
* Enhancement: Manual single-target cache clear detection is now stricter and only accepts validated one-page targets.
* Fix: Targeted signal dedupe now includes the source hook, preventing accidental same-request suppression across different purge paths.
* Fix: Unsupported Breeze runtime URL hooks were removed from signal ingestion to avoid false targeted invalidation.
* Enhancement: Breeze now supports intent-based single-page invalidation from the admin “Clear cache” action with nonce and permission validation.

### 10.3.4 – 2026-04-19
* New: Added tokenized support debug view endpoints (`POST /support-debug/view-token`, `GET /support-debug/view`) for refresh-safe in-browser log viewing.
* Security: Support debug view tokens are short-lived, user-bound, and HMAC-signed; the view route validates token claims plus logged-in cookie identity and required capability.
* Fix: Support debug View no longer depends on `_wpnonce` URL auth, eliminating intermittent `rest_cookie_invalid_nonce` failures in wp-admin.
* Fix: Support debug tokenized view responses now enforce strict no-cache headers and timestamped open URLs to avoid stale cached content on reload.
* Fix: Trigger registry debug logs (`option_triggers_registered`, `global_triggers_registered`) now emit only for real admin UI requests (excluding cron, REST, AJAX, and favicon noise).
* Enhancement: `EmissionHelper` is now the single emission boundary for `ekesto_ci_invalidation_urls`; all emissions are canonicalized and always include `context.urls`.
* Fix: Canonical emission rules now enforce `GLOBAL_INVALIDATION` payloads with `urls=[]` and explicit skip logs for empty targeted payloads (`eci_emission_skipped_empty_targeted`).
* Enhancement: Added centralized adapter signal recursion/dedupe protection (`AdapterSignalGuard`) with request-local ingestion/suppression logging.
* New: Added `SystemInvalidationListener` for lifecycle/system ownership (`upgrader_process_complete`, plugin activate/deactivate, manual full refresh), with plugin lifecycle unified under `global_invalidation.plugin_update`.
* Enhancement: Adapter operational authority is now centralized in `AdapterManager::hasOperationalAdapter()` with fail-closed filter bridge `ekesto_ci_has_operational_adapter`.

### 10.3.3 – 2026-04-17
* Fix: NON_STRUCTURAL `elementor_library` saves now use neutral global fallback identity (`non_structural_change`) instead of Elementor template fallback attribution.
* Breaking: Replaced global option lifecycle listeners with explicit option-trigger registration only (`ekesto_ci_option_triggers`).
* New: Added selector-driven option execution filter (`ekesto_ci_option_selectors`) with strict explicit behavior (empty selectors are logged and skipped; no implicit global fallback).
* Breaking: Renamed extension global bypass filter to `ekesto_ci_global_triggers` (direct global invalidation path, no selector resolution).
* Enhancement: Internal option triggers now run through shared selector normalization/expansion for deterministic targeted-or-global execution.
* Enhancement: Added shared cache-plugin full-clear emission helper (`EmitsFullInvalidationFromCacheClear`) and wired adapter full-clear hooks across LiteSpeed, Breeze, Cache Enabler, FlyingPress, and WP Rocket.
* Enhancement: Added deterministic cache-plugin URL purge signal emission (`cache_plugin_url_purged`) for WP Rocket and FlyingPress from native URL-batch hooks.
* Fix: Adapter-initiated purge calls now suppress self-hook re-emission in WP Rocket and FlyingPress to prevent feedback-loop invalidation signals.

### 10.3.2 – 2026-04-16
* Fix: Namespaced Cache Enabler static purge calls (`\Cache_Enabler`) to guarantee global class resolution in adapter runtime checks.

### 10.3.1 – 2026-04-16
* New: Added Cache Enabler and Breeze cache adapters with deterministic URL-level purge support.
* Enhancement: Adapter registry now prioritizes deterministic URL-purge adapters only (LiteSpeed, WP Rocket, FlyingPress, Cache Enabler, Breeze).
* Fix: Hardened Cache Enabler static purge calls with callable checks to avoid runtime fatals when APIs are unavailable.

### 10.3.0 – 2026-04-16
* New: Added zero-config WooCommerce baseline invalidation for published product updates.
* Enhancement: WooCommerce baseline targets now include the product, Shop page, and core Woo pages through the existing multilingual purge pipeline.
* Fix: Product saves no longer fall through to single-product warmup when no product post type configuration is enabled.

### 10.2.2 – 2026-04-15
* Fix: Structural trigger routing now deduplicates before dispatch across WordPress and Elementor entrypoints.
* Fix: Elementor library and structural entity saves no longer fall back into content invalidation paths.

### 10.2.1 – 2026-04-15
* Enhancement: Simplified Post Type Tabs and `Content Relationships` visibility with a shared single-toggle model (`Show non-public post types`).
* Fix: Removed `show_disabled_post_types` from settings persistence and REST/UI payloads; only `include_non_public_post_types` now controls non-public expansion.
* Fix: Selected non-public post types now remain visible in both Post Type Tabs and `Content Relationships`, even when non-public expansion is off.
* Fix: Empty-state Post Type Tabs notice now switches to a success notice after the first unsaved enable action and clears after Save.

### 10.2.0 – 2026-04-14
* New: Added per-post-type `tags_enabled` support with an `Include tags (post_tag)` toggle in Additional Invalidation Rules.
* Fix: Enforced dual-layer `post_tag` gating in taxonomy listeners and archive resolution so tag-driven invalidation and tag archive URLs are excluded when disabled.
* Enhancement: Documented async queue behavior: tag setting changes affect future events, while already queued taxonomy jobs may still run until drained.
* Fix: Post Type Tabs and the unconfigured post type notice now exclude internal/structural/admin post types through a settings UI-only exclusion policy, without changing runtime invalidation behavior.
* Enhancement: Split Post Type Tabs visibility into independent `Show disabled post types` and `Include non-public post types` toggles.
* Fix: Configured post types now remain visible in Post Type Tabs regardless of both visibility toggles.
* Fix: Removed legacy `show_non_public_post_types`; settings now use only `include_non_public_post_types` with strict partial REST update persistence.

### 10.1.1 – 2026-04-13
* Fix: Release metadata now stays in sync across the plugin header, runtime version, and npm package files.
* Fix: Retained the Post Type Tabs and unconfigured notice refinements from 10.1.0 while correcting the release version mismatch.

### 10.1.0 – 2026-04-13
* Enhancement: Post Type Tabs now default to enabled tabs only and can reveal disabled eligible post types on demand.
* Fix: Unconfigured post type notice now tracks eligible post types, including non-public types, and matches the new tab model.
* Fix: Settings UI now keeps configured non-public tabs visible while hiding non-applicable panels and preserving save safety.

### 10.0.1 – 2026-04-12
* Fix: Restored all public post types in `Content Relationships` so related trigger types can be selected without enabling additional post type tabs.
* Fix: Kept `Additional Post Type Targets` selectors constrained to configured post types in both Post Type and Timed contexts.
* Fix: Removed React-only post-type discovery notice path and now show the classic dismissible wp-admin notice on all admin screens, including ECI settings.

### 10.0.0 – 2026-04-12
* New: Broke legacy/global archive settings and moved archive behavior to explicit per-post-type configuration only.
* Enhancement: Post Type Tabs management now lists all public post types while runtime remains configured-post-types-only.
* Enhancement: Added passive unconfigured post type detection with dismissible notices in both wp-admin and ECI settings.
* Fix: Dismissed unconfigured post type notices now persist per user and reappear only when a genuinely new post type is detected.
* Fix: Removed timed-rule taxonomy-archive toggle and aligned timed archive behavior to post-type archive configuration.
* Fix: Configure notice action now only opens the ECI Settings tab and no longer auto-enables post types.
* Fix: Structural target emissions (synced patterns, Elementor conditions, forms) now correctly set trusted URLs so downstream warmup no longer skips deterministic targets.
* Fix: `wp_template` edits now use resolver-first structural resolution instead of unconditional full-site flush.
* Fix: Widget-triggered targeted invalidation now receives trust from the executor safety net instead of hardcoded empty trust.

### 9.2.1 – 2026-04-11
* New: Timed invalidation rules now support additive `additional_post_types` targets so selected/all page targets can include singular URLs from selected post types.
* Enhancement: Added shared `PostTypeExpansionResolver` so post-trigger and timed expansion paths use one flat expansion implementation.
* Enhancement: `Additional Post Type Targets` panels now auto-open on load in both Post Type tabs and Timed rules when saved selected post types still exist.
* Fix: Timed `selected` rules are now schedulable when `page_ids` is empty but valid `additional_post_types` targets are configured.

### 9.2.0 – 2026-04-10
* New: Added per-post-type `expand_post_types` flat target expansion so post-trigger resolution can append singular targets from selected post types in the same pass.
* Enhancement: Post type expansion now uses deterministic incremental ID batching (`ID > last_id`, `ORDER BY ID ASC`) for large datasets.
* Fix: Flat post type expansion now hard-skips when any matched enabled host post type uses `target_mode=full_flush`.
* Enhancement: Added a collapsed `Post Type Expansion` settings panel at the bottom of each Post Type tab, reusing the existing post-type checkbox selector UI.
* Fix: Expansion debug logs now include per-post-type target counts plus running totals (`post type expansion: {post_type} -> {count} targets (total: {total})`).
* Fix: Support debug log timestamps now include explicit UTC and WordPress local time (`YYYY-MM-DD HH:MM:SS UTC (YYYY-MM-DD HH:MM:SS local)`) to remove timezone ambiguity during timed invalidation diagnostics.
* Enhancement: Timed `selected`/`all` invalidation now classifies deterministic targeted emission as `intent=content` with `resolution=CONTENT_DIRECT`.
* Fix: Timed trust flags are now applied only when final emitted URLs exactly match scheduler-derived deterministic targets; filter-extended URL sets remain untrusted.
* Enhancement: Timed trust diagnostics now log `trusted` plus `trust_decision_reason` (`deterministic_match` or `extended_by_filter`) for faster support tracing.
* Enhancement: Renamed widget-template mapping extension hook to `ekesto_ci_widget_template_mappings` for explicit widget trigger context.
* Fix: Developer reference now documents widget sidebar IDs (`$changedElements`) and classic template mapping use cases with clearer widget-focused guidance.
* Fix: Canonical precise URL resolution now rejects posts that are not publicly viewable before emission.
* Fix: Unpublished draft query URLs (for example `?page_id=<id>`) are no longer emitted as warmup targets.
* Fix: Timed invalidation now preserves `Purge all cache` (`full_flush`) target mode after settings save/reload.
* Enhancement: Timed target mode normalization is now aligned across sanitize/read/runtime paths (`selected|all|full_flush`).
* New: Gutenberg `wp_template_part` updates now resolve deterministically through `wp_template` assignments (`_wp_page_template == template post_name`) to publicly viewable posts for precise `GLOBAL_TARGETED` invalidation.
* Fix: Template-part resolver-targeted emissions now preserve `intent=GLOBAL_TARGETED` and `resolution=GLOBAL_TARGETED`, restoring downstream targeted purge/warmup execution.
* Fix: Template-part updates with no deterministic assigned publicly viewable targets now fallback to global invalidation with `fallback_reason=unresolved_template_usage`.

### 9.1.0 – 2026-04-07
* New: Elementor Component (`elementor_component`) edits via the Atomic editor now trigger structural invalidation through the `elementor/document/after_save` Document lifecycle hook.
* New: Elementor condition-based targeted resolution for presentation templates (`_elementor_conditions`) with deterministic GLOBAL_TARGETED execution.
* Enhancement: Elementor structural classifier now prioritizes STRUCTURAL_CONTAINER over PRESENTATION_TEMPLATE in runtime evaluation order.
* Enhancement: Elementor structural fallback types expanded with `loop-item` and `component` template types.
* Enhancement: Template reference parser now detects Loop Grid widgets (`loop-grid`) and Global widgets (`global`/`templateID`) alongside legacy `template` widgets.
* Enhancement: Component reference SQL prefilters now support both Atomic schema (`$$type` wrapper) and legacy schema (value only).
* Enhancement: Library structural provider SQL prefilters now include Global widget `templateID` needles alongside legacy `template_id`.
* Enhancement: Global executor now includes a defensive catch-all that auto-fixes and logs GLOBAL_TARGETED non-fallback emissions missing the `confirmed_resolution_target` trust flag.
* Enhancement: Structural entity resolved references now explicitly set `confirmed_resolution_target` trust flag.
* Fix: Elementor library update handler now deduplicates within a request to prevent double processing from multiple save hooks.

### 9.0.1 – 2026-04-06
* Enhancement: Reorganized runtime integration domains under `src/Integrations/*` (Builders, Caching, Forms, Multilingual, RelationshipProviders, Structural) with aligned `Ekesto\\CacheInvalidator\\Integrations\\*` namespaces.
* Fix: Updated core and resolver consumers to the new Structural integration namespace (`Integrations\\Structural\\*`) without changing invalidation behavior.
* Enhancement: Added `ArchiveDefaultsProvider` as the central optional archive-taxonomy defaults provider used by archive resolution paths.
* Fix: Synced version references across plugin header, runtime constant, and package manifests.

### 9.0.0 – 2026-04-05
* New: Added `SelectorResolver` as the shared selector contract layer for `ekesto_ci_post_selectors`, `ekesto_ci_archive_selectors`, and `ekesto_ci_widget_selectors`.
* Enhancement: Selector outcomes now aggregate across post/archive/widget surfaces before execution, with strict GLOBAL precedence and single global execution.
* Enhancement: Emission context now treats deterministic post-ID-only targets as confirmed resolution targets via `confirmed_resolution_target` and mirrored `resolver_confirmed_target`.
* Enhancement: Normalized trust-flag migration bridge annotations (`resolver_confirmed_target` -> `confirmed_resolution_target`) with explicit ECI/ECW version gates.
* Enhancement: Global executor outcome logs now include `final_outcome` and render `resolution=GLOBAL_INVALIDATION (fallback: <reason>)` when a meaningful fallback reason exists.
* Enhancement: Renamed graph internals to relationship terminology (`RelationshipGraphResolver`, `resolveRelationshipEdges`, `MAX_RELATIONSHIP_GRAPH_DEPTH`) while preserving runtime behavior.
* Enhancement: Switched post-type relationship settings contract key from `dependencies` to `relationships` across resolver, settings, and settings UI state payloads.
* Fix: Selector-targeted outcomes that resolve empty now escalate to `GLOBAL_FALLBACK` only when valid selectors were provided (`has_selectors=true`).
* Fix: Selector-targeted and deterministic meta-change emissions now mark confirmed resolver targets (including URL-only deterministic meta targets) while excluding fallback/global outcomes, so downstream warmup intake does not drop deterministic URLs when sitemap membership cache is cold.
* Fix: Replaced trust-bridge migration placeholders with concrete remove-after majors (`ECI >= 10.0`, `ECW >= 4.0`) without changing runtime behavior.
* Fix: Removed legacy wildcard/map selector semantics from developer docs and aligned extension guidance to strict selector-only behavior.
* Fix: Widget selector `GLOBAL` intent no longer gets skipped by `mapped_ids=0 fallback=off` guards in widget option and sidebar update paths.
* Fix: Removed temporary `DependencyGraphResolver` alias bridge after full resolver/callsite migration to `RelationshipGraphResolver`.

### 8.0.0 – 2026-04-03
* New: Replaced legacy Group/Scope invalidation with a Post Type relationship graph model and PT-centric settings storage (`post_types`).
* New: Added `DependencyGraphResolver` and removed legacy `GroupResolver` / `Groups` runtime classes.
* Enhancement: Refactored async deep invalidation to re-resolve propagation edges per job and dedupe discovered entities by `post_type:post_id`.
* Enhancement: Rebuilt settings UI around Post Type tabs with `Content Dependencies` and `Additional Invalidation Rules` panels.
* Fix: Added unsaved-changes leave guard in settings UI to prevent accidental navigation loss before `Save All`.

### 7.3.30 – 2026-04-02
* Enhancement: Added `resolver_confirmed_target` to resolver-first invalidation emission context for clearer downstream diagnostics.
* Enhancement: Updated `.wp-standards` submodule reference to latest standards snapshot.
* Fix: Synced version references across plugin header, runtime constant, and package manifests.

### 7.3.29 – 2026-04-02
* Enhancement: Added a shared Upgrade Safety Doctrine to internal handover guidance across WP Cache Autopilot plugins.
* Enhancement: Updated settings UI/docs content alignment and rebuilt admin settings assets for the latest release snapshot.
* Fix: Synced version references across plugin header, runtime constant, and package manifests.

### 7.3.28 – 2026-04-01
* Enhancement: Final test release restores changelog sync as a dedicated workflow triggered after successful `Build Release` runs or by manual dispatch.
* Enhancement: Release workflow now supports manual `workflow_dispatch` runs with an explicit tag input for rebuild/debug scenarios.
* Fix: Changelog source headings are normalized to `###` so exported docs can prepend their own page title cleanly.

### 7.3.27 – 2026-04-01
* Enhancement: Moved changelog publishing fully into the release workflow and removed the legacy manual `sync-changelog.yml` fallback.
* Fix: Release `sync_changelog` export now recreates `output/` before writing generated changelog markdown, keeping the final test run green.

### 7.3.26 – 2026-04-01
* Fix: Manual changelog sync now strips everything before the changelog heading with `sed`, avoiding heading-detection edge cases during export.

### 7.3.25 – 2026-04-01
* Enhancement: Manual changelog sync now publishes to `docs/changelog/<plugin>/index.md` in the public docs repo for cleaner page paths.
* Fix: Manual changelog sync now preserves markdown changelog headings from `readme.txt` instead of rewriting them during export.
* Fix: Manual changelog sync now retries rebased pushes to reduce public-repo update failures during concurrent doc updates.

### 7.3.24 – 2026-03-31
* Fix: Forms resolver now discovers builder-backed form containers through the shared builder bridge before confirmation.
* Fix: Forms matching now normalizes escaped builder-stored shortcode text before adapter extraction.
* Fix: Contact Form 7 alias expansion now accepts modern 64-character `_hash` values and 7-character prefixes.
* Enhancement: Structural visibility and resolver dispatch now use request-local memoization to avoid repeated registry and post-type rebuilds in hot paths.
* Enhancement: Navigation, template part, and synced pattern invalidation now share a resolver-first precise targeting path with consistent fallback handling.
* Fix: Post `update_published` invalidation now skips duplicate same-request save cycles before resolution work begins.
* New: Added internal structural-container providers and shared traversal for core structural entities plus Elementor templates/components.
* Fix: Elementor library/component traversal now classifies runtime document families conservatively so presentation and unknown templates escalate to uncertainty instead of traversal.
* Fix: Structural invalidation now suppresses partial precise emission whenever unresolved structural uncertainty remains.
* Enhancement: Structural post-type classification now comes from the provider registry so builder scan hints stay candidate-only and structural entities remain non-emittable.
* Fix: Purged structural post IDs no longer emit their own permalinks into downstream warmup URLs.

### 7.3.23 – 2026-03-30
* Fix: Global template/style invalidation now resolves full-site targets using publicly viewable post types so built-in pages are emitted.
* Enhancement: Removed dead duplicate public-content helper methods from legacy listeners to prevent query-semantics drift.
* Enhancement: Added internal multilingual sibling-ID expansion for numeric `ref` discovery.
* Fix: Synced pattern and navigation resolvers now match translated object siblings before purge fanout runs.
* Enhancement: Builder usage adapters now declare normalized deterministic scan hints instead of ad-hoc resolver inputs.
* Fix: Builder container discovery now stays inside resolver scan scope and marks adapter lookup failures as scan-gap uncertainty only.
* Fix: Builder container matching now expands translated siblings before confirmation; builder template containers remain structural-only.
* Cleanup: Removed legacy backward-compatible `Multilingual\WpmlLanguageResolver` shim class; all internal paths already use `Multilingual\Resolvers\WpmlLanguageResolver`.

### 7.3.22 – 2026-03-30
* Enhancement: Added `GlobalInvalidationExecutor` as the canonical terminal execution path for global invalidation.
* Fix: Rerouted non-content trigger global paths away from group resolution and enforced safe global escalation for fallback scenarios.
* Fix: Added request-terminal short-circuiting to stop duplicate purge/emission/resolver work after `GLOBAL_INVALIDATION`.
* Enhancement: Added emission metadata fields (`intent`, `resolution`, `fallback`, `fallback_reason`) while preserving compatibility trigger fields.
* Fix: Added forms metadata normalization so precise/fallback/terminal-skip outcomes emit consistent `intent`/`resolution`/`fallback` fields.
* Fix: Added excluded post-type direct-path metadata with normalized `resolution=CONTENT_DIRECT` for content-trigger consistency.
* Enhancement: Reduced noisy `updated_option` terminal-skip logs to invalidation-relevant option keys.

### 7.3.21 – 2026-03-29
* Fix: Updated Freemius SDK initialization to use direct associative-array parameters for deployment validation compatibility.

### 7.3.20 – 2026-03-29
* Enhancement: Moved Freemius SDK updates to a Composer-managed `vendor/freemius/wordpress-sdk` workflow.
* Enhancement: Switched runtime class loading to Composer PSR-4 only (`vendor/autoload.php`) and removed legacy `src/autoload.php`.
* Fix: Added gated Freemius bootstrap wrappers with Git Updater-aware auto-disable and explicit update-system separation.
* Fix: Release packaging now installs Composer dependencies before building the `-dist.zip` artifact.
* Fix: Release verification now enforces lifecycle uninstall wiring and fails if legacy root `uninstall.php` is present.

### 7.3.19 – 2026-03-29
* Fix: Improved Freemius deployment workflow.
* Fix: Moved uninstall cleanup to a lifecycle uninstall hook for Freemius compatibility.

### 7.3.18 – 2026-03-29
* Fix: Improved Freemius deployment workflow.

### 7.3.17 – 2026-03-29
* Fix: Improved Freemius deployment workflow.

### 7.3.16 – 2026-03-29
* Fix: Improved Freemius deployment workflow.

### 7.3.15 – 2026-03-29
* Fix: Improved Freemius deployment workflow.

### 7.3.14 – 2026-03-29
* Fix: Release publishing now uses GitHub CLI (`gh release create/upload`) instead of the Node 20-based `softprops/action-gh-release` action.
* Fix: Removed remaining Node 20 deprecation warnings from the release pipeline while preserving deterministic `-dist.zip` upload behavior.

### 7.3.13 – 2026-03-29
* Enhancement: Release workflow now syncs changelog docs to the public repo as a dependent job after successful tag builds.
* Fix: Updated sync workflows to `actions/checkout@v5` to align with GitHub Actions Node.js 24 migration requirements.
* Fix: Added `.gitmodules` to `.distignore` so VCS submodule metadata is excluded from distribution archives.

### 7.3.12 – 2026-03-29
* Fix: Corrected release CI version extraction escaping so plugin header parsing no longer returns an empty value in GitHub Actions.
* Fix: Restored reliable tag/version validation by parsing `* Version: x.y.z` header lines correctly during release builds.

### 7.3.11 – 2026-03-29
* Fix: Release CI now parses plugin header `Version:` values correctly from root PHP files, preventing false tag/version mismatch failures.
* Fix: Tag verification now compares against the extracted semantic version (for example `7.3.11`) instead of the literal `Version:` token.

### 7.3.10 – 2026-03-29
* Fix: Release CI now installs WP-CLI from the stable gh-pages PHAR URL, avoiding curl HTTP failures (`exit 22`) on tag builds.
* Fix: Pinned `wp-cli/dist-archive-command` to `v3.1.0`, which is compatible with runner WP-CLI `2.12.x`.

### 7.3.9 – 2026-03-29
* Fix: Release CI now installs a deterministic WP-CLI `2.13.0` PHAR before `wp-cli/dist-archive-command` installation, preventing the WP-CLI dependency mismatch (`^2.13`).
* Fix: Added an explicit WP-CLI version guard in release CI so incompatible runner versions fail early with a clear error.

### 7.3.8 – 2026-03-29
* Fix: Release CI now force-updates WP-CLI before installing `wp-cli/dist-archive-command`, resolving the `wp-cli/wp-cli ^2.13` dependency error.
* Enhancement: Updated checkout action to `actions/checkout@v5` to align with GitHub Actions Node.js 24 migration warnings.

### 7.3.7 – 2026-03-29
* Fix: Added explicit GitHub Actions `contents: write` permission so release creation succeeds on tag-triggered runs.
* Fix: Keeps automated distribution publishing on Releases functional for the `-dist.zip` artifact.

### 7.3.6 – 2026-03-29
* Fix: Restored `dist/` exclusion in `.distignore` to keep local/CI packaging artifacts out of release ZIPs.
* Fix: Release ZIP hygiene validation now allows required runtime `build/` assets while still blocking development-only paths.

### 7.3.5 – 2026-03-29
* Fix: Release CI now installs the WP-CLI `dist-archive` package before building distribution archives.
* Enhancement: Release packaging now fails fast when `.distignore` is missing and publishes a single deterministic `-dist.zip` asset.
* Fix: Added release ZIP hygiene validation to fail builds if development files are detected in the distribution artifact.

### 7.3.4 – 2026-03-28
* Enhancement: Refined admin settings SCSS to use shared semantic color tokens for more consistent panel, modal, table, and plugin-meta styling.
* Enhancement: Reworked public docs structure, cross-links, and scenario guidance for clearer invalidation and warmup documentation.
* New: Added legal-page source documents for Terms of Service, Privacy Policy, and Imprint / Legal Notice under `legal/`.
* Enhancement: Added a GitHub workflow to sync the `readme.txt` changelog into the public documentation repo on `main` updates.
* Enhancement: Setup changelog workflow.

### 7.3.3 – 2026-03-25
* Enhancement: Added shared `wp_cache_autopilot_capability` capability filter support (default `manage_options`) across settings and protected REST actions.
* Enhancement: Refined settings labels/help text for clearer invalidation terminology across global, group, timed, and support-debug controls.
* Enhancement: Updated group and timed-rule action controls to compact icon actions, with settings panel z-index/panel spacing polish for improved admin UI usability.
* Enhancement: Refined public docs wording/section labels and requirements linking for terminology consistency.
* Enhancement: Forms invalidation now writes one compact `forms_change summary` debug line per event outcome with key counters and uncertainty flags.
* Fix: Moved forms manager boot and forms adapter hook-registration traces to verbose-only logging to reduce support debug noise.
* Enhancement: Added request-local dedupe for verbose debug traces to suppress exact duplicate lines.

### 7.3.2 – 2026-03-24
* Enhancement: Support debug View now opens an authenticated inline log URL so browser refresh always loads the latest log content.
* Fix: Added explicit REST root/nonce boot config for support debug View authentication stability in wp-admin.
* Fix: Removed false popup-warning notice from support debug View when browsers return a null window handle despite opening the tab.

### 7.3.1 – 2026-03-24
* Fix: Support debug mode now writes DEBUG entries to the shared support log when support mode is enabled.
* Enhancement: Support debug log filenames now default to `.txt` while keeping legacy `.log` filenames readable.
* Enhancement: Support debug panel now includes compact reload/view/delete icon actions, with View opening the current log file URL in a new tab.
* Fix: Updated cache invalidator docs wording/anchors for support-debug behavior and terminology consistency.

### 7.3.0 – 2026-03-23
* New: Added group target mode `Purge all cache` (`full_flush`) for post/CPT-triggered invalidation.
* Enhancement: Post/CPT triggers now run adapter `purgeAll()` and emit broad URL coverage when any matched enabled group uses `full_flush`.
* Fix: Relationship propagation controls are now disabled with a warning when group target mode is `Purge all cache`, while saved propagation values remain preserved.
* Enhancement: Standardized public extension hook names under the compact `ekesto_ci_*` namespace.
* Fix: Renamed invalidation emission action to `ekesto_ci_invalidation_urls` and updated all plugin docs/examples accordingly.

### 7.2.1 – 2026-03-19
* Fix: Forms invalidation now resolves Contact Form 7 hash-based identifiers and legacy numeric IDs as equivalent references for match, purge, and emission flows.
* Fix: Ninja Forms invalidation now listens to both legacy `nf_*` and current `ninja_forms_*` admin form lifecycle hooks, with request-local dedupe to prevent duplicate dispatch.
* Fix: Selected-pages target IDs are now normalized and deduplicated in the settings UI, preventing mixed-type duplicate IDs (for example `123` and `"123"`).
* Fix: In selected-pages mode, saving with a complete selector list now removes stale/deleted target page IDs so ghost selections no longer inflate summary counts.
* Fix: Selected-pages summary now shows a warning notice when saved IDs cannot be previewed from the loaded selector dataset (deleted targets or truncated list).
* Fix: `Select pages` modal height is now locked per open session to prevent search/filter layout jumps, and the lock is released on window resize to restore responsive sizing.

### 7.2.0 – 2026-03-18
* New: Added built-in WooCommerce relationship defaults for variation parents, linked products (`_upsell_ids`, `_crosssell_ids`, `_children`), and option-backed core pages (shop, cart, checkout, my account, terms).
* Enhancement: Added internal `RelationshipDefaultsProvider` and kept default assembly order stable (ACF defaults, Woo defaults, filter extensions).
* Enhancement: Added definition-level dedupe so overlapping built-in and custom relationship definitions do not execute duplicate lookups.
* Enhancement: Kept `ekesto_ci_relationship_meta_keys` as the extension/override layer after built-in defaults.
* Fix: Updated relationship and WooCommerce docs wording to describe baseline Woo propagation as built-in behavior.

### 7.1.2 – 2026-03-17
* Enhancement: Added fuzzy search for page target selection in the settings modal (title and URL slug matching).
* Enhancement: Added inline highlight rendering for matched title and URL slug fragments in selector rows.
* Fix: Kept select-all, row toggle, and filtered-selection behavior unchanged while search metadata is propagated through the table.
* Enhancement: Clarified `ekesto_ci_widget_selectors` docs to describe post-ID targeting and added a custom-post-type (`book`) mapping example.
* Fix: Removed legacy relationship-definition compatibility (`meta_key`/`multiple`) from runtime normalization and docs; strategy-based `kind` definitions are now required.
* New: Added project-level multilingual fanout policy toggles in Global Invalidation settings: `multilingual_fanout_post_triggers_enabled` and `multilingual_fanout_non_post_triggers_enabled`.
* Enhancement: Unified multilingual fanout execution across post, async, taxonomy, presentation, forms, comments, metadata, timed invalidation, and full-flush emitted URL paths.
* New: Added `ekesto_ci_multilingual_fanout_mode` filter to override computed fanout mode per trigger context.
* Enhancement: Extended multilingual context reporting with `fanout_mode` and `fanout_languages` in purge/emission metadata for downstream observability.
* Fix: Corrected async multilingual fanout isolation per queued job and added Polylang URL-model fanout mapping for URL-target expansion.

### 7.1.1 – 2026-03-15
* New: Added v1 documentation landing/overview content with cross-linked docs entry points and deep section links.
* Enhancement: Clarified invalidation terminology in settings/help text (e.g. `Async deep invalidation`, timed invalidation target modes).
* Enhancement: Refined plugin readme wording around cache invalidation decision-engine responsibilities and best-effort cache purge behavior.

### 7.1.0 – 2026-03-13
* New: Added `ekesto_ci_post_selectors` support for non-public trigger post types without scope/group membership (mapped target IDs purge directly).
* Enhancement: Added a practical, fully commented `ekesto_ci_post_selectors` usage example and consolidated filter docs into one canonical section.
* Fix: Preserved GUI/group-managed behavior for public trigger post types with no matched scope/group (no mapping-only bypass).

### 7.0.1 – 2026-03-12
* Fix: Prevented WP Super Cache full-cache purge fatals on single-site installs by avoiding multisite-only purge arguments when `get_blog_option()` is unavailable.
* Fix: Preserved WP Super Cache multisite purge behavior by continuing to pass the current blog ID when multisite blog APIs are loaded.

### 7.0.0 – 2026-03-12
* New: Added first-party multilingual resolvers for WPML, Polylang, and TranslatePress.
* Enhancement: Introduced deterministic multilingual resolver detection order (WPML -> Polylang -> TranslatePress) with `ekesto_ci_language_resolver` remaining the final override.
* Enhancement: Target-page invalidation from post/CPT triggers now applies resolver-model language fanout for object-model and URL-model integrations.
* Enhancement: Emitted multilingual context now reports resolver-aware metadata (`resolver`, `resolver_model`, `languages`) instead of hardcoded WPML values.
* Fix: Added migration-safe backward-compatible WPML resolver handover class while moving built-ins under `Multilingual/Resolvers`.
* Fix: Generalized multilingual resolver contracts for dual translation models (object and URL), which requires custom resolver implementations to update for this release.

### 6.1.0 – 2026-03-12
* New: Added cache adapters for WP Rocket, W3 Total Cache, and FlyingPress.
* Enhancement: Added URL-level purge support for the new adapters via their native APIs/actions.
* Enhancement: Extended adapter manager fallback chain to include the new adapters after existing priority entries.

### 6.0.0 – 2026-03-10
* New: Strategy-based relationship definitions in `ekesto_ci_relationship_meta_keys` (`meta_exact`, `meta_serialized`, `post_parent`, `option_id`, `callback`).
* Enhancement: Native ACF auto-discovery now feeds the same normalized relationship-definition pipeline used by custom integrations.
* Enhancement: Relationship resolver now supports serialized integer matching and framework-agnostic documentation examples for WooCommerce, Meta Box, and The Events Calendar.

### 5.0.4 – 2026-03-09
* Enhancement: Unified plugin lifecycle flow under `Lifecycle` with explicit activation, deactivation, and guarded upgrade reconciliation.
* Fix: Moved timed-sync version state handling out of runtime bootstrap and into lifecycle upgrade handling for safer updates.
* Fix: Text domain loading now runs at `init` priority `0` to reduce early translation-load notice risk.
* Fix: Uninstall cleanup now removes lifecycle lock/sync state options and transient.

### 5.0.3 – 2026-03-06
* Fix: Bootstrap now preflights boot-critical classes and fails closed on incomplete plugin packages.
* Fix: Timed schedule reconciliation now defers heavy sync work away from sensitive update requests.
* Enhancement: Added local release verification for runtime files, build artifacts, metadata versions, and optional zip contents.

### 5.0.2 – 2026-03-05
* Fix: Standardized settings modal CSS class namespace across modal markup and styles.
* Enhancement: Synced built settings assets with modal class-name updates.

### 5.0.1 – 2026-03-05
* Fix: Removed `all`-hook based full-flush trigger interception that could cause excessive hook recursion and memory exhaustion.
* Fix: Restored direct extension full-flush hook registration flow for stable runtime behavior.

### 5.0.0 – 2026-03-05
* New: Added centralized invalidation emission helper with `ekesto_ci_additional_urls`, comment invalidation listener, and meta-change invalidation listener.
* New: Added extensibility filters for language resolver, runtime full-flush triggers, option-change trigger mapping, and post-type meta-change mapping.
* Enhancement: Added new global toggles for comment changes and theme switch changes in settings.
* Fix: Full-flush extension trigger map is now evaluated at runtime so late-registered filters are honored.
* Fix: `ekesto_ci_additional_urls` now uses the canonical callback contract `(array $urls, array $context): array`.

### 4.4.0 – 2026-03-04
* New: Page target mode support (`Selected pages` / `All pages`) for group settings and timed invalidation rules
* Enhancement: Modal-first page target selection UX with compact selected-page summaries (count + preview rows)
* Enhancement: Group create/edit now uses native WordPress modal flow
* Fix: Unified modal layout classes across settings modals to prevent footer/background inconsistencies

### 4.3.0 – 2026-03-04
* New: Timed invalidation rules in Settings tab with daily time + selected page ID targets (site timezone)
* New: Cron-backed timed invalidation scheduler with per-rule scheduling, cleanup, and emitted timed trigger context
* Enhancement: Settings REST payload now includes `timed_invalidation` and `site_timezone`
* Enhancement: Scheduled publish transitions (`future` -> `publish`) now emit a distinct `scheduled_publish` reason key

### 4.2.0 – 2026-03-02
* New: Forms invalidation module with adapter coverage for Contact Form 7, WPForms, Gravity Forms, and Ninja Forms
* New: Builder-aware reference scanning with centralized builder adapters and confidence-based scan profiles
* Enhancement: Global settings now include `forms_enabled` and `forms_global_fallback_enabled` toggles
* Enhancement: Cache adapter architecture moved to `src/Caching` with concrete adapters under `src/Caching/Adapters`
* Fix: Forms fallback path now limits broad invalidation to configured post/page cache targets only

### 4.1.3 – 2026-03-01
* Fix: Refreshed packaged admin build assets for the settings UI
* Enhancement: Normalized build artifact set in plugin distribution

### 4.1.2 – 2026-03-01
* Enhancement: Added explicit trigger metadata in emitted invalidation context (`trigger_class`, `trigger_event`)
* Enhancement: Async invalidation context now includes aggregated trigger details (`trigger_kinds`, `trigger_reasons`)

### 4.1.1 – 2026-03-01
* New: Global master switch to pause/resume all invalidation paths (post, taxonomy, presentation, and async)
* Enhancement: Clarified settings/admin wording for engine-wide pause behavior
* Fix: Global paused admin notice no longer appears on the plugin's own settings screen
* Fix: Async queue enqueuing/scheduling/processing now hard-stops when master switch is off

### 4.1.0 – 2026-03-01
* New: Presentation-layer invalidation triggers for menus, customizer saves, widget updates, reading settings, and permalink settings
* New: Block-entity coverage expanded with `wp_navigation` trigger handling
* New: Synced pattern invalidation with best-effort reference scan and global fallback
* New: Archive URL invalidation support for post type archives and publicly queryable taxonomy archives
* New: Adapter URL-purge capability path with emitted URL fallback when direct URL purge is unavailable
* New: Filter `ekesto_ci_widget_selectors` for mapping widget changes to explicit post IDs
* New: Filter `ekesto_ci_archive_urls` for archive URL target customization
* New: Filter `ekesto_ci_presentation_trigger_enabled` for runtime trigger gating
* New: Filter `ekesto_ci_should_fallback_global` for synced-pattern fallback control

### 4.0.0 – 2026-02-28
* Enhancement: Simplified propagation model to one source of truth using `groups[*].propagation` in both simple and advanced modes (breaking change)
* Fix: Simple mode propagation toggles now persist independently per post type/group tab
* Enhancement: Settings-tab async controls now bulk-apply async propagation values across all groups
* Fix: Removed legacy `simple_propagation` handling from REST payloads and option sanitization

### 3.0.0 – 2026-02-28
* Breaking: Refactored `Settings` architecture to DI-based services and removed public static behavior methods from `Settings`
* New: `Settings` is now a constants-only compatibility boundary (option key, route/path slugs, handles, defaults, exclusions)
* New: Settings internals split into focused services (repository, sanitizer, payload builder, REST controller, admin page)
* New: PHP is now the single source of truth for settings app config (`config` + `initial` passed to JS boot object)
* Fix: `GroupResolver::getWatchedPostTypesForScope()` advanced-mode path no longer references undefined variable
* Internal: Runtime consumers now resolve settings through plugin-managed services instead of `Settings::*` static methods

### 2.0.1 – 2026-02-19
* Improved: UI
* Fix: Multilingual related issues in page list.
* New: Excluded post types (page, attachment) bypass scope/group system — editing these post types purges the post itself directly without requiring group configuration
* New: Centralized exclusion constant `Settings::EXCLUDED_POST_TYPES` for consistent post type filtering across codebase

### 2.0.0 – 2026-02-19
* New: Relationship propagation — when a related post (e.g. collaborator) changes, affected host posts (e.g. events) are automatically purged via ACF relationship field auto-discovery
* New: Taxonomy propagation — term edits, deletions, and reassignments trigger targeted invalidation of affected posts and group target pages, scoped to watched post types only
* New: Async deep invalidation — background processing via WP-Cron with configurable delay, deterministic deduplication keys, safe queue drain, and 200-item size cap
* New: Single-emission policy — async mode defers URL emission to the processor; sync mode emits at end of processing
* New: Request-level dedup — prevents double emission when `save_post` and `set_object_terms` fire for the same post in one request
* New: PropagationPanel UI per group tab with toggles for relationships, taxonomies, and background processing
* New: GroupResolver class for mode-aware scope resolution with `getWatchedPostTypes()` helper for taxonomy scoping
* New: Filter `ekesto_ci_relationship_meta_keys` for extending relationship field discovery
* New: `Settings::isAsyncAvailable()` enforces `DISABLE_WP_CRON` at runtime — async scheduling is hard-blocked when cron is disabled
* New: ACF relationship map transient is multisite-aware (keyed per blog_id)
* New: Async processor re-resolves host posts from trigger info for deterministic results
* Fix: Advanced mode group resolution — editing a member post type (e.g. collaborator in an "event" group) now correctly resolves and purges the group's target pages
* Fix: `DISABLE_WP_CRON` logic — corrected operator precedence bug that could schedule cron when it shouldn't

### 1.2.11 – 2026-02-18
* Improved: UX

### 1.2.10 – 2026-02-18
* Improved: UX

### 1.2.9 – 2026-02-17
* Fix: Only register LiteSpeed Cache REST no-cache filter when LSC is active — prevents fatal error when switching to another cache plugin
* Enhancement: Dedicated CSS class variants for group list and page selection tables
* Enhancement: Renamed _footer.scss to _adapter-info.scss with scoped .eci-adapter-info styles

### 1.2.8 – 2026-02-15
* Added: Search functionality with keyboard shortcut (Shift+F) to filter pages in group settings
* Added: Click-to-toggle page selection — click anywhere on a table row to select/deselect
* Added: Visual feedback for selected pages in the table
* Added: Indeterminate checkbox state when some (but not all) pages are selected
* Added: NextControls wrapper components for standardized WordPress UI controls
* Improved: Page selection table UI with better styling and visual hierarchy
* Improved: Overall settings page styling with new SCSS partials for search and text

### 1.2.7 – 2026-02-14
* Added: Settings link on plugin page

### 1.2.6 – 2026-02-14
* Fix: Make preventLscCaching method public for filter callback

### 1.2.5 – 2026-02-14
* Fix: Prevent LiteSpeed Cache from caching plugin REST API responses — uses rest_pre_dispatch filter for reliable cache suppression

### 1.2.4 – 2026-02-14
* Fix: Prevent LiteSpeed Cache from caching REST API responses

### 1.2.3 – 2026-02-12
* Fixed: Options weren't saved correctly on production server

### 1.2.2 – 2026-02-12
* Missing build folder added.

### 1.2.1 – 2026-02-12
* UI improvements

### 1.2.0 – 2026-02-12
* New feature: Simple/Advanced group management modes — toggle post type visibility or create custom groups with multiple post types
* New components: ModeToggle, SimpleModePanel, AdvancedModePanel, GroupEditor, GroupList, CustomSection, ValidationErrors, SettingsTab
* REST API now supports mode, simple_mode_visibility, and custom_groups parameters
* Server-side validation for custom groups (unique labels, exclusive post type assignment)
* UI improvements: reusable panel layout (CustomSection), group key immutability hint and visual deactivation in edit mode

### 1.1.0 – 2026-02-07
* New feature: Settings page goes REACT!

### 1.0.4 – 2026-02-04
* Treat Site Editor template, template part, and global styles updates as full-site invalidations (purges target pages across all enabled groups).

### 1.0.3 – 2026-02-03
* Now only `publish` pages are shown
* Improved documentation

### 1.0.2 – 2026-02-02
* Github repo added

### 1.0.1 – 2026-02-02
* Improved documentation with real-world customization examples

### 1.0.0 – 2026-02-02
* Initial release with unified cache invalidation and trigger abstraction
