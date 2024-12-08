# `String.prototype.count` Proposal

## Status

- Stage: [0 - Strawman](https://tc39.github.io/process-document/)
- Author: Faraz Kelhini
- Champion(s): TBD 

## Introduction

This proposal seeks to introduce a `count()` method to the `String.prototype` which counts the number of non-overlapping occurrences of a substring within a string. This method is analogous to Python's `str.count()` method.

## Motivation

Counting occurrences of a substring is a common operation in string processing. Currently, there is no built-in method in ECMAScript to do this. Developers often resort to workarounds using regular expressions or split/join operations, which can be less readable and less efficient. The addition of `String.prototype.count` would provide a straightforward way to perform this common task, and would align with functionality provided in other popular languages:

- **[Python](https://docs.python.org/3/library/stdtypes.html#str.count)**: `str.count(sub[, start[, end]])` is widely used and highly intuitive.
- **[PHP](https://www.php.net/manual/en/function.substr-count.php)**: The `substr_count()` function is similar to Python's `count()` method. 
- **[Ruby](https://ruby-doc.org/core-1.9.3/String.html#method-i-count)**: `String#count` works similarly but also supports character ranges.

## Method Signature

```javascript
String.prototype.count(searchString[, start[, end]])
```

## Parameters

- **searchString** *(Required)*: A string that represents the substring to search for within the calling string.
- **start** *(Optional)*: An integer value that specifies the zero-based index at which the search should begin. Defaults to `0`.
- **end** *(Optional)*: An integer value that specifies the zero-based index at which the search should end. Defaults to the length of the string.

## Implementation

A JavaScript implementation of the proposal.

```javascript
if (!String.prototype.count) {
  String.prototype.count = function (searchString, start, end) {
    if (this == null) {
      throw new TypeError("String.prototype.count called on null or undefined");
    }

    const string = String(this);
    const search = String(searchString);
    const startIndex = Math.max(0, Math.min(string.length, start ? Number(start) || 0 : 0));
    const endIndex = Math.max(startIndex, Math.min(string.length, end ? Number(end) || string.length : string.length));

    if (!search) return 0; 

    const substring = string.slice(startIndex, endIndex);
    let count = 0;
    let position = 0;

    while ((position = substring.indexOf(search, position)) !== -1) {
      count++;
      position += search.length;
    }

    return count;
  };
}
```

## Example Usage

```javascript
const str = "hello world, hello universe";

console.log(str.count("hello"));       // 2
console.log(str.count("o"));           // 3
console.log(str.count("hello", 13));   // 1
console.log(str.count("hello", 0, 5)); // 1
console.log(str.count("notfound"));    // 0
console.log(str.count(""));            // 0
```

## Proposed Specification

1. Let _O_ be ? RequireObjectCoercible(*this* value).
2. Let _S_ be ? ToString(_O_).
3. Let _searchString_ be ? ToString(_searchString_).
4. If _start_ is undefined, let _startIndex_ be 0.
5. Else, let _startIndex_ be ? ToIntegerOrInfinity(_start_).
6. If _end_ is not present, let _endIndex_ be the length of _S_.
7. Else, let _endIndex_ be ? ToIntegerOrInfinity(_end_).
8. Let _startIndex_ be min(max(_startIndex_, 0), the length of _S_).
9. Let _endIndex_ be min(max(_endIndex_, 0), the length of _S_).
10. If _startIndex_ >= _endIndex_, return 0.
11. Let _subString_ be the substring of _S_ from _startIndex_ to _endIndex_.
12. If _searchString_ is an empty string, return 0.
13. Let _count_ be 0.
14. Let _position_ be 0.
15. Repeat, while _position_ is not -1:
    1. Let _position_ be the result of searching for _searchString_ in _subString_ starting at index _position_.
    2. If _position_ is not -1:
        1. Increment _count_ by 1.
        2. Set _position_ to _position_ + the length of _searchString_.
16. Return _count_.

Note: This method is intentionally generic; it does not require that its this value be a String object.
Therefore, it can be transferred to other kinds of objects for use as a method.

## Behavior

- It is case-sensitive, consistent with other string methods like `includes()`.
- Ensures that the method is called on a value that can be converted to an object (e.g., strings, numbers, or booleans). Throws a `TypeError` if the method is called on `null` or `undefined`.
- If `start` is not provided, it defaults to `0`. If provided, it is converted to an integer. The final value is clamped to be within `[0, length of string]` to ensure it is valid.
- If `end` is not provided, it defaults to the length of the string. If provided, it is converted to an integer. The final value is clamped to be within `[0, length of string]` to ensure it is valid.
- If `startIndex` is greater than or equal to `endIndex`, there is no range to search within, so the method immediately returns `0`.
- If `searchString` is an empty string, returns `0`. This avoids potential infinite loops when counting empty substrings.

## Backward Compatibility

Adding this method will not break existing code, as it adds new functionality to the `String` prototype without modifying existing methods.

## Q&A

Q: Why not add an option to toggle case sensitivity?

A: To maintain consistency with other string methods like `includes()`.
