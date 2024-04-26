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

1. Create a new target Java SDK Repo by clicking the __Use this template__ button at the top of this repository.
1. Set `MAVEN_USERMANE` and `MAVEN_PASSWORD` action secrets in the target SDK repo with the values generated from the [Account](https://central.sonatype.com/account) page of the Maven Central Portal. (see [Appendix A](#appendix-a) for more information)
1. Set `GPG_PRIVATE_KEY` and `GPG_PASSPHRASE` action secrets in the target SDK repo (see [Appendix B](#appendix-b) for detailed instructions)
1. If you already have a Control Repo:

    1. Update your `LIBLAB_GITHUB_TOKEN` actions secret to a new token that has access to all your existing SDK repos, as well as this new one.

    1. Update your config file with the field values required for publishing:

        1. [`groupId`](https://developers.liblab.com/cli/config-file-overview-language/#groupid) with the value of your approved Central Repository namespace
        2. [`githubRepoName`](https://developers.liblab.com/cli/config-file-overview-language/#githubreponame) with the name of the target SDK repo
        3. [`homepage`](https://developers.liblab.com/cli/config-file-overview-language/#homepage) with the valid public URL of the SDK homepage

1. Run the GitHub Action `Generate SDKs using liblab` in the Control Repo that builds the SDK, and raises a PR against this target SDK Repo. This will be triggered automatically when you commit and push the update to the liblab config file.

1. Review and merge the PR.

1. Create a release in the target SDK Repo.

1. The GitHub Action `Publish to Maven Central Repository` in the target SDK Repo will be triggered by the release, and deploy the package to Maven Central Repository. 

1. Package will immediately appear on the [Deployments](https://central.sonatype.com/publishing/deployments) page under `PUBLISHING` status. After the validation process has been successfully finished, deployment will transition to `PUBLISHED` status, and the package will become available on [Maven Central Repository](https://central.sonatype.com/search). 


## Appendix A - Central Portal Setup

Publishing action automates publishing packages via the [Central Portal](https://central.sonatype.org/), which is, as of March 12th, 2024, the default publishing server for Maven packages. We do not support automatic publishing through the Legacy OSSRH.

### Account Creation

The [Central Portal Account Documentation](https://central.sonatype.org/register/central-portal/) explains how to create an account that is required for claiming namespaces and managing package deployments.

### Namespace Registration

Namespace is the crucial prerequisite for publishing a package to the Central Repository as it is the `groupId` of the package.  The [Central Portal Namespace Documentation](https://central.sonatype.org/register/namespace/#switching-to-ossrh-during-portal-early-access) provides detailed instructions for claiming a namespace. 

One should note that the namespace verification process is not instant. For own domain namespaces (e.g. `com.liblab`), additional step is required from the user, which involves setting up the verification key as a DNS record in order for the domain name to be verified by the Central Repository. For GitHub namespaces (namespace in the form of `io.github.githuborgname`), though, verification process is automatic if the user is registered using the GitHub SSO.   


### Generating Maven Credentials

Credentials for `MAVEN_USERNAME` and `MAVEN_PASSWORD` action secrets can be generated from the [Account](https://central.sonatype.com/account/) page.


## Appendix B - GPG Key and Passphrase

The Central Repository requires all artifacts to be signed with GPG. This section explains how to generate your own key pair, distribute it to the key server and obtain the private key.

1. Install [GnuPG](https://www.gnupg.org/download/)\
    (available through `brew install gnupg`)

1. Initialize the key generation prompt by running:\
    `gpg --gen-key`

1. Input all necessary data and choose the passphrase

1. Copy the key id - 40 character long value printed out in the second row by the command:\
    `gpg --list-secret-keys`

1. Send the key to the key server (in our case `openpgp`) by running:\
    `gpg --keyserver keys.openpgp.org --send-keys ${key-id}`

1. Export the private key to the file by running:\
    `gpg --export-secret-keys ${key-id} --armour > key.asc`

1. Copy the contents of the `key.asc` file and paste it in the `GPG_PRIVATE_KEY` action secret

1. Add chosen passphrase to the `GPG_PASSPHRASE` action secret
