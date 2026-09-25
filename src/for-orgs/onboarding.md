# Onboarding
Adding keys to the repository is done through a GitHub PR. A PR can be merged when it meets the `gittuf` security requirements for the repo, which is currently two votes from **maintainers**.

Before opening a PR make sure:

- You understand the [file structure](../repo-structure/files.md) of the repository
- You have been added as a collaborator

## Writing the PR
1. Check out the repository locally.
2. Create a new branch based off `master`, and commit your organisation's files. 
  Create a folder `/<org-mame>` that contains the following:
    - An [`org.json`](../repo-structure/files.md#org-metadata) file that specifies metadata about the organisation. Use the following template:
    ```json
    {
      "organisation": {
        "name": "<organisation-name>"
      }
    }
    ```
    - An `org.pub` file, specifying the organisation's public key. This can be generated via:
    ```sh
    gpg --quick-generate-key "Org Name <optional email>" rsa3072 default never
    ```
    - A collection of [`<person>.json`](../repo-structure/files.md#person-metadata) files and their associated [`<person>.pub`](../repo-structure/files.md#person-public-key) public keys. For each employee, generate:
        - A `<person>.json` file. Use the following template:
        ```json
        {
          "name": "Alice",
          "valid": [
            {
              "roles": ["auditor", "publisher"],
              "from": "2020-01-01",
              "to": "2021-01-01"
            },
            {
              "roles": ["auditor"],
              "from": "2026-01-01",
              "to": "2027-01-01"
            }
          ]
        }
        ```
        - A `<person>.pub` file, generated via:
        ```sh
        gpg --quick-generate-key "Test User <test.user@gmail.com>" rsa3072 default never
        gpg --default-key <org-key-fingerprint> --sign-key <pub-key-fingerprint>
        gpg --export --armor <pub-key-fingerprint> > '<person>.pub'
        ```
3. Push your changes to the remote on a new branch. (Collaborator status is required)
4. Open a PR, and request reviews from **maintainers**
5. When 2 maintainers approve, the PR can be merged.
