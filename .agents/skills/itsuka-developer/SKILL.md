You are a Professional Nagram Developer.

## Core Directives & Workflow

1. **Locating Symbols:**
   - Always use `rg` (ripgrep) to search for symbols and locate references efficiently.

2. **Clean Imports:**
   - Always import classes explicitly instead of using inline, fully-qualified package names across the codebase.

3. **No Unrequested Compilation:**
   - Do not attempt to build or compile the project unless the user explicitly asks for it.

4. **Strict Conditional Logic (Preserve Existing `if` Checks):**
   - Do not convert existing sequential `if` checks into `if / else` blocks. Maintain the existing control flow patterns.
   - **Correct:**
     ```java
     if (NekoConfig.disableSystemAccount.Bool()) {
         deleteUnknownAppAccounts();
     }
     if (systemAccount == null && !NekoConfig.disableSystemAccount.Bool()) {
         try {
             TLRPC.User user = getUserConfig().getCurrentUser();
             systemAccount = new Account(formatName(user.first_name, user.last_name), BuildConfig.APPLICATION_ID);
             am.addAccountExplicitly(systemAccount, "", null);
         } catch (Exception e) {
             FileLog.e(e);
         }
     }
     ```
   - **Incorrect:**
     ```java
     if (NekoConfig.disableSystemAccount.Bool()) {
         deleteUnknownAppAccounts();
     } else {
         if (systemAccount == null) {
             try {
                 TLRPC.User user = getUserConfig().getCurrentUser();
                 systemAccount = new Account(formatName(user.first_name, user.last_name), BuildConfig.APPLICATION_ID);
                 am.addAccountExplicitly(systemAccount, "", null);
             } catch (Exception e) {
                 FileLog.e(e);
             }
         }
     }
     ```

5. **Feature Registration & Architecture:**
   - When developing a new feature or setting, register it in:
     - `TMessagesProj/src/main/kotlin/xyz/nextalone/nagram/NaConfig.kt`
     - Settings activity: `TMessagesProj/src/main/java/tw/nekomimi/nekogram/settings/NekoGeneralSettingsActivity.java` or `TMessagesProj/src/main/java/tw/nekomimi/nekogram/settings/NekoChatSettingsActivity.java` (depending on relevance).
     - String resources: `TMessagesProj/src/main/res/values/strings_na.xml`
   - **Never** add new features to `TMessagesProj/src/main/java/tw/nekomimi/nekogram/NekoConfig.java`. That file is legacy inherited from Nekogram/NekoX.

6. **Localization Scope:**
   - Add new strings **only in English** in `TMessagesProj/src/main/res/values/strings_na.xml`. Do not add translations for other language folders unless explicitly requested.

7. **Feature Descriptions in Settings:**
   - By default, do not add description subtitles for settings in `strings_na.xml` or the settings UI unless explicitly requested.
   - If a description is explicitly requested:
     - In `TMessagesProj/src/main/res/values/strings_na.xml`:
       ```xml
       <string name="DisableOptionDesc">Description of the feature</string>
       ```
     - In `NekoGeneralSettingsActivity.java` / `NekoChatSettingsActivity.java`:
       ```java
       private final AbstractConfigCell disableOptionRow = cellGroup.appendCell(new ConfigCellTextCheck(NaConfig.INSTANCE.getDisableOption(), LocaleController.getString(R.string.DisableOptionDesc)));
       ```

8. **UI Updates & App Restart Triggers:**
   - Never call both `parentLayout.rebuildAllFragmentViews(false, false);` and `tooltip.showWithAction(0, UndoView.ACTION_NEED_RESATRT, null, null);` together. Pick only one depending on whether an immediate view rebuild suffices or a restart tooltip is required.

9. **Protected Files:**
   - Do not revert changes in:
     - `TMessagesProj/release.keystore`
     - `TMessagesProj/jni/jni.c`

---

## Configuration Registration Examples

### 1. `TMessagesProj/src/main/res/values/strings_na.xml`
```xml
<string name="DisableOption">Disable option</string>
```

### 2. `TMessagesProj/src/main/kotlin/xyz/nextalone/nagram/NaConfig.kt`
- **Boolean Option:**
  ```kotlin
  val disableOption =
      addConfig(
          "DisableOption",
          ConfigItem.configTypeBool,
          false
      )
  ```
- **Integer Option:**
  ```kotlin
  val sideBarOption =
      addConfig(
          "SideBarOption",
          ConfigItem.configTypeInt,
          0
      )
  ```

### 3. Settings Activity Registration
In `NekoGeneralSettingsActivity.java` or `NekoChatSettingsActivity.java`:
```java
private final AbstractConfigCell disableOptionRow = cellGroup.appendCell(new ConfigCellTextCheck(NaConfig.INSTANCE.getDisableOption()));
```
