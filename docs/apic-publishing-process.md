# API components publishing process

## Git branches

As in most projects the master branch represents production state of components. This means the component published in NPM and on GitHub release page always reflect the state of the master branch. However, main branch in every component repository is `stage`. This is a transition layer between master and development. In most cases `stage` equals `master`.
The `stage` branch is created to accept pull requests from contributors and the community. This branch initializes tests and CI pipeline. After all checks finish with success the CI pipeline automatically builds the release and documentation, and merges changes with master.
The rule of thumb here is that commits to the `master` branch are never made manually.

## Sending the PR flow

![Sending PR to stage](../resources/sending-pr-flow.png)

## Running the CI flow

The CI pipeline has the following tasks:

-   Update `changelog.md` file
-   Merge `stage` with `master`
-   Build docs for version and insert to the datastore
-   Build changelog for version and insert to datastore
-   Publish release

The CI pipeline only accepts status change from `stage` and `master` branches. Depending on the state it performs merge or publishing operations.

The changelog and docs are inserted into the datastore. This data are available via [API Components API](https://api.advancedrestclient.com).

Note, the CI pipeline do not bump version for the release. It has to be done manually when commiting changes. Automatic bumping system was tested previously and did not meet expectations.

![Running the CI pipeline](../resources/run-ci-pipeline.png)

Next: [Codding style](code-style.md)
Previous: [API components development process](apic-development-process.md)
