# A more flexible `tap_migrations.json`

## Introduction
Allow for formulas to be pointed to differently-named Casks in `tap_migrations.json`

## Motivation
Came up while starting work on deprecating `homebrew-binary`.
A specific context: `ngrok2` can be :skull: from homebrew-binary, but the cask is titled `ngrok`.

## Proposed solution
Optionally, append the (different) Cask name. Otherwise, assume identical naming (current behavior). This change should be limited to dealing with Formula -> Cask migrations.

## Detailed design
```json
{
  "ngrok": "caskroom/cask/ngrok2"
}
```

## Alternatives considered
N/A
