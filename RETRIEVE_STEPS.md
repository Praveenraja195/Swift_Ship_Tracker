# Retrieving SwiftShip metadata from Salesforce

Run these on any computer. The repo already contains the SFDX project files
(`sfdx-project.json`, `force-app/`, `manifest/package.xml`).

## 1. Install Salesforce CLI and Git (once per computer)

```powershell
winget install Salesforce.CLI
winget install Git.Git
```

Close and reopen the terminal afterwards so `sf` and `git` are found.

## 2. Clone the repo and enter it

```powershell
cd $HOME\Downloads
git clone https://github.com/Praveenraja195/Swift_Ship_Tracker.git
cd Swift_Ship_Tracker
```

## 3. Log in to the Developer org that holds SwiftShip

```powershell
sf org login web --alias swiftship --set-default
```

Sign in in the browser, then confirm the username is the right org:

```powershell
sf org display
```

## 4. Retrieve the metadata

```powershell
sf project retrieve start --manifest manifest/package.xml
```

Files appear under `force-app/main/default/`. Check that these exist:

- `objects/Parcel__c/`, `objects/Delivery__c/`, `objects/Sender__c/`, `objects/Receiver__c/`
- `flows/` with the Parcel Details flow
- `genAiPromptTemplates/` with Retrieve Parcel Details
- `genAiPlannerBundles/` with the agent planner and the Parcel Tracker topic
- `permissionsets/` with Swift Ship

If a type came back empty, list what the org actually has and fix the
name in `manifest/package.xml`:

```powershell
sf org list metadata --metadata-type Flow
sf org list metadata --metadata-type GenAiPromptTemplate
sf org list metadata --metadata-type GenAiPlannerBundle
sf org list metadata --metadata-type PermissionSet
```

## 5. Commit and push

```powershell
git add .
git commit -m "Add SwiftShip Tracker metadata retrieved from Developer org"
git push
```
