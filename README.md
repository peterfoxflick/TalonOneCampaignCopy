This is a copying tool to copy campaigns across different deployments. 

Here is a summary of everything the script can currently copy and synchronize from a source application to a destination application:

1. Core Campaign Data & Rules
Campaign Metadata & Settings: Copies standard fields including name (appending " copy 02"), state, tags, limits, features, description, startTime, endTime, type, attributes, reevaluateOnReturn, couponSettings, referralSettings, and campaignGroups.

Targeting & Stores: Copies linkedStoreIds, linkedStoreIntegrationIds, targetedAudienceIds, and contextId.

Active Rulesets: Copies the full active ruleset (conditions, effects, and bindings), automatically activating it in the destination.

2. Application & Deployment-Level Dependencies (Smart Sync)
The script parses the ruleset's Abstract Syntax Tree (AST) to find required dependencies. If copying to the same deployment, it securely links the destination app to existing entities. If crossing deployments, it recreates them:

Custom Attributes: Automatically finds and recreates/links custom attributes used in the campaign or ruleset across multiple entities (CustomerProfile, CustomerSession, CartItem, Event, Campaign).

Custom Effects: Parses the ruleset for the ! operator, extracts custom effect names, and recreates/links them in the destination.

Webhooks: Finds callApi### effects. It safely drops the authenticationId (to prevent cross-deployment crashes) and recreates/links the webhook. It also features a fallback: if the user opts out of copying webhooks, it safely transforms them into standard notification effects in the destination ruleset.

3. Campaign-Level Dependencies
Achievements: Copies campaign-specific achievements (including title, target, period, and recurrence policies).

Collections (Allowed Lists): Recreates campaign collections and actually exports the CSV data from the source and imports it into the destination.

Coupons: Copies up to 100 associated coupons, preserving their exact patterns, usage limits, discount limits, and expiration dates.

4. Structure & ID Mapping
Because copying creates new database IDs in the destination, the script handles the complex ID-swapping required to keep rules from breaking:

Evaluation Groups (Folder Tree): Can optionally recreate your exact Evaluation Group hierarchy (folders/trees), preserving evaluationMode (e.g., stackable) and evaluationScope (e.g., session), and places the copied campaigns into their correct folders.

Webhook ID Mapping: Swaps the old webhook ID in the ruleset (e.g., callApi189) with the newly generated destination ID (e.g., callApi452).

Achievement ID Mapping: Swaps the old achievement IDs with the newly generated ones so rules like updateAchievementProgress continue to work.

Loyalty Program Mapping: Takes user input from the UI (Step 5) to dynamically swap Source Loyalty Program IDs with Destination Loyalty Program IDs inside the ruleset.
