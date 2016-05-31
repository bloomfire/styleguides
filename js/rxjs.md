# Bloomfire RxJS Style Guide

*Very much a work in progress*

## Table of Contents

  1. [Observable Naming](#observables)

## observables 

  - Files containing observables should be named in camelCase.
  - Observable classes should be capitalized and MixedCase.
  - Observable classes should substitute a trailing $ for the word "observable."
  - Observable instances should be camelCase$ with a trailing $.

  ```jsx
  import SomeAPICall$ from 'app/observables/someAPICallObservable';
  ````

  ```jsx
  const someAPICall$ = app.getInstance(SomeAPICall$);
  ```

**[⬆ back to top](#table-of-contents)**
