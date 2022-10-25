# Major release

This playbook covers our process for shipping a major release of Carbon. It
details the various stages, roles, and processes that we use to deliver the
release in manageable chunks. Our goal with every major release is to:

- Find the right balance between delivering meaningful changes while not making
  migration overly challenging or time-consuming
- Communicate clear roadmap and expectations to teams that depend on us, whether
  that is a product team looking to use a new feature or a PAL team that depends
  on us for their work
- Share our work early and often with teams who are using Carbon today and
  integrate that feedback before we release

# Overview

- We deliver features via releases
- Releases have stages for incremental delivery and feedback
- Features have acceptance criteria to clearly state intended outcomes
- Completeness is measured in two ways
  - Does the feature meet the _universal_ definition of done?
  - Does the feature achieve the _specific_ acceptance criteria?

# Roles and responsibilities

- Release manager
  - Updates the release table found in the planning issue
  - Works with feature owners in a release to determine delivery timeline
  - Make the go/no-go decision for a release
- Feature owner
  - Writes the acceptance criteria for a piece of functionality
  - Coordinates the effort to get the feature complete, stable, and documented

# **Release stages**

A release goes through many stages from early testing to general availability.
The stages are measured based on completeness (done done), stability (feature
API, strategy, methodology will not change prior to GA), and documentation
(usage and migration docs.)

- Alpha: features are early in development and not complete, software is highly
  unstable, little to no usage or migration documentation is available
- Beta: certain features may be complete or later on in their development cycle,
  software is somewhat unstable, usage and migration documentation is somewhat
  complete
- Release Candidate: features are complete or nearing completion, software is
  mostly stable, usage and migration documentation is mostly complete
- Generally Available: features are complete, software is stable, usage and
  migration documentation is complete but can be added to

All release stages leading up to GA provide a means for testing and feedback. As
Carbon is open-source, there's no need for private release stages.

To move from one stage to the next, every feature in that planned release stage
needs to, at the minimum, meet that stage's criteria of completeness, stability,
and documentation.

A release stage can get broken up into sub-releases (e.g. beta.1, beta.2) if the
team wants to package up and share their work prior to moving on to the next
stage.

Alpha, beta, and release candidate stages can state exceptions to the definition
of done, should it be advantageous to de-prioritize certain efforts during those
earlier stages.

# Release lifecycle

This process details the steps that go into delivering a release to a particular
release stage.

1. Determine the release theme
   - What does the team aim to deliver by the end of this release?
   - What kind of feedback are we looking for?
2. Break down the release theme into feature sets
3. Write out the scope and acceptance criteria of a feature set
4. Deliver against the feature set in meaningful chunks
5. Review and test the feature set
6. Release the set of features for the release
7. Communicate the release through various mediums (Slack, Medium, website,
   release partners, YourLearning events, etc.)

# Definition of done

Applicable to every bug we fix and feature we deliver:

- [ ] The code for the feature has been reviewed and merged into the codebase
- [ ] The code for the feature is either available in stable or behind a feature
      flag
- [ ] The feature owner has reviewed and accepted all changes
- [ ] Usage documentation has been written
- [ ] If applicable, documentation has been written for migrating from the
      currently available feature to the feature being delivered
  - [ ] Storybook MDX
  - [ ] Storybook stories
    - [ ] Default usage
    - [ ] Controlled usage
    - [ ] Playground
- [ ] If applicable, documentation has been written and is available on the
      Carbon website
  - [ ] Usage
  - [ ] Style
  - [ ] Code
  - [ ] Accessibility
- [ ] TODO kit parity (prioritize a main kit)
- [ ] TODO style and usage sync

Items to consider:

- Reviewed by design, content, and development
- Public API is documented
- Feature parity with Sketch Kit
- Accessibility compliance and documentation
- Internationalization support
- Browser support
- Test coverage (unit, VRT, etc.)
- Works on all common web viewport sizes (e.g. mobile web)
- If applicable, migration tooling and documentation on how to use it

NOT in our definition of done:

- Feature parity in for non-React implementations and non-Sketch design kits
- IE11 for v11 and later releases
