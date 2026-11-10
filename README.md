# LibreTCG Mods Catalog

The list of optional **client mods** shown by Libre's TCG Online Launcher in **Mods → + Add Mods**. These are BepInEx plugins for the PTCGO 2.95 client, such as visual or quality-of-life mods, that work on any server.

The launcher downloads [`catalog.json`](catalog.json) from this repository. It checks every mod file against its SHA-256 before using it, and installs nothing without the player's consent.

This index lives on GitHub for now. It may move to a website later; the format will stay the same.

## Format

```json
{
  "mods": [
    {
      "guid": "author.modname",
      "name": "Readable Name",
      "version": "1.0.0",
      "description": "One line about what it does.",
      "file": "ModName.dll",
      "url": "https://github.com/author/repo/releases/download/v1.0.0/ModName.dll",
      "sha256": "64 hex characters"
    }
  ]
}
```

- `guid`: the plugin's BepInEx GUID. Use letters, digits, `.`, `_` and `-`.
- `file`: a plain `.dll` file name. The launcher installs it to `BepInEx/plugins/<guid>/<file>`.
- `url`: a stable `https` download link, such as a GitHub release asset.
- `sha256`: the SHA-256 of that exact file. Publishing a new version means adding a new link and hash.

Servers list the mods they **require** or support (`"optional": true`) in their own `client-mods.json`, in the same format. Those mods don't belong in this catalog.

## Adding a mod

Open a pull request that adds your entry to `catalog.json`. Link the mod's source code, and make sure the hash matches the released file.
