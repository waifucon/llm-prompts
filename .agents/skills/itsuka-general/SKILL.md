You are following the General Development Rules for Telegram-based Android clients.

## Core Rules for All Projects

1. **Locating Symbols:**
   - Always use `rg` (ripgrep) to search for symbols and locate references efficiently across the codebase.

2. **Clean Imports:**
   - Always import classes explicitly instead of using inline, fully-qualified package names across the codebase in Java and Kotlin.

3. **Compilation & Verification:**
   - Do not compile automatically on your own.
   - After completing code modifications, ask the user if they would like to compile with `./gradlew assembleDebug` and verify if the changes work properly.
   - If the user agrees to compile, install the APK and launch the app on the device afterwards (refer to [`itsuka-adb`](../itsuka-adb/SKILL.md)).

4. **Strict Conditional Logic (Preserve Existing `if` Checks):**
   - Do not convert existing sequential `if` checks into `if / else` blocks. Maintain existing control flow patterns to prevent unintended side effects.
   - **Correct:**
     ```java
     if (conditionA) {
         actionA();
     }
     if (conditionB && !conditionA) {
         actionB();
     }
     ```
   - **Incorrect:**
     ```java
     if (conditionA) {
         actionA();
     } else {
         if (conditionB) {
             actionB();
         }
     }
     ```

5. **Protected Files:**
   - Do not revert changes in:
     - `TMessagesProj/release.keystore`
     - `TMessagesProj/jni/jni.c`

6. **Pull Requests & GitHub CLI (`gh`):**
   - Automatically open a PR after pushing when the user asks to commit the changes and push.
   - Refer to [`itsuka-github`](../itsuka-github/SKILL.md) when creating or managing pull requests with `gh`.
