# Release Checklist

<!-- This is an example of an issue used for tracking releases for libraries in a design system -->

**Status:** Unreleased | Prerelease | Stable
**Preview date:** TODO
**Target date:** TODO
**Changelog:**  TODO # Fill out after publishing a version

### Timeline

- Monday
  - Publish release candidate
- Tuesday
- Wednesday
  - Promote to stable
- Thursday

### Release checklist

- [ ] Determine scope of changed items (and dependencies)
- [ ] Verify new behavior
- [ ] Verify no regressions have occurred
- [ ] Create release candidate and PR against internal properties
- [ ] Create stable release and publish to next
- [ ] Push git tag and create changelog
- [ ] Switch next tag to latest for changed packages

**Browser support**

- [ ] Chrome
- [ ] Safari
- [ ] Firefox
- [ ] Edge
- [ ] IE11

**Screen reader support**

- [ ] JAWS
- [ ] NVDA
- [ ] VoiceOver on macOS
- [ ] VoiceOver on iOS

### Smoke tests

<!-- This are often sponsor or internal teams that we lean on for smoke testing the release -->

- [ ] Team A
- [ ] Team B
- [ ] Team C
