# File Structure
The repository has the following structure:

```txt
daml-identity/
├── <org-1>/
│   ├── _meta.json   <- Org metadata
│   ├── alice.json   <- Person metadata
│   ├── alice.pub    <- Person public key
│   ├── bob.json
│   ├── bob.pub
│   ├── charlie.json
│   └── charlie.pub
│
└── <org-2>/
    ├── _meta.json
    ├── <person>.json
    ├── <person>.pub
    └── ...
```

## Org Metadata
Currently the org metadata simply contains `organisation.name`:

```json
{
  "organisation": {
    "name": "Org A"
  }
}
```

### Fields

- `organisation.name` : The name of the organisation

## Person Metadata
A person can contain the following data:

```json
{
  "name": "Alice",
  "valid": [
    {
      "from": "2020-01-01",
      "to": "2021-01-01"
    },
    {
      "from": "2026-01-01",
      "to": "2027-01-01"
    }
  ]
}
```

### Fields
- `name` : Person's name
- `valid[]` : A list of `{ from: ...,  to: ... }` declaring the periods of time during which these keys should be associated with this person
- `valid[].from` : An ISO 8601 date specifying the starting date of a period
- `valid[].to` : An ISO 8601 date specifying the finishing date of a period

## Person Public Key
Each `<person>.pub` file is a user's public gpg key. This can be exported by running:
```sh
gpg --export --armor <gpg-fingerprint> > <person-name>.pub
```
The fingerprint of a key can be obtained via `gpg --fingerprint`.
