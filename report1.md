# Multilipi MCP Regression Report

Date: 2026-07-07
Tester: Codex (MCP tool regression)
Scope: MCP tools only (no codebase access)
Target Projects: 
- Domain ID 4354 ("testing subdirectory", Technology: woo, Mode: sub_directory)
- Domain ID 4350 ("testing", Technology: others, Mode: cctld)

## Summary
- Ran a broad regression across available MCP tools.
- Confirmed core read endpoints and several mutation flows.
- Identified multiple API/tool issues and plan-limit/permission constraints.

## Tools Tested and Results
### Account / Session
- account_login: PASS (valid credentials) 
- get_current_user_profile: PASS
- clear_session: PASS (required re-login afterwards)
- set_auth_token: FAIL (validation error when token missing)
- account_signup: WARN (response labeled "Signup Failed (200)" but payload shows success=true with is_new_user=false; token empty)
- initialize_user_workspace: PASS

### Workspace
- get_workspace_details: PASS
- check_live_usage_metrics: PASS
- switch_workspace: FAIL for workspace_id=3000 (403 Forbidden: not a member)

### Projects / Analytics (Active projects 4354, 4350)
- get_projects_list: PASS
- get_project_data: PASS
- get_project_word_consumption (translation + llm): PASS
- get_project_translation_requests: PASS (note: breakdown has "Unknown" labels)
- get_project_page_views: PASS
- get_project_llm_analytics: FAIL (403: upgrade required)

### Exclusions
- get_translation_exclusions: PASS (but see bug below)
- add_translation_exclusion_rule: FAIL (500: Predefined_rules.DoesNotExist)
- add_translation_exclusion_block: PASS (API success)
- delete_translation_exclusion_block: NOT TESTED (blocked: exclusion blocks not returned by list API, so no IDs to delete)
- delete_translation_exclusion_rule: NOT TESTED (rules never created; no rule IDs)

### Glossary / Translation Memory
- get_linguistic_assets: PASS
- add_glossary_term: PASS
- update_glossary_term: PASS
- delete_glossary_term: PASS
- add_tm_term: PASS
- update_tm_term: PASS
- delete_tm_term: PASS
- sync_glossary_and_translation_memory: FAIL (schema/validation error)

### Project Feature Flags
- update_project_feature_flags: PASS (toggled llm/slug/query settings on 4354 and 4350 and restored)

### Verification / Onboarding
- verify_script_integration: FAIL (402 plan limit: max 1 active site)
- verify_dns_infrastructure: FAIL (402 plan limit: max 1 active site)
- onboard_new_project: FAIL
  - validation error for translation_mode "live_JS"
  - validation error for industry "software" (requires exact enum)
  - blocked by plan limit (project limit reached)

## Issues / Bugs Found
1. add_translation_exclusion_rule returns 500: Predefined_rules.DoesNotExist
   - Expected: rule creation or validation error with allowed rule names.
   - Actual: server error for any rulename tested.

2. add_translation_exclusion_block succeeds but get_translation_exclusions returns none
   - Expected: added HTML block IDs listed for deletion.
   - Actual: list remains empty after successful add (block not retrievable).

3. get_onboarding_options fails with schema/validation error
   - Expected: options payload (technologies, translation_modes, industries, languages).
   - Actual: tool returns validation error: “Invalid input: expected array, received undefined” and “Unrecognized keys”.

4. sync_glossary_and_translation_memory fails with schema/validation error
   - Expected: batch insert/confirm flow.
   - Actual: tool returns validation error (content undefined, unexpected keys).

5. account_signup response inconsistent
   - Expected: success=true with normal success message.
   - Actual: “Signup Failed (200)” but success=true, is_new_user=false, empty token.

6. Plan limit blocks verification flows
   - verify_script_integration / verify_dns_infrastructure fail with 402 for Free plan (max 1 active website).
   - Not a bug, but a constraint that blocks regression on multiple active domains.

## Notes
- No codebase access was used to keep testing fair.
- Feature flags were toggled and restored to original values.
- Temporary glossary/TM entries were created and deleted.
- Exclusion blocks were created but could not be listed or deleted.

## Recommendations
- Fix exclusions API: ensure added HTML blocks are retrievable and deletable.
- Fix exclusion rule creation: return valid rules list or create custom rules without 500 errors.
- Repair onboarding options tool schema.
- Repair batch glossary/TM sync schema.
- Normalize account signup responses.
- For full verification testing, provide a plan/workspace that allows >1 active site.
