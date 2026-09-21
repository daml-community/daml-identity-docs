# Windows Setup
Install `gpg4win`, `git` and `gittuf` from winget by running the following from powershell:

```sh
winget install GnuPG.Gpg4win
winget install Git.Git
winget install gittuf.gittuf
winget install gittuf.git-remote-gittuf
git config --global gpg.program $(Resolve-Path (Get-Command gpg | Select-Object -Expand Source) | Select-Object -Expand Path)
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

> [NOTE] Your user git configuration file lives in `C:\Users\<user>\.gitconfig`
> It can be made and edited with the following commands:
> ```
> notepad C:\Users\<user>\.gitconfig
```


## Adding gpg keys to GitHub
To export your gpg public key and copy it to the clipboard run:

```
gpg --export --armor "<fingerprint>" | Set-Clipboard
```

And follow the process outlined [here](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
