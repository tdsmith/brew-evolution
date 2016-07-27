# Install bottles of arbitrary versions

## Introduction
I'd like to be able to install bottles of arbitrary versions of a formula.

## Motivation
The motivation for this feature is to install specified versions of software to repeat a data analysis pipeline, an oft-requested feature of Homebrew-Science, which could be happily stored in a Brewfile. See Homebrew/homebrew-science#1191

## Proposed solution
Pouring the bottle (extracting the tarball) is easy enough. The bottle's dependencies and its `post_install` are the needed missing info.

One simple way of storing this info is to store the Ruby formula that generated the bottle alongside the bottle. To that end, when uploading bottles to Bintray, I'd also like to upload the Ruby formula that generated the bottle.

That Ruby formula could alternatively be stored inside the bottle, with the caveat that it would be necessary to remove the bottle `sha256`, as it wouldn't be possible to store the `sha256` of the bottle inside the bottle, lest the universe implode. As a bonus, it would be a nice feature of the bottle file format if the bottle itself contained all of the necessary information to install the bottle.

## Detailed design
When building a bottle, store the Ruby formula inside the bottle in a file named `NAME/VERSION/NAME.rb` after removing the bottle `sha256` lines.

## Alternatives considered
1. Store the Ruby formula inside the bottle (without the bottle `sha256` lines)
2. Store the Ruby formula alongside the bottle on Bintray
