# Project Versioning
This document describes two approaches of project build versioning, which will help developers keep track of their work and act as a point of reference in communications of features, bugs, tasks and etc.

## Server
Server projects versioning conforms to the [semantic versioning](http://semver.org). The gist of it is that each version is broken down to three components: `MAJOR`, `MINOR` and `PATCH`. For server projects, this notation applies not only to public builds, but also in-house testing builds.

1. `MAJOR` version represents incomptible API changes (i.e. deprecate API support, complete API behavior change, behavior breaking bug fixes)
2. `MINOR` version represents backward compatible changes (i.e. new features)
3. `PATCH` version represents backward compatible bug fixes.

For instance:
- If we found a critical bug after release `2.3.0`, we fix it by releasing `2.3.1`.
- We then added several new feature, it will be released as `2.4.0`.
- After several years of support, we decided to drop some API which only serves old devices, it will be released as `3.0.0`.

## iOS
iOS project releases subject to AppStore approval and it does not share a free lifecycle as the server projects. Hence, we make some modifications on top of [semantic versioning](http://semver.org).

1. Releases submitted to AppStore has `MAJOR`.`MINOR` version.
2. Release in-house has `MAJOR`.`MINOR`.`PATCH`.
3. `PATCH` version represents either bug fixes or new features
4. Bump `MINOR` version when submitting to AppStore if the change since the last AppStore release is backward compatible.
5. Bump `MAJOR` version when submitting to AppStore if the change since the last AppStore release is NOT backward compatible.

## Best Practices
- Use version planning in Jira and give developers a sense of what are they working on for this build. This helps wrapping up a build when all issues related to it are closed. It also avoid endlessly adding issues to a build and keep postponing the release date.
- Tag source code commits with the version number in git. This helps searching for various commits and see what was done.
