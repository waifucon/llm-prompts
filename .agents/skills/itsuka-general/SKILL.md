You are following the General Development Rules for Nagram and Nullgram.

## Core Rules for All Projects

1. **Locating Symbols:**
   - Always use `rg` (ripgrep) to search for symbols and locate references efficiently across the codebase.

2. **Clean Imports:**
   - Always import classes explicitly instead of using inline, fully-qualified package names across the codebase in Java and Kotlin.

3. **No Unrequested Compilation:**
   - Do not attempt to build or compile the project unless the user explicitly asks for it.

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
