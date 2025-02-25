## Unreleased

### Feat

- relocates OmniFocus tag and JIRA query to preferences section
- removes functionality for creating tasks based on comments

### Fix

- sets the max records per page to 1000
- reads preferences at beginning of execution
- declares missing alertMessage
- fixes typo with console message

## [1.4.0](https://github.com/PowerSchill/jira-omnifocus/compare/v1.3.0...v1.4.0) (2025-02-25)


### Features

* moves URL and credentials to a preference section ([d2f877c](https://github.com/PowerSchill/jira-omnifocus/commit/d2f877cdd2efc9ed002ece639035d8ffbcda7732))
* relocates OmniFocus tag and JIRA query to preferences section ([9685a32](https://github.com/PowerSchill/jira-omnifocus/commit/9685a322dd97767fe262bc6aa3d0f90c94c1ce04))
* removes functionality for creating tasks based on comments ([69b46a6](https://github.com/PowerSchill/jira-omnifocus/commit/69b46a6391e9839dcd5c060fd935caf4655c2b11))
* stores JIRA credentials in secured keychain ([c335042](https://github.com/PowerSchill/jira-omnifocus/commit/c33504280d140816a5368cfc23fc8927c2953fe3)), closes [#1](https://github.com/PowerSchill/jira-omnifocus/issues/1)


### Bug Fixes

* declares missing alertMessage ([664cd4d](https://github.com/PowerSchill/jira-omnifocus/commit/664cd4d9bfc4f2dfabb45340cf6d39f6ac7163cf))
* fixes typo with console message ([4ff5efb](https://github.com/PowerSchill/jira-omnifocus/commit/4ff5efb47a42fbebbd631a50cd4dca4b40f90f8d))
* reads preferences at beginning of execution ([94274c6](https://github.com/PowerSchill/jira-omnifocus/commit/94274c6e0982c36ac2b4d05bfea2951ec26f37b1))
* sets the max records per page to 1000 ([460ed36](https://github.com/PowerSchill/jira-omnifocus/commit/460ed36a0e35422898d469bee4c6489dea07b718))

## v2.0.0-alpha.0 (2024-04-27)

### Feat

- moves URL and credentials to a preference section

### Refactor

- **commitizen**: removes major-version-zero option
