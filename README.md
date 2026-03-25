# Talon.One Campaign Copier

🚀 **[Try it live here!](https://peterfoxflick.github.io/TalonOneCampaignCopy/)**

A robust, browser-based utility designed to seamlessly duplicate [Talon.One](https://talon.one/) campaigns across different applications or entirely different deployments (e.g., Sandbox to Production). 

This tool goes beyond simple superficial duplication. It acts as a deep-crawling dependency manager, parsing the Abstract Syntax Tree (AST) of your rulesets to ensure that all required attributes, custom effects, webhooks, and achievements are properly recreated, linked, and mapped in the destination environment.

## 🌟 Key Features & Capabilities

### 1. Core Campaign Data & Rules
* **Campaign Metadata:** Copies standard fields including name (appends " copy 02" to prevent naming collisions), state, tags, limits, features, description, start/end times, and evaluation settings.
* **Targeting & Stores:** Carries over linked stores, integrations, target audiences, and contexts.
* **Active Rulesets:** Copies the full active ruleset (conditions, effects, and bindings) and automatically activates it in the destination.

### 2. Smart Dependency Syncing
When migrating rules, the script parses the underlying JSON AST to find dependencies. If copying within the same deployment, it securely links the destination app to existing entities. If crossing deployments, it recreates them:
* **Custom Attributes:** Automatically finds and recreates/links custom attributes used across multiple entities (`CustomerProfile`, `CustomerSession`, `CartItem`, `Event`, `Campaign`).
* **Custom Effects:** Extracts custom effect names (`!`) and recreates/links them in the destination.
* **Webhooks:** Extracts webhook IDs (`callApi###`), strips deployment-specific authentication credentials to prevent crashes, and recreates/links them. Includes a fallback toggle to safely transform webhooks into standard `notification` effects if the user opts out.

### 3. Campaign-Level Assets
* **Achievements:** Copies campaign-specific achievements (title, target, period, recurrence).
* **Collections (Allowed Lists):** Automatically exports CSV data from the source collection and imports it directly into the newly created destination collection.
* **Coupons:** Copies up to 100 associated coupons, preserving their exact patterns, usage/discount limits, and expiration dates.

### 4. Structural ID Mapping
Because copying creates new database IDs in the destination, the script handles the complex ID-swapping required to keep rules from breaking:
* **Evaluation Groups (Folder Tree):** Can optionally recreate your exact Evaluation Group hierarchy (folders/trees), preserving `evaluationMode` (e.g., stackable) and `evaluationScope` (e.g., session), placing copied campaigns into their correct folders.
* **AST ID Swapping:** Automatically swaps old webhook and achievement IDs inside the ruleset payload with the newly generated destination IDs.
* **Loyalty Program Mapping:** Includes a UI step to dynamically map Source Loyalty Program IDs to Destination Loyalty Program IDs inside the rules.

## 🛠 How to Use

The tool operates in a simple 7-step wizard:

1. **Source Login:** Enter the subdomain, email, and password of the deployment you want to copy *from*.
2. **Source Application:** Select the specific application containing the campaigns.
3. **Destination Login:** Enter the credentials for the deployment you want to copy *into*.
4. **Destination Application:** Select the target application. *(Includes a "Danger Zone" option to wipe the destination application clean before copying).*
5. **Loyalty Mapping:** Match any loyalty programs from the source to their equivalents in the destination.
6. **Scope:** Choose to copy All Campaigns, Active Only, or a Single Campaign. You can also configure Evaluation Group logic and Webhook preferences here.
7. **Review & Copy:** Double-check your mappings and initiate the copy process. A progress bar and detailed activity log will track the migration in real-time.

## 🔒 Security & Privacy

This application is built entirely in a single HTML/JS file. **It has no backend server.** All API requests are made directly from your browser to the Talon.One Management API. Your credentials, tokens, and campaign data are never stored, tracked, or sent to any third-party servers. 

## 💻 Local Development

Because it is a single file without external build dependencies, running it locally is as easy as cloning the repo and opening the file in your browser:

```bash
git clone [https://github.com/peterfoxflick/TalonOneCampaignCopy.git](https://github.com/peterfoxflick/TalonOneCampaignCopy.git)
cd TalonOneCampaignCopy
open index.html
