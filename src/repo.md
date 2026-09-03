This repository contains the identities of verfied **developers** and **auditors** of DAML packages.

# Repository Structure
The repository is protected by a `gittuf` security layer. In order to contribute you will need to have `gittuf` [installed](https://gittuf.dev/quickstart/), and have [enabled gpg signing](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key) on your git commits. See the `gittuf` [docs](https://gittuf.dev/documentation) for more info.

## `gittuf` Configuration
The repository has the following configuration:
- The **root of trust** consists of `patrickaldis`. These members can propose new **policy signers**.
- The **policy signers** consist of:
  - `<obsidian-keyholder>`
  - `<da-keyholder>`
  - `<cf-keyholder>`

  Who can propose and attest to new policies
- The current **policies** are:
  - Any modification to any file must be approved by at least 2 **policy signers**

# Common Processes
## Onboarding an Organisation
If you are an organisation that wishes to be onboarded:

1. Request to be added as a collaborator to the repository.
2. Clone the repository via `git`
3. Run `gittuf sync`
4. Stage and commit the relevant files to a branch, ensuring that you have set up [gpg signing](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key)
5. Run the following command to ensure that the `gittuf` reference state log is populated:
  ```sh
  gittuf rsl record <your-branch-name> origin
  ```
6. Open a PR.
7. Gain 2 approvals from **policy signers**.

## Promoting an Organisation to **Policy Signer**
Before promoting an organisation to **Policy Signer** the following information needs to be known:
- The organisation's `gpg` key
- The organisation's github **username** and **id**

> NOTE: A user's github id can easily be found from their username via the following http endpoint:
> ```sh
> curl -s https://api.github.com/users/<username>
> | jq '.id'
> ```

A current **policy signer** must then:
- Add the identity of the new organisation:
  ```sh
  gittuf policy add-person -k "gpg:<current-member-gpg-key>" --public-key "gpg:<new-member-gpg-key>" --person-ID "<new-member-name>" --associated-identity "https://gittuf.dev/github-app::<new-member-github-username>+<new-member-github-id>"
    ```

- Propose an amendment to the security policy so as to allow the new member to vote:
  ```sh
    gittuf policy remove-rule --rule-name all-files
    gittuf policy add-rule \
      --signing-key "gpg:<current-member-gpg-key>" \
      --rule-name "all-files" \
      --rule-pattern "file:*" \ --authorize "<current-member-1-name>" \
      --authorize "<current-member-2-name>" \
      -- ... \
      --authorize "<new-member-name>" \
      --threshold 2
  ```


Another **policy signer** must then counter-sign this policy on their own checked out repo:
```sh
gittuf policy remote pull
gittuf policy sign -k "gpg:<member-2-key>"
gittuf policy stage
gittuf policy apply --create-rsl-entry
```
