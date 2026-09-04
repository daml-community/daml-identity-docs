# Adding a New Maintainer
## Prerequisites
Before promoting an organisation to be a **maintainer**, the organisation must decide on a keyholder responsible for carrying out responsibilities as a **maintainer**. The following information needs to be known:

- The keyholder's `gpg` key

- The keyholder's GitHub **username** and **id**

> NOTE: A user's GitHub id can easily be found from their username via the following http endpoint:
> ```sh
> curl -s https://api.github.com/users/<username>
> | jq '.id'
> ```

## Process
A current **maintainer** must then:
- Add the identity of the new organisation as a **policy signer**:
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


Another **policy signer** must then counter-sign this policy on their own checked-out repo:
```sh
gittuf policy remote pull
gittuf policy sign -k "gpg:<member-2-key>"
gittuf policy stage
gittuf policy apply --create-rsl-entry
```
