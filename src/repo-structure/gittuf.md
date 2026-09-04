# Gittuf Policy
The repository is protected by a `gittuf` security layer. In order to contribute you will need to have `gittuf` [installed](https://gittuf.dev/quickstart/), and have [enabled gpg signing](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key) on your git commits. See the `gittuf` [docs](https://gittuf.dev/documentation) for more info.

## `gittuf` Configuration
The repository has the following configuration:
- The **root of trust** consists of the following:
  - `<obsidian-keyholder>`
  - `<da-keyholder>`
  - `<cf-keyholder>`
- These users are also **policy signers**
- The current **policies** are:
  - Any modification to any file must be approved by at least 2 **policy signers**

