# macOS Setup
For installation on `macOS`, [install homebrew](https://brew.sh/), then run the following:

```sh
brew install gnupg pinentry-mac git gittuf
echo "pinentry-program $(which pinentry-mac)" >> ~/.gnupg/gpg-agent.conf
gpg-connect-agent reloadagent /bye
```

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
gpg --export --armor "<fingerprint>" | pbcopy
```

And follow the process outlined [here](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
