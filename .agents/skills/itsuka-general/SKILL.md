You are following the General Development Rules for Telegram-based Android clients.

## Core Rules for All Projects

1. **Locating Symbols:**
   - Always use `rg` (ripgrep) to search for symbols and locate references efficiently across the codebase.

2. **Clean Imports:**
   - Always import classes explicitly instead of using inline, fully-qualified package names across the codebase in Java and Kotlin.

3. **Compilation & Verification:**
   - Do not compile automatically on your own.
   - After completing code modifications, ask the user if they would like to compile with `./gradlew assembleDebug` (prefer `./gradlew :TMessagesProj:assembleLiteArm64Debug` for exteraGram) and verify if the changes work properly.
   - If the user agrees to compile, install the APK and launch the app on the device afterwards (refer to [`itsuka-adb`](../itsuka-adb/SKILL.md)).

4. **Conditional Logic in Upstream Telegram Classes:**
   - This rule applies **only** to official upstream Telegram Android classes.
   - In upstream Telegram classes, do not convert existing sequential `if` checks into `if / else` blocks. Maintain existing control flow patterns to prevent merge conflicts and unintended side effects during upstream merges/updates.
     - **Example:**
       ```java
       // Preserve existing control flow
       if (conditionA) {
           actionA();
       }
       if (conditionB && !conditionA) {
           actionB();
       }
       ```

5. **Protected Files:**
   - Do not revert changes in:
     - `TMessagesProj/release.keystore`
     - `TMessagesProj/jni/jni.c`

6. **Pull Requests & GitHub CLI (`gh`):**
   - Automatically open a PR after pushing when the user asks to commit the changes and push.
   - Always use `--force` when force pushing.
   - Refer to [`itsuka-github`](../itsuka-github/SKILL.md) when creating or managing pull requests with `gh`.

7. **String Management & Localization:**
   - Refer to [`itsuka-strings`](../itsuka-strings/SKILL.md) for string management, resource conventions, and localization workflows across all projects.
