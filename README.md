# Crafting100x

![](/thumbnail.png)

A PalSchema mod for **Palworld 1.0** that increases the output of crafted Pal Spheres, arrows, ammunition, and selected advanced materials.

Unlike older PAK-based recipe mods, this mod applies small runtime patches to individual recipe rows. It does **not** replace the complete recipe DataTable, so recipes added by newer game versions remain available.

## Features

- 100× output for all supported Pal Spheres, including the Ancient Spheres added in 1.0.
- 100× output for arrows and weapon ammunition while preserving each recipe's original batch size.
  - Example: a recipe that normally produces 20 rounds produces 2,000 rounds.
  - Example: a recipe that normally produces 10 arrows produces 1,000 arrows.
- 100× output for selected advanced materials:
  - Carbon Fiber
  - Low Temperature Coolant
  - Corrosive Solvent
  - Bio Battery
  - Thermal Core
  - Computer
  - AI Core
- The following outputs intentionally remain at 1× for balance:
  - Plastic
  - Stainless Steel
  - Sky Island Ingot
  - World Tree Ingot

The mod changes output quantities only. Material costs, work amounts, unlock requirements, and crafting stations are not modified.

## Requirements

- Palworld 1.0 or a compatible later version
- UE4SS Experimental for Palworld
- PalSchema 0.6.0 or a compatible later version

## Manual installation from GitHub

1. Install and verify **UE4SS Experimental for Palworld**.
2. Install and verify **PalSchema**.
3. Open the following directory:

```text
<Palworld or PalServer>\Pal\Binaries\Win64\UE4SS\Mods\PalSchema\mods\
```

4. Create a new folder named:

```text
Crafting100x
```

5. Open the `PalSchema` folder included in this release, then copy its contents into the newly created `Crafting100x` folder.

After installation, the directory structure must be:

```text
PalSchema\mods\Crafting100x\
├── metadata.json
└── raw\
    ├── advanced_materials_100x.json
    └── spheres_and_ammo_100x.json
```

Do not copy the release `PalSchema` folder itself into `Crafting100x`. The following structure is incorrect:

```text
PalSchema\mods\Crafting100x\PalSchema\
├── metadata.json
└── raw\
```

6. Restart the game or dedicated server after installing the mod.


For multiplayer, install the same version on the dedicated server and on every client. The server determines crafting results, while matching client files prevent the crafting UI from displaying values that differ from the actual result.

## Changing the multipliers

The mod stores final crafted output values in JSON files. You can edit them with any plain-text editor.

### Pal Spheres and ammunition

Edit:

```text
PalSchema/raw/spheres_and_ammo_100x.json
```

Each entry uses this format:

```json
"PalSphere": {
  "Product_Count": 100
}
```

`Product_Count` is the final quantity produced by one crafting operation, not a multiplier evaluated at runtime.

For example, the original Assault Rifle Ammo recipe produces 20 rounds. The included 100× setting is therefore:

```json
"AssaultRifleBullet": {
  "Product_Count": 2000
}
```

To make it 10× instead, use `200`. To restore the vanilla value, use `20` or remove that complete recipe entry from the JSON file.

### Advanced materials

Edit:

```text
PalSchema/raw/advanced_materials_100x.json
```

Most selected advanced materials normally produce one item, so:

```json
"AIcore": {
  "Product_Count": 100
}
```

means 100× output. To use 10× output, change the value to `10`. To keep an item at its vanilla 1× output, set it to `1` or remove its entry.

The balanced defaults already keep these recipes at `1`:

```json
"Plastic": {
  "Product_Count": 1
},
"StainlessSteel": {
  "Product_Count": 1
},
"SkyislandIngot": {
  "Product_Count": 1
},
"WorldTreeIngot": {
  "Product_Count": 1
}
```

### JSON editing rules

- Keep property names and recipe IDs inside double quotes.
- Keep commas between entries, but do not add a comma after the final entry in an object.
- Use whole numbers for `Product_Count`.
- Restart the game or server after editing.
- Keep identical edited files on the server and all clients.

If the mod stops loading after an edit, validate the JSON syntax and check the UE4SS/PalSchema log for the file and line that failed to parse.

## Compatibility

This mod is designed to be more update-friendly than a traditional recipe-table PAK because it modifies only named recipe rows and the `Product_Count` field.

It may conflict with another PalSchema mod that changes `Product_Count` for the same recipe. The result depends on load order. It should not conflict with mods that change unrelated recipe fields such as material costs or work amounts.

A major game update may rename recipe rows or change the recipe table structure. In that case, affected entries may stop working and the mod will need an update, but it will not replace or remove the rest of the game's recipe table.