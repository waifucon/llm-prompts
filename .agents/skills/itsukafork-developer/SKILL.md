You are a Forkgram Developer. Always follow the universal guidelines in [`itsuka-general`](../itsuka-general/SKILL.md).

## Forkgram-Specific Directives & Workflow

1. **Feature Registration & Architecture:**
   - When developing a new feature or setting in Forkgram, register it across these layers:
     - **Config Definition & Helper:** In `org.telegram.messenger.forkgram` (e.g. creating a dedicated helper like `PhotoViewerHelper.kt` or defining preferences via `MessagesController.getGlobalMainSettings()`).
     - **Settings UI Activity:** `TMessagesProj/src/main/java/org/telegram/ui/ForkSettingsActivity.java` (declare item ID constant, add item to the list in `fillItems()` under the relevant section, and handle clicks in `onClick()`).
     - **Programmatic Config Layer:** `TMessagesProj/src/main/java/org/telegram/messenger/forkgram/ForkSettings.kt` (register under `OPTIONS` via `BoolOption`, `IntOption`, `StringOption`, etc.).
     - **Settings Search Indexing:** `TMessagesProj/src/main/java/org/telegram/ui/ProfileActivity.java` (add a `SearchResult` entry mapping the string and ID to `ForkSettingsActivity.highlight(...)` for global search support).

---

## Configuration Registration Examples

### 1. Config Key & Helper
File: `TMessagesProj/src/main/java/org/telegram/messenger/forkgram/MyFeatureHelper.kt`
```kotlin
package org.telegram.messenger.forkgram

import org.telegram.messenger.MessagesController

object MyFeatureHelper {
    const val PREF_DISABLE_OPTION = "disableOption"

    @JvmStatic
    fun isDisableOptionEnabled(): Boolean =
        MessagesController.getGlobalMainSettings().getBoolean(PREF_DISABLE_OPTION, false)
}
```

### 2. Setting UI Registration in `ForkSettingsActivity.java`
File: `TMessagesProj/src/main/java/org/telegram/ui/ForkSettingsActivity.java`:

- **Declare Item ID Constant:**
  ```java
  public static final int ID_DISABLE_OPTION = 58;
  ```

- **Add Item in `fillItems()` (e.g. under `ForkSectionMedia`):**
  ```java
  items.add(UItem.asButtonCheck(ID_DISABLE_OPTION, LocaleController.getString(R.string.DisableOption), LocaleController.getString(R.string.DisableOptionInfo))
      .setChecked(pref("disableOption", false)).setMultiline(true));
  ```

- **Handle Click Event in `onClick()`:**
  ```java
  } else if (id == ID_DISABLE_OPTION) {
      toggle("disableOption", item, view);
  ```

### 3. Programmatic Config Registration in `ForkSettings.kt`
File: `TMessagesProj/src/main/java/org/telegram/messenger/forkgram/ForkSettings.kt`:

- **Register in `OPTIONS` Map:**
  ```kotlin
  BoolOption("disableOption", ForkSettingsActivity.ID_DISABLE_OPTION, false),
  ```

### 4. Settings Search Index Registration in `ProfileActivity.java`
File: `TMessagesProj/src/main/java/org/telegram/ui/ProfileActivity.java`:

- **Add Search Result:**
  ```java
  new SearchResult(1059, getString(R.string.DisableOption), getString(R.string.ForkSettingsTitle), R.drawable.menu_fork, () -> f.presentFragment(new ForkSettingsActivity().highlight(ForkSettingsActivity.ID_DISABLE_OPTION))),
  ```
