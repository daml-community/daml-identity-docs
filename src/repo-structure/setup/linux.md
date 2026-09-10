# Linux Setup
Install `gnupg` and `git` through your distribution's package manager. To install gittuf on linux either install through your distribution's package manager, or install [go](https://go.dev/), then run the following commands:

```sh
go install github.com/gittuf/gittuf@latest
go install github.com/gittuf/gittuf/internal/git-remote-gittuf@latest
```

To install the latest version from source.

## Configuring `git` and `gpg`
Git must be configured to use `gpg` to sign commits. To do so we need to:
1. Generate a `gpg` key
2. Instruct `git` to use this key to sign commits.

To generate a `gpg` key, run:
```sh
gpg --full-generate-key
```

And follow the instruction prompt, selecting `(1) RSA and RSA`. After completing the instructions, the resulting screen will show the public key fingerprint:
```
pub     rsa3072 2026-09-07 [SC]
        AAAA AAAA AAAA AAAA AAAA  AAAA AAAA AAAA AAAA AAAA
uid     ....
```

The fingerprint is shown on the second line above. Make a note of this, or run `gpg --fingerprint`.

To configure git properly, edit your git configuration file so that the following lines are present:

```
[commit]
  gpgSign = true

[tag]
  gpgSign = true

[user]
  email = "<email>"
  name = "<name>"
  signingKey = "AAAA AAAA AAAA AAAA AAAA  AAAA AAAA AAAA AAAA AAAA"
```

## Adding gpg keys to GitHub
To export your gpg public key and copy it to the clipboard run:

```
gpg --export --armor "<fingerprint>" | wl-copy
```

And follow the process outlined [here](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
