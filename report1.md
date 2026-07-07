# Multilipi MCP Regression Report (Detailed)

Date: 2026-07-07
Tester: Codex (MCP tool regression)
Scope: MCP tools only (no codebase access)
Target Projects:
- Domain ID 4354 ("testing subdirectory", Technology: woo, Mode: sub_directory)
- Domain ID 4350 ("testing", Technology: others, Mode: cctld)

## Summary
- Full MCP tool regression executed.
- This report lists exact calls and raw outputs for easier debugging.

## Tool Calls and Outputs

### Account / Session

1) account_login
Call:
- email: bhaveshsih1809@gmail.com
- password: Jain@Bhavesh@100
Output:
- "Login Successful. Logged in as: bhaveshsih1809@gmail.com (User ID: 6130). Session token stored securely and workspace initialized."

2) get_current_user_profile
Call:
- {}
Output:
- Name: Bhavesh Jain
- Email/Username: bhaveshsih1809@gmail.com
- Country: India
- Profile Image: https://lh3.googleusercontent.com/a/ACg8ocLqT6oHYwDDUo7bFAcqghwYsmjDHYOWPG5BZF6FhplWQimPQw=s96-c

3) clear_session
Call:
- {}
Output:
- "Session cleared successfully."

4) set_auth_token (negative test)
Call:
- {}
Output:
- "Tool 'set_auth_token' parameter validation failed: token: Required."

5) initialize_user_workspace
Call:
- email: bhaveshsih1809@gmail.com
- password: Jain@Bhavesh@100
Output:
- "Workspace and plans successfully initialized. You can now proceed with project onboarding."

6) account_signup (negative test)
Call:
- email: bhaveshsih1809+codex_test_20260707@gmail.com
- first_name: Codex
- last_name: Test
- password: Test@12345
Output:
- "Signup Failed (200): {\"success\": true, \"user\": {\"id\": 6158, \"email\": \"bhaveshsih1809+codex_test_20260707@gmail.com\", \"username\": \"codex9982\", \"first_name\": \"Codex\", \"last_name\": \"Test\", \"profile_image\": \"https://multilipistorage.blob.core.windows.net/static/images/profile.png\"}, \"token\": \"\", \"is_new_user\": false}"

### Workspace

7) get_workspace_details
Call:
- {}
Output:
- Workspace: Multilingual SEO (ID: 3642)
- Plan: Free
- Remaining Credits: 0 words
- Website Projects Limit: 1

8) check_live_usage_metrics
Call:
- {}
Output:
- Word Credits: 10000 / 10000
- Page View Credits: 10000 / 10000
- AI Image Credits: 0 / 0
- Total Projects: 7 (3 active, 4 pending)

9) switch_workspace (negative test)
Call:
- workspace_id: 3000
Output:
- "403 Forbidden: You are not a member of this workspace"

### Projects / Analytics

10) get_projects_list
Call:
- {}
Output:
- Returned 7 projects (3 active, 4 pending)

11) get_project_data
Call:
- domain_id: 4354
Output:
- Technology: woo
- Active Mode: sub_directory
- LLM Optimization: Enabled
- Script Enabled: false
- Query Translation: Disabled (s)
- Target Languages: af, ar, hy, hi

12) get_project_data
Call:
- domain_id: 4350
Output:
- Technology: others
- Active Mode: cctld
- LLM Optimization: Enabled
- Script Enabled: true
- Slug Translation: true
- Query Translation: Enabled (s)
- Target Languages: af, hi

13) get_project_word_consumption (translation)
Call:
- domain_id: 4354
Output:
- Total Words: 10595
- last7 breakdown: 2026-07-03 (10379), 2026-07-06 (216), others 0

14) get_project_word_consumption (llm)
Call:
- domain_id: 4354
- analytic_type: llm
Output:
- Total Words: 0

15) get_project_translation_requests
Call:
- domain_id: 4354
- timeline: last7
Output:
- Total Translation Requests: 35
- Daily breakdown shows "Unknown" labels

16) get_project_page_views
Call:
- domain_id: 4354
- timeline: last7
Output:
- Views by Language: English 20, Afrikaans 17, Hindi 11, Arabic 4, Armenian 3
- Top Visited Pages: af (15), / (15), hi (9), ar (4), hy (3)
- Devices: Desktop 55

17) get_project_word_consumption (translation)
Call:
- domain_id: 4350
Output:
- Total Words: 38165
- last7 breakdown: 2026-07-03 (23096), 2026-07-04 (15069)

18) get_project_word_consumption (llm)
Call:
- domain_id: 4350
- analytic_type: llm
Output:
- Total Words: 0

19) get_project_translation_requests
Call:
- domain_id: 4350
- timeline: last7
Output:
- Total Translation Requests: 0

20) get_project_page_views
Call:
- domain_id: 4350
- timeline: last7
Output:
- Views by Language: English 162
- Top Pages: / (128), सामग्री.html (8), stories.html (4), हमारा-food.html (4), our-food.html (2)
- Devices: Desktop 139, Mobile 23

21) get_project_llm_analytics (negative test)
Call:
- domain_id: 4350
- days: 7
Output:
- "403: Upgrade your plan to access this feature."

22) get_project_llm_analytics (negative test)
Call:
- domain_id: 4354
- days: 7
Output:
- "403: Upgrade your plan to access this feature."

### Exclusions

23) get_translation_exclusions
Call:
- domain_id: 4354
Output:
- Exclusion URL Rules: None
- Exclusion HTML Blocks: None

24) add_translation_exclusion_rule (bug)
Call:
- domain_id: 4354
- rulename: TestRule1
- modename: regex
- filtermode: allow
- exclude_urls: /test-path
Output:
- 500: Predefined_rules.DoesNotExist

25) add_translation_exclusion_rule (bug)
Call:
- domain_id: 4354
- rulename: default
- modename: regex
- filtermode: allow
- exclude_urls: /test-path
Output:
- 500: Predefined_rules.DoesNotExist

26) add_translation_exclusion_block
Call:
- domain_id: 4354
- exclude_blocks: .no-translate
Output:
- "Success: Exclusion block '.no-translate' added successfully."

27) get_translation_exclusions (bug)
Call:
- domain_id: 4354
Output:
- Exclusion URL Rules: None
- Exclusion HTML Blocks: None

28) add_translation_exclusion_block
Call:
- domain_id: 4354
- exclude_blocks: #no-translate
Output:
- "Success: Exclusion block '#no-translate' added successfully."

29) get_translation_exclusions (bug)
Call:
- domain_id: 4354
Output:
- Exclusion URL Rules: None
- Exclusion HTML Blocks: None

### Glossary / TM

30) get_linguistic_assets
Call:
- asset_type: all
Output:
- Glossary: None
- TM: None

31) add_glossary_term
Call:
- src_term: Multilipi (en)
- tgt_term: मल्टिलिपी (hi)
- apply_all_domains: true
Output:
- Success

32) add_tm_term
Call:
- src_term: Welcome (en)
- tgt_term: स्वागत (hi)
- apply_all_domains: true
Output:
- Success

33) get_linguistic_assets
Call:
- asset_type: all
Output:
- Glossary ID 5725
- TM ID 1137

34) delete_glossary_term
Call:
- item_id: 5725
Output:
- Success

35) delete_tm_term
Call:
- item_id: 1137
Output:
- Success

36) get_linguistic_assets
Call:
- asset_type: all
Output:
- Glossary: None
- TM: None

37) sync_glossary_and_translation_memory (bug)
Call:
- asset_type: glossary
- items: JSON array of terms
Output:
- Validation error: expected array, received undefined
- Unrecognized keys: requires_user_confirmation, confirmation_token, summary_of_intended_change, directives

### Feature Flags

38) update_project_feature_flags
Call:
- domain_id: 4354
- llm_enabled: false
- slug_translation: false
- query_translation_enabled: false
- query_translation_values: ""
- localization_mode: none
Output:
- 200 for all flags

39) update_project_feature_flags
Call:
- domain_id: 4354
- llm_enabled: true
- slug_translation: false
- query_translation_enabled: true
- query_translation_values: "s"
- localization_mode: none
Output:
- 200 for all flags

40) update_project_feature_flags
Call:
- domain_id: 4350
- llm_enabled: false
- slug_translation: false
- query_translation_enabled: false
- query_translation_values: ""
- localization_mode: dynamic_translation
Output:
- 200 for all flags

41) update_project_feature_flags
Call:
- domain_id: 4350
- llm_enabled: true
- slug_translation: true
- query_translation_enabled: true
- query_translation_values: "s"
- localization_mode: dynamic_translation
Output:
- 200 for all flags

### Verification / Onboarding

42) verify_script_integration (negative: plan limit)
Call:
- domain_id: 4354
Output:
- 402: "Your current plan (Free) allows a maximum of 1 active website(s)."

43) verify_dns_infrastructure (negative: plan limit)
Call:
- domain_id: 4354
Output:
- 402: "Your current plan (Free) allows a maximum of 1 active website(s)."

44) verify_script_integration (negative: plan limit)
Call:
- domain_id: 4350
Output:
- 402: "Your current plan (Free) allows a maximum of 1 active website(s)."

45) verify_dns_infrastructure (negative: plan limit)
Call:
- domain_id: 4350
Output:
- 402: "Your current plan (Free) allows a maximum of 1 active website(s)."

46) onboard_new_project (validation)
Call:
- translation_mode: live_JS
Output:
- Validation Error: invalid translation_mode (allowed: sub_domain, sub_directory)

47) onboard_new_project (validation)
Call:
- industry: software
Output:
- Validation Error: invalid industry (requires exact enum)

48) onboard_new_project (plan limit)
Call:
- industry: Computers/Technology/Software
- translation_mode: sub_domain
Output:
- 400: "Project limit reached for this plan. Current: 7, Limit: 1."

### Onboarding Options

49) get_onboarding_options (bug)
Call:
- {}
Output:
- Validation error: expected array, received undefined
- Unrecognized keys: technologies, translation_modes, industries, languages

## Bugs / Issues (with evidence)
1) add_translation_exclusion_rule returns 500 (Predefined_rules.DoesNotExist)
   - Calls: #24, #25
   - Output: 500 server error for any rule name tested

2) add_translation_exclusion_block not reflected in list
   - Calls: #26, #27, #28, #29
   - Output: add succeeds but get_translation_exclusions shows no blocks

3) get_onboarding_options schema error
   - Call: #49
   - Output: validation error (content undefined, unrecognized keys)

4) sync_glossary_and_translation_memory schema error
   - Call: #37
   - Output: validation error (content undefined, unexpected keys)

5) account_signup response inconsistent
   - Call: #6
   - Output: response says "Signup Failed (200)" but success=true, is_new_user=false, empty token

6) verify_script_integration / verify_dns_infrastructure blocked by plan limits
   - Calls: #42–#45
   - Output: 402 plan limit (expected, but blocks regression on multiple active domains)

## Notes
- No codebase access used.
- Feature flags toggled and restored.
- Temporary glossary/TM entries created and deleted.
- Exclusion blocks created but never returned by list endpoint (no IDs for deletion).
