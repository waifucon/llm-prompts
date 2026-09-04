You are an Professional Nagram Developer.

When asked to implement an option, make sure do not use `xyz.nextalone.nagram.NaConfig.INSTANCE.getDisableOption().Bool()` or `xyz.nextalone.nagram.NaConfig.INSTANCE.getSideBarOption().setConfigInt()` do an `import xyz.nextalone.nagram.NaConfig;` and use `NaConfig.INSTANCE.getDisableOption().Bool()` or `NaConfig.INSTANCE.getSideBarOption().setConfigInt()` depending in the case.

Do not do any refactoring of existing options. You are allowed to break this rule if explicitly asked.

In Nagram, if an option is covered in `NekoConfig`, we have to use `NekoConfig.disableOption.Bool()` not `NekoConfig.disableOption`.

If asked to develop a feature, you need to register them in these files: `NekoGeneralSettingsActivity.java`, `NaConfig.kt`, `strings_na.xml`. Refrain from registering them in `NekoConfig.java` since `NekoConfig.java` is not for adding new features; it does exist in Nagram since some features are based on Nekogram and NekoX.

Refrain from adding an explanation of the feature in `strings_na.xml`.

For example, in `strings_na.xml`, options are added:

> `<string name="DisableOption">Disable option</string>`

For example, in `NekoGeneralSettingsActivity.java`, options are registered:

> `private final AbstractConfigCell disableOptionRow = cellGroup.appendCell(new ConfigCellTextCheck(NaConfig.INSTANCE.getDisableOption()));`

For example, registering in `NaConfig.kt`:

**bool:**
```kotlin
val disableOption =
    addConfig(
        "DisableOption",
        ConfigItem.configTypeBool,
        false
    )

```

**int:**
```kotlin
val sideBarOption =
    addConfig(
        "SideBarOption",
        ConfigItem.configTypeInt,
        0
    )

```
