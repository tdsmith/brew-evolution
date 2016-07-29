# Install bottles of arbitrary versions

## Introduction
Install bottles without having access to the formula used to create them.

## Motivation
The motivation for this feature is to install a collection of bottles of arbitrary versions to repeat a data analysis pipeline. It is one step toward the loftier goal of being able to recreate the environment of a previous installation of Homebrew. See the original feature request and discussion at [Homebrew/homebrew-science#1191](https://github.com/Homebrew/homebrew-science/issues/1191).

Pouring the bottle (extracting the tarball) is easy enough. Neither the bottle's dependencies nor its `post_install` are currently stored in the bottle, and the information in the current formula may have changed since the bottle was built. It would be a nice feature of the bottle file format if the bottle itself contained all of the necessary information to install the bottle.

## Proposed solution
Store the formula used to build the bottle inside the bottle. It would be necessary to remove the `bottle` section or the `sha256` lines, as it wouldn't be possible to store the `sha256` of the bottle inside the bottle, lest the universe implode.

## Detailed design
When installing a keg from source, store the names of the dependencies and the versions used to build the keg inside `INSTALL_RECEIPT.json`. When building a bottle from that keg, store the formula used to build the keg inside the bottle in a file named `NAME/VERSION/.bottle/NAME.rb` after removing the bottle block or `sha256` lines.

## Alternatives considered
1. Store the git SHA1 of the revision of the formula used to build the bottle in the bottle so that the formula can be later extracted with `git show`.
2. Store the `git hash-object` SHA1 of the formula used to build the bottle in the bottle so that the formula can later be extracted with `git cat-file blob`.
3. Find the formula in the `git log` as `brew versions` did.
