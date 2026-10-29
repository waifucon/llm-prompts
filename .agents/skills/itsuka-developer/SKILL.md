You are a Nagram Developer. Always follow the universal guidelines in [`itsuka-general`](../itsuka-general/SKILL.md).

## Nagram-Specific Directives & Workflow

1. **Feature Registration & Architecture:**
   - When developing a new feature or setting in Nagram, register it across these layers:
     - `TMessagesProj/src/main/kotlin/xyz/nextalone/nagram/NaConfig.kt`
     - Settings activity: `TMessagesProj/src/main/java/tw/nekomimi/nekogram/settings/NekoGeneralSettingsActivity.java` or `TMessagesProj/src/main/java/tw/nekomimi/nekogram/settings/NekoChatSettingsActivity.java` (depending on relevance).
     - String resources: `TMessagesProj/src/main/res/values/strings_na.xml` (refer to [`itsuka-strings`](../itsuka-strings/SKILL.md) for string management and localization).
   - **Never** add new features to `TMessagesProj/src/main/java/tw/nekomimi/nekogram/NekoConfig.java`. That file is legacy inherited from Nekogram/NekoX.

2. **UI Updates & App Restart Triggers:**
   - Never call both `parentLayout.rebuildAllFragmentViews(false, false);` and `tooltip.showWithAction(0, UndoView.ACTION_NEED_RESATRT, null, null);` together. Pick only one depending on whether an immediate view rebuild suffices or a restart tooltip is required.

---

## Configuration Registration Examples

### 1. String Resource in `strings_na.xml`
File: `TMessagesProj/src/main/res/values/strings_na.xml`
```xml
<string name="DisableOption">Disable option</string>
```

### 2. Config Key in `NaConfig.kt`
File: `TMessagesProj/src/main/kotlin/xyz/nextalone/nagram/NaConfig.kt`

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

### 3. Setting UI Registration in Settings Activity
File: `TMessagesProj/src/main/java/tw/nekomimi/nekogram/settings/NekoGeneralSettingsActivity.java` or `TMessagesProj/src/main/java/tw/nekomimi/nekogram/settings/NekoChatSettingsActivity.java`:
```java
private final AbstractConfigCell disableOptionRow = cellGroup.appendCell(new ConfigCellTextCheck(NaConfig.INSTANCE.getDisableOption()));
```

### 4. Reading Config Values in Application Code
Because `NaConfig` properties return `ConfigItem` wrappers, for example use `.Bool()` / `.Int()` to read values:
```java
// Reading boolean config
if (NaConfig.INSTANCE.getDisableOption().Bool()) {
    // ...
}

// Reading integer config
int value = NaConfig.INSTANCE.getSideBarOption().Int();
```
