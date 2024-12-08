# String.prototype.count Proposal

## Status

[Stage 0](https://tc39.github.io/process-document/) - Strawman

## Introduction

This proposal seeks to introduce a `count()` method to the `String.prototype` which counts the number of non-overlapping occurrences of a substring within a string. This method is analogous to Python's `str.count()` method.

## Motivation

Counting occurrences of a substring is a common operation in string processing. Currently, there is no built-in method in ECMAScript to do this. Developers often resort to workarounds using regular expressions or split/join operations, which can be less readable and less efficient. The addition of `String.prototype.count` would provide a straightforward way to perform this common task, and would align with functionality provided in other popular languages:

- **[Python](https://docs.python.org/3/library/stdtypes.html#str.count)**: `str.count(sub[, start[, end]])` is widely used and highly intuitive.
- **[PHP](https://www.php.net/manual/en/function.substr-count.php)**: The `substr_count()` function is similar to Python's `count()` method. 
- **[Ruby](https://ruby-doc.org/core-1.9.3/String.html#method-i-count)**: `String#count` works similarly but also supports character ranges.

## Method Signature

String.prototype.count(searchString[, start[, end]])

## Parameters

- **searchString** *(Required)*: A string that represents the substring to search for within the calling string.
- **start** *(Optional)*: An integer value that specifies the zero-based index at which the search should begin. Defaults to `0`.
- **end** *(Optional)*: An integer value that specifies the zero-based index at which the search should end. Defaults to the length of the string.





## Create your proposal repo

Follow these steps:
  1. Click the green [“use this template”](https://github.com/tc39/template-for-proposals/generate) button in the repo header. (Note: Do not fork this repo in GitHub's web interface, as that will later prevent transfer into the TC39 organization)
  1. Update ecmarkup and the biblio to the latest version: `npm install --save-dev ecmarkup@latest && npm install --save-dev --save-exact @tc39/ecma262-biblio@latest`.
  1. Go to your repo settings page:
      1. Under “General”, under “Features”, ensure “Issues” is checked, and disable “Wiki”, and “Projects” (unless you intend to use Projects)
      1. Under “Pull Requests”, check “Always suggest updating pull request branches” and “automatically delete head branches”
      1. Under the “Pages” section on the left sidebar, and set the source to “deploy from a branch”, select “gh-pages” in the branch dropdown, and then ensure that “Enforce HTTPS” is checked.
      1. Under the “Actions” section on the left sidebar, under “General”, select “Read and write permissions” under “Workflow permissions” and click “Save”
  1. [“How to write a good explainer”][explainer] explains how to make a good first impression.

      > Each TC39 proposal should have a `README.md` file which explains the purpose
      > of the proposal and its shape at a high level.
      >
      > ...
      >
      > The rest of this page can be used as a template ...

      Your explainer can point readers to the `index.html` generated from `spec.emu`
      via markdown like

      ```markdown
      You can browse the [ecmarkup output](https://ACCOUNT.github.io/PROJECT/)
      or browse the [source](https://github.com/ACCOUNT/PROJECT/blob/HEAD/spec.emu).
      ```

      where *ACCOUNT* and *PROJECT* are the first two path elements in your project's Github URL.
      For example, for github.com/**tc39**/**template-for-proposals**, *ACCOUNT* is “tc39”
      and *PROJECT* is “template-for-proposals”.


## Maintain your proposal repo

  1. Make your changes to `spec.emu` (ecmarkup uses HTML syntax, but is not HTML, so I strongly suggest not naming it “.html”)
  1. Any commit that makes meaningful changes to the spec, should run `npm run build` to verify that the build will succeed and the output looks as expected.
  1. Whenever you update `ecmarkup`, run `npm run build` to verify that the build will succeed and the output looks as expected.

  [explainer]: https://github.com/tc39/how-we-work/blob/HEAD/explainer.md
