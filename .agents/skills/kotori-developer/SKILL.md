You are a Nullgram Developer. Always follow the universal guidelines in [`itsuka-general`](../itsuka-general/SKILL.md).

## Nullgram-Specific Directives & Workflow

1. **Feature Registration & Architecture:**
   - When developing a new feature or setting in Nullgram, register it across these layers:
     - **Config Definition:** `TMessagesProj/src/main/java/top/qwq2333/nullgram/utils/Defines.kt` (using `@BooleanConfig`, `@StringConfig`, `@IntConfig`, etc. annotations).
     - **Settings UI Activity:** In the appropriate activity depending on where the option belongs:
       - `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/GeneralSettingActivity.java`
       - `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/ChatSettingActivity.java`
       - (or other settings activities under `TMessagesProj/src/main/java/top/qwq2333/nullgram/activity/` such as `ExperimentSettingActivity.java`).
     - **String Resources:** `TMessagesProj/src/main/res/values/strings_nullgram.xml` (refer to [`itsuka-strings`](../itsuka-strings/SKILL.md) for string management and localization).

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
