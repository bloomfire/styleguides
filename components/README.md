# Bloomfire Component Styleguide

* This is the component styleguide that we use at Bloomfire engineering. It
defines the best practices that we've come up with when developing components.
The goal is to keep the code normalized and upheld to an agreed upon standard,
with the author of the component being indistinguishable because we are all
following the same coding style.*

## Table of Contents

  1. [Naming](#naming)

## Naming

  - Components should follow a standardized naming structure. They should be
  placed in the `common` directory in a camelcased named folder. They should
  contain applicable `views`, `models`, `collections`, and `templates` folders
  Place the relevant files in that folder.

  ```
  example of file structure for a component.
  common
  |-comments
  | |-collections
  | | |-commentsCollection.js
  | |-models
  | | |-commentModel.js
  | | |-answerModel.js
  | |-views
  | | |-commentListView.js
  | | |-commentBlockView.js
  | | |-newCommentFormView.js
  ```

  - view, template, model, and collection files should camel cased.

  - view, model, and collection files should end with the applicable word `View`,
  `Model`, or `Collection`. Templates should end with file extension `.tpl`

  - view, model, and collection class definitions should be pascal case.

  **[⬆ back to top](#table-of-contents)**


