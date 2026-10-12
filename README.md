# go-pob-testdata

Test data for [go-pob](https://github.com/Vilsol/go-pob), published as GitHub release assets. The repository itself
holds only this README and the license; the data lives in the releases.

Each release carries, for one game-data version:

| asset | contents |
|---|---|
| `corpus-<version>.tar.br` | `corpus-<version>/`: real Path of Building build exports (XML) and the corpus `MANIFEST.md` |
| `oracle-<version>.tar.br` | `oracle-<version>/`: what the pinned Path of Building computes for every corpus build |
| `SHA256SUMS` | the SHA-256 of each archive |

The archives are tar + brotli, packed deterministically (sorted entries, fixed mtime and owner), so the same tree
always packs to the same bytes.

## Release naming

`<version>-<yyyy.mm.dd>`, e.g. `3.29-2026.10.12`: the game-data version the corpus belongs to and the date it was
packed. A new corpus or a regenerated oracle is a new release; releases are never edited in place.

## How go-pob consumes it

go-pob pins one release in `scripts/testdata.json` (the tag and each asset's SHA-256). In a go-pob checkout:

```shell
mise run testdata
```

downloads the pinned assets, refuses any checksum mismatch, unpacks them into `testdata/` and records the installed
tag in `testdata/.installed-release`; it does nothing when that release is already installed. `TESTDATA_BASE_URL`
points it at a mirror instead of this repository's releases.

## How to update

In go-pob, with the new corpus / oracle in `testdata/`:

```shell
mise run testdata-pack OUT=<dir>
```

writes the archives and `SHA256SUMS` into `<dir>`, refreshes `scripts/testdata.json`, and prints the
`gh release create` command that publishes them here. Run it, then commit `scripts/testdata.json` in go-pob. The
packer is [`scripts/testdatarelease`](https://github.com/Vilsol/go-pob/tree/dev/scripts/testdatarelease) in go-pob.

## License

Distributed under the same terms as go-pob; see [LICENSE](LICENSE).
