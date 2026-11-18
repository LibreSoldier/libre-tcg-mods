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
      "description": "What it does, in a few lines.",
      "logo": "https://raw.githubusercontent.com/author/repo/main/logo.png",
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
- `description` (optional): text shown before the mod is downloaded, at most 2000 characters. Once the mod is downloaded, the launcher shows the description written in the DLL itself instead, when it has one. Mod authors set it with `<Description>...</Description>` in the `.csproj` (it becomes `[assembly: AssemblyDescription]`).
- `logo` (optional): an `http(s)` link to the mod's logo. The launcher shows it next to the mod's name, smoothly shrunk to fit 64×64, and downloads it only when someone opens the mod's details. As with the description, a link written in the DLL itself wins once the mod is downloaded.
  - Use a **square PNG of 256×256** (at most 1024×1024 and 1 MB, 8 or 16 bits, not interlaced). A transparent background works. One image serves everywhere: the launcher shrinks it, and a mod can show the same file bigger inside the game.

### Description and logo inside the mod

Mod authors can put both in the DLL, so every server and catalog shows the same thing, and mods installed from a file have them too. In the `.csproj`:

```xml
<PropertyGroup>
  <Description>What the mod does, in a few lines.</Description>
</PropertyGroup>
<ItemGroup>
  <AssemblyMetadata Include="Logo" Value="https://raw.githubusercontent.com/author/repo/main/logo.png" />
</ItemGroup>
```

The logo is a link, not a file inside the DLL: it keeps the DLL small, and the same hosted image can be used elsewhere (for example, shown inside the game by the mod). The launcher reads both without loading or running the DLL.

Servers list the mods they **require** or support (`"optional": true`) in their own `client-mods.json`, in the same format. Those mods don't belong in this catalog.

## Adding a mod

Open a pull request that adds your entry to `catalog.json`. Link the mod's source code, and make sure the hash matches the released file.
