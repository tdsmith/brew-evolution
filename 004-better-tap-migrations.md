# A more flexible `tap_migrations.json`

## Introduction
Allow for formulas to be pointed to differently-named formula/casks in `tap_migrations.json`

## Motivation
Came up while starting work on deprecating `homebrew-binary`. Would also be generally useful to deal with formula renames.
A specific context: ngrok2 can be :skull: from homebrew-binary, but the cask is titled ngrok.

## Proposed solution
Optionally, append the (different) Formula/Cask name. Otherwise, assume identical (current behavior)

## Detailed design
```json
{
  "ngrok": "caskroom/cask/ngrok2"
}
```

## Alternatives considered
N/A
