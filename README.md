# Book a secure move frameworks

YAML defintions of the [Book a secure move](https://github.com/ministryofjustice/hmpps-book-secure-move) service frameworks.

The repository contains the following frameworks:

- [Person Escort Record (PER)](./frameworks/person-escort-record)

## Creating a new release

1. Checkout and pull the latest version of master
1. Increase the version property using the [`npm version`](https://docs.npmjs.com/cli/version) command. Follow [semantic versioning](https://semver.org/) for version numbers. This will create the git version commit and tag associated to the new version as well.
1. Increase the version number in the package.json `name` property to match the new version. This is to support the method needed to cater for multiple versions of the framework in the [frontend project](https://github.com/ministryofjustice/hmpps-book-secure-move-frontend/blob/d374ac9e46f0e258ec2a9fa1bbc9a7df2fb81cc4/package.json#L70-L71).
1. Push the commit to the remote server (`git push`)
1. Push tags to remote (`git push --tags`)
