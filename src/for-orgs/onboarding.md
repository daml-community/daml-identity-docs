# Onboarding
Adding keys to the repository is done through a GitHub PR. A PR can be merged when it meets the `gittuf` security requirements for the repo, which is currently two votes from **maintainers**.

Before opening a PR make sure:

- You understand the [file structure](../repo-structure/files.md) of the repository
- You have been added as a collaborator
- You have `gittuf` and `git` installed
- You have enabled [gpg signing of commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)

## Writing the PR
1. Check out the repository locally.
2. Create a new branch based off `master`, and commit your organisation's files. 
  As a reminder, an organisation folder should contain:
    - A [`_meta.json`](../repo-structure/files.md#org-metadata) file that specifies metadata about the organisation
    - A collection of [`<person>.json`](../repo-structure/files.md#person-metadata) files and their associated [`<person>.pub`](../repo-structure/files.md#person-public-key) public keys.

3. Record the gittuf reference state via `gittuf rsl record <branch-name>`
4. Push your changes to the remote
5. Run a `gittuf sync`
6. Open a PR, and request reviews from **maintainers**
7. When 2 maintainers approve, the PR can be merged.
