You are a Professional Nullgram Developer.

## Core Directives & Workflow

1. **Locating Symbols:**
   - Always use `rg` (ripgrep) to search for symbols and locate references efficiently.

2. **Clean Imports:**
   - Always import classes explicitly instead of using inline, fully-qualified package names across the codebase.

3. **No Unrequested Compilation:**
   - Do not attempt to build or compile the project unless the user explicitly asks for it.

4. **Strict Conditional Logic (Preserve Existing `if` Checks):**
   - Do not convert existing sequential `if` checks into `if / else` blocks. Maintain existing control flow patterns.
   - **Correct:**
     ```java
     if (Config.disableSystemAccount) {
         deleteUnknownAppAccounts();
     }
     if (systemAccount == null && !Config.disableSystemAccount) {
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
     if (Config.disableSystemAccount) {
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
   - When developing a new feature or setting in Nullgram, register it across these layers:
     - **Config Definition:** `TMessagesProj/src/main/java/top/qwq2333/nullgram/utils/Defines.kt` (using `@BooleanConfig`, `@StringConfig`, `@IntConfig`, etc. annotations).
     - **Settings UI Activity:** In the appropriate activity depending on where the option belongs:
       - `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/GeneralSettingActivity.java`
       - `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/ChatSettingActivity.java`
       - (or other settings activities under `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/` such as `ExperimentSettingActivity.java`).
     - **String Resources:** `TMessagesProj/src/main/res/values/strings_nullgram.xml` (refer to [`itsuka-strings`](../itsuka-strings/SKILL.md) for string management and localization).

6. **Protected Files:**
   - Do not revert changes in:
     - `TMessagesProj/release.keystore`
     - `TMessagesProj/jni/jni.c`

---

## Configuration Registration Examples

### 1. Config Key in `Defines.kt`
File: `TMessagesProj/src/main/java/top/qwq2333/nullgram/utils/Defines.kt`
```kotlin
@BooleanConfig const val disableOption = "disableOption"
@StringConfig("DefaultValue") const val customOption = "customOption"
```

### 2. String Resource in `strings_nullgram.xml`
File: `TMessagesProj/src/main/res/values/strings_nullgram.xml`
```xml
<string name="DisableOption">Disable Option</string>
```

### 3. Setting UI Registration in Settings Activity
File: `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/GeneralSettingActivity.java` or `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/ChatSettingActivity.java`:

- **Declare Row Field:**
  ```java
  private int disableOptionRow;
  ```

- **Initialize Row in `updateRows()`:**
  ```java
  disableOptionRow = addRow("disableOption");
  ```

- **Handle Click Event in `onItemClick()`:**
  ```java
  } else if (position == disableOptionRow) {
      Config.toggleDisableOption();
      if (view instanceof TextCheckCell) {
          ((TextCheckCell) view).setChecked(Config.disableOption);
      }
  ```

- **Bind View in `onBindViewHolder()`:**
  ```java
  } else if (position == disableOptionRow) {
      textCell.setTextAndCheck(LocaleController.getString("DisableOption", R.string.DisableOption),
          Config.disableOption, true);
  ```
