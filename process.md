# Homebrew Evolution Process
Homebrew is a package manager for OS X. Homebrew is evolving, guided by a
community-driven process referred to as the Homebrew evolution process. This
document outlines the Homebrew evolution process and how a feature grows from a
rough idea into something that can improve the Homebrew package management
experience for our users.

## Scope
The Homebrew evolution process covers major changes to the Homebrew package
manager and the public interface of the Formula DSL. Smaller changes, such as
bug fixes, optimisations or diagnostic improvements can be contributed via the
normal contribution process; see
 [Contributing to Homebrew](https://github.com/Homebrew/brew/blob/master/CONTRIBUTING.md).

## Goals
The Homebrew evolution process aims to leverage the collective ideas, insights
and experience of the Homebrew community to improve the Homebrew user
experience. Its two primary goals are:

* Engage the wider Homebrew community in the ongoing evolution of Homebrew
* Maintain the vision and conceptual coherence of Homebrew

There is a natural tension between these two goals. Open evolution processes
are, by nature, chaotic. Yet, maintaining a coherent vision for something as
complicated as a package manager requires some level of coordination. The
Homebrew evolution process aims to strike a balance that best serves the
Homebrew community as a whole.

## Participation
Everyone is welcome to propose, discuss and review ideas to improve the
Homebrew language and standard library on this repository.

The Homebrew [maintainers](https://github.com/homebrew/brew#who-are-you) are
responsible for the direction of Homebrew. Maintainers initiate, participate in
and manage the public review of proposals and have the authority to accept or
reject changes to Homebrew.

## What goes into a review?
The goal of the review process is to improve the proposal under review through
constructive criticism and, eventually, determine the direction of Homebrew.
When writing your review, here are some questions you might want to answer in
your review:

* What is your evaluation of the proposal?
* Is the problem being addressed significant enough to warrant a change to Homebrew?
* Does this proposal fit well with the feel and direction of Homebrew?
* If you have used other package managers with a similar feature, how do you feel that this proposal compares to those?
* How much effort did you put into your review? A glance, a quick reading or an in-depth study?

Please state explicitly whether you believe that the proposal should be
accepted into Homebrew.

## How to propose a change
* **Check prior proposals**: Many ideas come up frequently and may either be in active discussion on the mailing list or may have been discussed already and have joined the [Commonly Rejected Proposals](commonly_rejected_proposals.md) list. Please check this list for context before proposing something new.
* **Develop the proposal**: Expand the rough sketch into a complete proposal, using the [proposal template](proposal_template.md).
* **Request a review**: Initiate a pull request to the [Homebrew Evolution repository](https://github.com/Homebrew/brew-evolution) to indicate to the maintainers that you would like the proposal to be reviewed.
* **Address feedback**: In general, and especially during the review period, be responsive to questions and feedback about the proposal.

If your proposal is merged that means the Homebrew maintainers have committed
to either actively working on it or accepting pull requests (with a mutually
agreed good implementation) to implement this change.
