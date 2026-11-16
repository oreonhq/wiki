# Oreon Contribution Policy

Anyone may contribute to Oreon. That includes code, packaging, docs, testing, design, and bug reports.

By submitting a pull request, patch, or other contribution to an Oreon repository, you agree to this policy. Actions that break it will not be accepted. AI use while contributing is covered in the [AI Policy](ai.md).

## Right to contribute

Only submit work you have the legal right to contribute.

You confirm that:

- the work is yours, or
- you have permission to submit it under the repository license

If your employer has rights to what you make, you also confirm you have their permission, or that they have waived those rights for this contribution.

## Copyright and license grant

You keep the copyright in your contribution.

You grant Oreon a perpetual, worldwide, non-exclusive, royalty-free, irrevocable license to use, reproduce, modify, prepare derivative works of, publicly display, publicly perform, sublicense, and distribute your contribution and those derivative works under the license of the repository you contributed to.

## Same license as the repo

Your contribution is licensed under the same terms as the repository you submit it to. If that repo is GPLv3, your contribution is GPLv3. Same idea for any other allowed license.

All Oreon repositories must include a license file. Allowed licenses include GPLv2, GPLv3, MIT, or another license approved by the Oreon project lead. Prefer GPLv3 unless there is a real reason not to.

## Relicensing

Changing a repository's license needs agreement from:

- the contributors to that repository
- the Oreon project lead
- BoostyConnect Technologies

No one else can relicense that repo on their own.

## Official repositories

Official Oreon development happens in the project's public repositories.

Personal forks and private branches are fine while you work. Code that should become part of Oreon has to land in an official Oreon repository. Once it is merged there, it is part of the project and follows that repo's license plus this policy.

## Commit messages

Commits need enough detail that someone can tell what changed. Vague subjects are not useful.

Packaging commits are one spec each, with a subject like `sed: update to 4.10`. Read [Commits](../packaging/packaging-commits.md) for the full documentation.

## Pull requests

PRs should say what the change is for. Include:

- a clear description
- a reasonable commit history
- any testing that matters

Maintainers can ask for changes before merge. Not every contribution will be accepted.

## Maintainer responsibilities

Maintainers protect the stability and direction of their area. They may:

- review and merge contributions
- request changes
- reject contributions that do not fit project goals
- revert changes that cause problems
