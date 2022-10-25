# Release

## Roles

**Release team**

The release team is made up of a release lead and a side-kick. The goal of the team is to effectively ship versions of the Carbon Design System every month. This includes:

- Determining what is eligible to go into a specific release
- Coordinating the release of the package on `npm`
- Coordinating the release of the package under the releases tab of GitHub with an appropriate changelog
- Verifying each release candidate with a sampling of stakeholders
- Addressing any release-related issues, including:
    - Accidental breaking changes
    - General issues with a publish step

**Release lead**

This individual leads the release for the given month. They are responsible for:

- Training the side-kick so that they can run the next release
- Being the point person for any release-related questions or support tickets

**Release side-kick**

This individual is involved in the release process and assists the release lead and learns how to run a release. They are responsible for:

- Coordinating with the release lead on what needs to get done and helping out where appropriate
- Learning the release process such that they are confident enough to follow it in the next cycle
- Fallback support for release-related questions or support tickets if the release lead is unavailable or is occupied by other issues

### Conventions

#### Rollback

Sometimes there's a problem with a specific release that got set as latest. For example, feedback might come from product teams that this release breaks some live deployment process. In this case, you will need to rollback the latest release of the affected packages. To do this, you will use `npm dist-tag` in the following way:

```bash
# Current package: carbon-components
# Latest version: 10.16.0
# Previous version: 10.15.0

# Rollback from latest to previous (10.16.0 -> 10.15.0)
npm dist-tag add carbon-components@10.15.0 latest
```

After you rollback the impacted packages, create an issue detailing the problems and determine how to hotfix the release. Once a fix is identified, create a PR and start the process for the next version starting with a Release Candidate and verify that the affected teams will not run into a problem when using the newly published Release Candidate.

After confirming, go through the stable release process with the hotfix.

At the end of this process, conduct a [post mortem using the template on our GitHub repo](https://github.com/carbon-design-system/carbon/tree/master/docs/postmortems).

---

- Release branches are named: `release/vX.Y.Z`
- Release commits are named `chore(release): vX.Y.Z`
- Release tags are named `vX.Y.Z`
- We never publish a package directly to latest
