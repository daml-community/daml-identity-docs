# Linux Setup
Install `gnupg` and `git` through your distribution's package manager. To install gittuf on linux either install through your distribution's package manager, or install [go](https://go.dev/), then run the following commands:

```sh
go install github.com/gittuf/gittuf@latest
go install github.com/gittuf/gittuf/internal/git-remote-gittuf@latest
```

To install the latest version from source.

## Configuring `git` and `gpg`
We must first generate a `gpg` key, if you do not already have one, to represent your organisation. It is important that YOU DO NOT LOSE THIS KEY. We would advise that this key is kept somewhere secure by the organisation. Once this key is generated, the most secure practice is then to distribute a subkey to anyone that wishes to use this key, and to never use the key directly. The following documents the process of:
1. Generating an organisation `gpg` key
2. Adding a subkey to this key
3. Exporting the subkey
4. Importing the subkey
5. Adding the subkey to your `git` config
6. Adding the subkey to your `github` account

### Generating an organisation `gpg` key
To generate a `gpg` key, run:
```sh
gpg --full-generate-key
```

And follow the instruction prompt, selecting `(1) RSA and RSA`, entering your organisation's name, a secure password, and NO expiration date. After completing the instructions, the resulting screen will show the public key fingerprint:
```
pub     rsa3072 2026-09-07 [SC]
        AAAA AAAA AAAA AAAA AAAA  AAAA AAAA AAAA AAAA AAAA
uid     ....
```

### Adding a subkey to this key
To add a subkey to the key, run:
```sh
gpg --expert --edit-key 'AAAA AAAA AAAA AAAA AAAA  AAAA AAAA AAAA AAAA AAAA' # your key fingerprint here
gpg> addKey
```

Follow the setup instructions selecting:
- (4) RSA (sign only)
- A sensible validity period (1 year?)

### Exporting the subkey
To export the subkey run:
```sh
gpg --list-secret-keys
gpg --export-secret-subkeys <SUBKEYFINGERPRINT>! > subkey.asc
```

### Importing the subkey
To import the subkey on a new machine, run:
```sh
gpg --import subkey.asc
```

### Adding the subkey to your `git` config
The fingerprint is shown on the second line above. Make a note of this, or run `gpg --fingerprint`.

To configure git properly, edit your git configuration file (`~/.config/git/config`) so that the following lines are present:

```
[commit]
  gpgSign = true

[tag]
  gpgSign = true

[user]
  email = "<email>"
  name = "<name>"
  signingKey = "<SUBKEYFINGERPRINT>"
```

Where the subkey fingerprint can be found via:
```sh
gpg --list-secret-keys
```

### Adding the subkey to your github account
To export your gpg public key and copy it to the clipboard run:

```
gpg --export --armor "<fingerprint>" | wl-copy
```

And follow the process outlined [here](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)

## Altering the `gittuf` policy to add the new maintainer
In order the alter the gittuf policy the following information must be known
- The subkey from the step above
- The keyholder's GitHub **username** 
- The keyholder's GitHub **id** 

> [!NOTE] A user's GitHub id can easily be found from their username via the following http endpoint: 
> ```sh
> curl -s https://api.github.com/users/<username>
> | jq '.id'
> ```

The altering of the policy is done in two stages:
1. An existing maintainer proposes an amendent to the policy
2. Another maintainer co-signs this amendment

### Proposing an amendment (Current Maintainer 1)
A current **maintainer** must:
- Add the identity of the new organisation as a **policy signer**:
  ```sh
  gittuf policy add-person -k "gpg:<current-member-gpg-key>" --public-key "gpg:<new-member-gpg-key>" --person-ID "<new-member-name>" --associated-identity "https://gittuf.dev/github-app::<new-member-github-username>+<new-member-github-id>"
    ```
- propose an amendment to the security policy so as to allow the new member to vote:
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

### Approving the ammendement (Current Maintainer 2)
Another **maintainer** must then counter-sign this policy on their own checked-out repo:
```sh
gittuf policy remote pull
gittuf policy sign -k "gpg:<member-2-key>"
gittuf policy stage
gittuf policy apply --create-rsl-entry
```

