# Template repo for Java SDK  
This template helps developers get started with publishing the Java SDK to Maven Central package repository.

## Prerequisites
The user will need the following:

- A [Maven Central](https://central.sonatype.org/) account and token credentials generated on the [Account](https://central.sonatype.com/account) page that allow uploading packages to the [Central Maven Repository](https://repo.maven.apache.org/maven2/)
- Generated [GPG](https://central.sonatype.org/publish/requirements/gpg/#managing-your-credentials) key pair that is used for signing artifacts during the publishing process

## Contents
This repository contains the following:

- A `README` that contains the instructions
- A GitHub Action workflow to publish the Java SDK to Maven Central package repository.


## Instructions

1. Create a new target Java SDK Repo by clicking the **Use this template* button** at the top of this repository.
1. Set `MAVEN_USERMANE` and `MAVEN_PASSWORD` action secrets in the target SDK repo with the values generated from the (Account)[https://central.sonatype.com/account] page of the Maven Central Portal. (see [Appendix A](#appendix-a) for more information)
1. Set `GPG_PRIVATE_KEY` and `GPG_PASSPHRASE` action secrets in the target SDK repo (see [Appendix B](#appendix-b) for detailed instructions)
1. If you already have a Control Repo:

    1. Update your `LIBLAB_GITHUB_TOKEN` actions secret to a new token that has access to all your existing SDK repos, as well as this new one.

    1. Update your config file with field values required for publishing

        1. [`groupId`](https://developers.liblab.com/cli/config-file-overview-language/#groupid) with the value of your approved Central Repository namespace
        2. [`githubRepoName`](https://developers.liblab.com/cli/config-file-overview-language/#githubreponame) with the name of the target SDK repo
        3. [`homepage`](https://developers.liblab.com/cli/config-file-overview-language/#homepage) with the valid homepage URL of the SDK

1. Run the GitHub Action `Generate SDKs using liblab` in the Control Repo that builds the SDK, and raises a PR against this target SDK Repo. This will be triggered automatically when you commit and push the update to the liblab config file.

1. Review and merge the PR.

1. Create a release in the target SDK Repo.

1. The GitHub Action `Publish to Maven Central Repository` in the target SDK Repo will be triggered by the release, and deploy the package to Maven Central Repository. 

1. Package will immediately appear on the [Deployments](https://central.sonatype.com/publishing/deployments) page under `PUBLISHING` status. After the validation process has been successfully finished, deployment will transition to `PUBLISHED` status and the package will become available on [Maven Central Repository](https://central.sonatype.com/search). 


## Appendix A

- 

## Appendix B

-
