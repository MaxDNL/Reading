# taylor-ui-kernel
taylor-ui-kernel is a foundational library for Taylor UI. It contains low-level general capabilities that are intended to be leveraged cross platform.

## Table Of Contents

* [Overview](#user-content-overview)
* [Development](#user-content-development)

## Overview

### Directory Structure
```
.
├── Assert.js
├── dom
│   └── Event.js
├── finance
│   └── Money.js
├── index.js
├── math
│   └── Arithmetic.js
└── patterns
    └── specification
        ├── AbstractSpecification.js
        ├── AndSpecification.js
        ├── FalseSpecification.js
        ├── NotSpecification.js
        ├── OrSpecification.js
        ├── TrueSpecification.js
        └── XorSpecification.js

```

## Conventions and Patterns

### Conventions
A few conventions to be aware of and adhere to aid in reasoning about systems that are modeled in an OO methodology where the language (JavaScript) does not natively support expected OO constructs.

#### Access Modifiers 
Because JavaScript exposes all methods and properties implicitly as public, taylor-ui-kernel expresses member accessibility intentions by convention:

* **$**: Prepending member names with a "$" indicate a "Protected" member. Protected members should be accessed only by the defining class and inheriting classes. 

* **\_**: Prepending member names with a "\_" indicate a "Private" member. Private members should be accessed only by the defining class.

### Patterns
Common patterns are implied in this project. Links for more information on these patterns:

* [Design Patterns](https://books.google.com/books/about/Design_Patterns.html?id=6oHuKQe3TjQC&printsec=frontcover&source=kp_read_button&hl=en#v=onepage&q&f=false)

### Idioms
This project leverages ES6 idioms transpiled by Babel:

* [ES6](http://es6-features.org/#Constants)

## Development
taylor-ui-kernel promotes modular, test-driven development. Before development it is important for a developer to fully understand requirements and how those requirements can be defined by a suite of tests. To ensure maximal code stability and quality it is suggested to _**not**_ begin development where requirements cannot be so defined.

### Tests
Tests are found in `./test`

#### Unit Tests
Unit tests are intended to cover individual units of functionality at an arbitrarily minimal aggregate. A common and natural arbitrary aggregate is a class. It is likely that a given project will have one test file per class file. For example, a class `./src/ExampleClass.js` will have a corresponding Unit Test `./test/ExampleClass.test.js`. The intention of a unit test is to confirm expected outcomes for all possible scenarios.

##### Unit Test Development
taylor-ui-kernel leverages [Mocha](https://mochajs.org/) for Unit Testing.

#### Test Coverage
taylor-ui-kernel should always have 100% test coverage. This can be confirmed by running `npm run test-cov`.

### Building 
taylor-ui-kernel is intended to be built before distribution. The project can be built by rinning `npm run build`.

## Commands
taylor-ui-kernel is managed under NPM. There a numerous usefil npm commands that this project exposes.

* `npm test` runs all unit tests.

* `npm run test-file` runs only those tests in the file specified.

* `npm run test-cov` runs all unit tests and test coverage.

* `npm run test-cov-d` opens Chrome to display a navigable test hierarchy useful for understanding which lines of code are not covered. _(Mac only. Assumes Chrome installed)_

* `npm run test-cov-d-win` opens Chrome to display a navigable test hierarchy useful for understanding which lines of code are not covered. _(Windows only. Assumes Chrome installed and chrome set up in PATH)_

* `npm run eslint` runs eslint on entire project.

* `npm run check` runs all unit tests, test coverage, and linting. This is useful for code reviews and pre-PR.

* `npm run build` builds the distribution package.
