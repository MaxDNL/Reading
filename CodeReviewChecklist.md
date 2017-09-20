# UI Code Review Checklist
The following checklist is a checklist for UI code reviews. It is useful for developers to reference when doing a UI code review.

## ES6
* [ ] 100% passing unit test coverage.
* [ ] Passes all linting checks.
* [ ] All naming conventions are followed.
  * [ ] All Classes are Pascal case. 
  * [ ] All Class files are Pascal case.
  * [ ] All Class members are Camel case.
  * [ ] All directories are Camel case.
  * [ ] Correct member accessor conventions (_private, $protected, public)
* [ ] One Class per file.
* [ ] No circular dependencies.
* [ ] Favors destructors.
* [ ] No null reference exceptions.
* [ ] Code is correctly formatted.
* [ ] No redundant or extraneous code. (e.g. unnecessary or commented out)
* [ ] All expected error scenarios are covered and accounted for.
* [ ] All unexpected error scenarios are covered and accounted for. (e.g. downed services; malformed API responses; null, undefined, or empty values; incorrect types)

## React
* [ ] All ES6 review passing.
* [ ] No data direction reversals.

## CSS
* [ ] All naming conventions are followed.
* [ ] All selectors Kebob case.
* [ ] Only class and pseudo selectors. (No tag, id, name, etc.)
* [ ] Only one level of nesting.
* [ ] Code is correctly formatted.
* [ ] Adheres to correct units.
* [ ] No redundant or extraneous code. (e.g. unnecessary or commented out)

## HTML / JSX
* [ ] All naming conventions are followed.
* [ ] Code is correctly formatted. (e.g. attributes, nesting)

## Overall
Along with the specifics described above for the target language, the code should be:

* [ ] Readable
* [ ] Descriptive
* [ ] Comprihensible
* [ ] Unabiguous
* [ ] Maintainable
* [ ] Extensible