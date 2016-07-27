# Install bottles of arbitrary versions

## Introduction
Install bottles without having access to the formula used to create them.

## Motivation
The motivation for this feature is to install a collection of bottles of arbitrary versions to repeat a data analysis pipeline. See [Homebrew/homebrew-science#1191](https://github.com/Homebrew/homebrew-science/issues/1191).

## Proposed solution
Pouring the bottle (extracting the tarball) is easy enough. Neither the bottle's dependencies nor its `post_install` are currently stored in the bottle, and the information in the current formula may have changed since the bottle was built. It would be a nice feature of the bottle file format if the bottle itself contained all of the necessary information to install the bottle.

The formula use to build the bottle could be stored inside the bottle. It would be necessary to remove the `bottle` section or the `sha256` lines, as it wouldn't be possible to store the `sha256` of the bottle inside the bottle, lest the universe implode.

## Detailed design
When building a bottle, store the formula inside the bottle in a file named `NAME/VERSION/NAME.rb` after removing the bottle block or `sha256` lines.

## Alternatives considered
Find the formula in the `git log` as `git versions` did.
