# Mkdir create intermediate directories
## Introduction
As suggested [in this PR](https://github.com/Homebrew/homebrew-core/pull/4976#issuecomment-248258491)
this change would change the patched/chained `mkdir` (including with block) to used `mkdir_p` instead.

## Motivation
When using `mkdir` with block at present, a failure naturally occurs when intermediaries don't exist.

E.g. `mkdir "foo/bar/baz" { puts "uh oh" } 

## Proposed solution
In the patched `mkdir` with block, use `mkdir_p` instead of `mkdir`.

## Detailed design
Alter `/Library/Homebrew/test/test_pathname.rb` with trivial change mentioned above

## Alternatives considered
Could add `mkdir_p` with block, but @mikemcquaid suggested the above.
