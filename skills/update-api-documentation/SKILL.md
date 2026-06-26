---
name: update-api-documentation
description: 'Batch update Dynamsoft JS/Mobile API documentation based on a list of new or modified interfaces, classes, methods, and properties. Use this skill when given a release diff or a structured list of API changes to apply across multiple doc files.'
argument-hint: 'Paste the full list of new/modified interfaces, classes, and methods. Optionally specify a version string (e.g. "CaptureVisionBundle 3.4.2000") to include in Remarks.'
user-invocable: true
---

# Update API Documentation

## When to Use
- A new SDK release introduces new interfaces, classes, methods, or properties.
- Existing interfaces gain new properties or methods.
- Method signatures, return types, or default values change.
- A new module-level class needs a new doc file.
- Release notes need to record the API changes.

## Inputs To Confirm
Before starting, ask the user to confirm or provide:
1. The **full list of new/modified interfaces/classes** (TypeScript signatures + JSDoc comments).
2. The **version string** for Remarks (e.g. `CaptureVisionBundle version 3.4.2000 & BarcodeReaderBundle version 11.4.2000`). If not provided, omit the Remarks block.
3. The **target doc repository** (e.g. `barcode-reader-docs-js`, `capture-vision-docs-js`, `label-recognition-docs-js`).
4. Whether **new files** need to be created, or only existing files updated.
5. Whether the **sidebar HTML** (`_includes/sidelist-programming/programming-js.html`) needs updating.
6. Whether **release notes** (`release-notes/js-11.md`, `release-notes/index.md`, or `dcvb-3.md`) need updating.

## Pre-Flight Checks
1. Read the relevant existing doc files before editing — never assume content.
2. For each new interface, search for an existing doc file by name pattern (`file_search`) to avoid duplication.
3. Check nearby sibling doc files to confirm the exact formatting conventions in use (front matter keys, `**Parameters**` vs `#### Parameters`, bullet list style `*` vs `-`, `**See also**` vs `**See Also**`).
4. For Liquid template variable links (`{{ site.xxx }}`), look up the correct variable by grepping 2–3 neighboring files in the same repo.

## Procedure

### A. New interface or class → Create a new doc file

1. Identify the correct directory from the target repo's folder structure.
2. Copy the front matter pattern from an existing sibling file in the same directory.  
   Key front matter fields: `layout`, `title`, `description`, `keywords`, `needAutoGenerateSidebar`, `needGenerateH3Content`, `noTitleIndex`, `breadcrumbText`.
3. Write the file in this order:
   - Front matter
   - `# ClassName` heading
   - One-sentence description (what it represents, what it extends)
   - TypeScript interface/class definition code block
   - Property/method table (columns: Name, Description)
   - One `## propertyOrMethod` section per member, each containing:
     - One-line description
     - TypeScript code block (signature)
     - **Parameters** block (if any)
     - **Return Value** block
     - **Remarks** block (if version info provided)
     - **See also** block (link to related types)
     - **Code snippet** block (if meaningful)

### B. Existing interface gains new property/method → Update existing doc file

1. Read the current file in full before editing.
2. Add the new member to:
   - The TypeScript definition code block
   - The property/method table (commented-out `<!-- ... -->` if that style is in use)
   - A new `## memberName` section at the appropriate position (keep alphabetical or logical grouping consistent with the file)
3. If a **Remarks** version string was provided, add it as a `**Remarks**` block inside the new section.
4. Link all type references using the same Liquid variable pattern (`{{ site.xxx }}`) found in the file.

### C. Method signature or default value changes → Update existing section

1. Locate the exact section by heading name.
2. Update only: the TypeScript code block, the Parameters descriptions, the Return Value description.
3. Add a **Remarks** note if the change is significant or version-specific.

### D. Sidebar HTML update

1. Open `_includes/sidelist-programming/programming-js.html`.
2. Locate the correct module block (find by `<!-- ModuleName -->` comment or by adjacent module entries).
3. Insert a new `<li>` block following the exact HTML indentation and `class="otherLinkColour"` pattern.
4. For a new top-level module, insert it between the two adjacent modules it logically belongs between.
5. For new classes within an existing module, add `<li>` entries inside that module's `<ul>`.

### E. Release notes update

For `release-notes/js-11.md` (or equivalent versioned file):
1. Add a new `## X.Y.Z (MM/DD/YYYY)` entry at the top, below the `# Release Notes` heading.
2. Use `### Highlights`, `### API Changes`, `#### New`, `#### Changed`, `#### Removed`, `### Fixed` sub-sections as needed.
3. Link every API name to its doc page using `{{ site.xxx }}` Liquid variables.
4. Match the bullet style (`-`) and dash-separated em-dash (`–`) conventions already in the file.
5. For strategic/breaking changes, add a `### ⚠️ Important Change: ...` section with a "What this means in practice" bullet list.

For `release-notes/index.md`:
1. Prepend the new version entry at the top of the list.
2. Anchor format: lowercase, no dots, no spaces — e.g. `#1142000-03242026`.

## Output Expectations
- Each file edit is minimal and scoped — no unrelated formatting changes.
- Existing content is never rephrased or restructured unless directly related to the change.
- All new content matches the style conventions (bold labels, list style, Liquid link format) of the surrounding file.
- A short confirmation listing: files created, files updated, and any sidebar/release-notes changes made.

## Style Conventions Reference

| Convention | Rule |
| ---------- | ---- |
| Section labels | `**Parameters**`, `**Return Value**`, `**Remarks**`, `**See also**`, `**Code snippet**` (bold, not headings) |
| List bullets | Match the file — most JS doc files use `-`; some use `*`. Check siblings. |
| TypeScript block language | ` ```typescript ` for signatures, ` ```js ` for code snippets |
| Liquid variable for DBR JS docs | `{{ site.js_api }}` or `{{ site.dbr_js_api }}` |
| Liquid variable for DCVB JS docs | `{{ site.dcvb_js_api }}` |
| Liquid variable for DLR JS docs | `{{ site.dlr_js_api }}` |
| Liquid variable for DCP JS docs | `{{ site.dcp_js_api }}` |
| Liquid variable for DCVB params | `{{ site.dcvb_parameters }}` |
| New file trailing newline | Always end files with a single newline |
| Remarks version example | `New added in CaptureVisionBundle version 3.4.2000.` |

## Notes
- When a new class is both a new file **and** a new sidebar entry, do both in the same batch.
- When the same property/method appears in multiple related interfaces (e.g. `BarcodeResultItem` and `DecodedBarcodeElement`), update all of them unless the user specifies otherwise.
- `noTitleIndex: true` in front matter suppresses the h1 from generating a TOC entry — this is intentional and is not a lint error.
- `MD036` lint warnings on `**bold labels**` are pre-existing patterns in these repos — do not convert them to headings.
