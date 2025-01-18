Okay, this section provides a detailed look into how Tolk handles its standard library (stdlib) in comparison to FunC. Here's a breakdown of the key information:

**FunC's Standard Library:**

*   **Single File:** FunC's stdlib is traditionally located in a single file, often named `stdlib.fc`.
*   **Low-Level Focus:** It contains low-level functions closely mirroring TVM instructions.
*   **Direct Mapping to TVM:** Function names tend to be very similar to the underlying TVM commands.

**Tolk's Standard Library:**

*   **Modularized Files:** Tolk's stdlib is broken down into multiple files, such as `common.tolk`, `tvm-dicts.tolk`, and others, which promotes better organization and manageability.
*   **Selective Imports:** Functions in `common.tolk` are available without any explicit import statement. However, functions in other files require explicit import statements using the `import "@stdlib/..."` syntax.
*   **Embedded Distribution:** The stdlib is bundled directly with the Tolk compiler, eliminating the need to download it separately from GitHub, thereby simplifying the setup and reducing external dependencies.
*   **Verbose Function Names:** Almost all FunC functions have been renamed to more descriptive, `camelCase` names, improving code readability and making it easier to understand the function's purpose.
*   **Some Functions Removed:** Certain rarely used or redundant FunC functions have been removed from the Tolk stdlib. These are often functions that are expressed more naturally in Tolk's syntax or have limited use cases.
*   **New Functions Added:** Tolk's stdlib introduces some new, commonly used functions that were absent in FunC, expanding the capabilities of the language.
*   **Added Constants:** Tolk stdlib also introduces constants such as `SEND_MODE_*` and `RESERVE_MODE_*`.
*   **Mutability Changes:** Many FunC functions that operated on data via the tilda (`~`) now directly modify their objects, instead of creating copies. This includes operations on cells, slices, and dictionaries.

**Key Differences Explained:**

*   **Modularity:** The splitting of the stdlib into multiple files enhances the organization and makes it more manageable. It also reduces code bloat.
*   **Readability:** The renaming of functions to more descriptive names greatly improves code clarity and reduces the need to constantly reference documentation.
*   **Accessibility:** The embedding of the stdlib within the compiler eliminates the need for external downloads, making it more convenient to set up and use the toolchain.
*   **Mutating vs. Non-Mutating:** The change from FunC's tilda operator (~), which implied copy on modify, to direct mutations impacts how developers handle state in Tolk.

**Specific Examples:**

*   `beginCell()` is available without import, as it's in `common.tolk`.
*   `createEmptyDict()` is in the `tvm-dicts` and requires `import "@stdlib/tvm-dicts"`.
*   `cs~load_uint(32)` in FunC becomes `var flags = cs.loadUint(32)` in Tolk, directly modifying the slice `cs`.
*   `dict~udict_set(...)` in FunC becomes `dict.uDictSet(...)` in Tolk, modifying the dictionary `dict`.

**Notable Changes & Implications:**

*   **"import what you use" Philosophy:** Tolk encourages developers to explicitly import only the necessary modules to reduce namespace pollution and improve clarity.
*   **Easier to Grasp:** The renaming of standard functions to more readable names makes it easier for developers to understand the underlying logic and intent of the code, especially for newcomers to TON development.
*   **Performance Implications of Mutation:** Developers need to be conscious of mutability when working with slices, builders, and dictionaries, as changes directly modify objects instead of creating copies. This approach can save on unnecessary memory allocations and be more efficient if done correctly, but also increases the need for extra caution.
*   **Tolk stdlib in the future:** The Tolk stdlib is still actively being developed and will likely undergo more changes as the language matures.

**Embedded Stdlib Implementation Details:**

*   **Compiler Location:** The Tolk compiler automatically locates the `stdlib` folder using predefined paths relative to its executable or through the `TOLK_STDLIB` environment variable. This ensures the stdlib is always available during compilation.
*   **WASM Wrapper:** The `tolk-js` WASM wrapper also includes the standard library, making it accessible for web-based environments or for projects using tools like blueprint.
*   **IDE Plugin Support:** Both JetBrains and VS Code plugins also auto-locate the stdlib folder to provide auto-completion and other IDE support features.

**In Summary:**

Tolk's redesigned standard library is a significant improvement over FunC's, offering better organization, readability, and ease of use. The embedding of the stdlib and use of more descriptive function names, combined with new functions and a more streamlined approach to mutability significantly enhances the developer experience. This makes Tolk more approachable while remaining powerful enough to produce highly optimized and efficient smart contracts for the TON blockchain.
