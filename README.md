# PS4 Cheats and Patches Repository

Official Cheat and Patches used for shadPS4 emulator

## Repository Structure

### Cheats

Cheats should be organized in the `CHEATS` folder with the following file name format: `SERIAL_VERSION.json`.

**Examples:**
- `CUSA07023_01.03.json`
- `CUSA07023_01.03_2.json`
- `CUSA07023_01.03_3.json`

**Note:** Each cheat file should be associated with a single author.

Additionally, update the `CHEATS_JSON.txt` file with the name of the added cheat file. The format is:

```
<fileName.json>=<game name>
```

**Examples:**
```
CUSA00265_01.95.json=Minecraft
CUSA14683_01.04_2.json=The Ascent
CUSA03617_01.12.json=Mafia III: Definitive Edition
SLES52482_01.00.json=Steel Dragon EX
CUSA16579_01.61_2.json=Cyberpunk 2077
CUSA34766_01.05.json=Granblue Fantasy Relink
```


**Example Cheat File:**

*`CHEATS/CUSA07023_01.03_TEST.json`*
<details>

```json
{
  "name": "Sonic Mania",
  "id": "CUSA07023",
  "version": "01.03",
  "process": "eboot.bin",
  "mods": [
    {
      "name": "Infinite Rings",
      "hint": null,
      "type": "checkbox",
      "memory": [
        {
          "offset": "450759",
          "on": "C683EC000000FF",
          "off": "488983EC000000"
        },
        {
          "offset": "4A5975",
          "on": "66C783EC000000E703909090",
          "off": "8B88B4030000018BEC000000"
        }
      ]
    },
    {
      "name": "Infinite Lives",
      "hint": null,
      "type": "checkbox",
      "memory": [
        {
          "offset": "4AA374",
          "on": "909090909090",
          "off": "8983F8000000"
        }
      ]
    },
    {
      "name": "Enable Super Sonic",
      "hint": null,
      "type": "checkbox",
      "memory": [
        {
          "offset": "A4415C",
          "on": "01",
          "off": "02"
        }
      ]
    },
    {
      "name": "Disable Super Sonic",
      "hint": null,
      "type": "button",
      "memory": [
        {
          "offset": "A4415C",
          "on": "03",
          "off": "03"
        }
      ]
    },
    {
      "name": "Super Speed",
      "hint": null,
      "type": "button",
      "memory": [
        {
          "offset": "A44180",
          "on": "00000F0000400000",
          "off": "00000F0000400000"
        }
      ]
    },
    {
      "name": "All Silver Medals",
      "hint": null,
      "type": "button",
      "memory": [
        {
          "offset": "200036604",
          "on": "20",
          "off": "20"
        }
      ]
    }
  ],
  "credits": [
    "Talixme"
  ]
}
```
</details>


### Patches

Set base address to `0x00400000` when importing binaries for consistency with PS4 memory address. (ASLR disabled)
* [Ghidra](https://ghidra-sre.org/)
  * [GhidraOrbis](https://github.com/astrelsky/GhidraOrbis/releases/latest)
* [IDA Pro](https://hex-rays.com/ida-pro/)
  * [PS4 Module Loader / IDA 7.0-7.7](https://github.com/SocraticBliss/ps4_module_loader/releases/latest)
* Text editors:
  * [Visual Studio Code](https://code.visualstudio.com/)
  * [VSCodium](https://vscodium.com/)

### Submission Guidelines
* Patch must be named `GameTitle.xml` and be in `/PATCHES`.
<br>For example, a patch file for Gravity Rush 2 must be called `GravityRush2.xml`.
* If you are making a patch for a game that already has a file, then add to it, as patches support more than one author.
* Submitting patches:
  * No whitespace.
  * Lowercase hex for address/value hex, uppercase for Title ID.

### Patch types

| `type`        | Info                                                                                                                                                                                                                                                         | Value (example)        |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `byte`        | Hex, 1 byte                                                                                                                                                                                                                                                  | `"0x00"`               |
| `bytes16`     | Hex, 2 bytes                                                                                                                                                                                                                                                 | `"0x0000"`             |
| `bytes32`     | Hex, 4 bytes                                                                                                                                                                                                                                                 | `"0x00000000"`         |
| `bytes64`     | Hex, 8 bytes                                                                                                                                                                                                                                                 | `"0x0000000000000000"` |
| `bytes`       | Hex, max size is 255 bytes string (no spaces)                                                                                                                                                                                                                | `"####"`               |
| `float32`     | Float, single                                                                                                                                                                                                                                                | `"1.0"`                |
| `float64`     | Float, double                                                                                                                                                                                                                                                | `"1.0"`                |
| `utf8`        | String, UTF-8*                                                                                                                                                                                                                                               | `"string"`             |
| `utf16`       | String, UTF-16*                                                                                                                                                                                                                                              | `"string"`             |
| `mask`        | Pattern Scan, max size is 255 bytes string (with spaces)<br>**Parameters**:<br>`Offset`: Offset from first address of found address                                                                                                                          | `"aa bb ?? dd"`        |
| `mask_jump32` | Pattern Scan With Branch (32 bit), max size is 255 bytes string (with spaces)<br>**Parameters**:<br>`Target`: target bytes string for branch<br>`Size`: size of jump (min: 5) with nop padding after<br>`Offset`: Offset from first address of found address | `"aa bb ?? dd"`        |

* Note: Strings must be manually null terminated.

#### Example Patch File

*`PATCHES/ShadowoftheColossus-Orbis_TEST.xml`*

<details>
  
```xml
<?xml version="1.0"?>
<Patch>
    <TitleID>
        <ID>CUSA08804</ID>
        <ID>CUSA08809</ID>
        <ID>CUSA08034</ID>
    </TitleID>
    <Metadata Title="Shadow of the Colossus" Name="Debug Menu" Author="illusion" PatchVer="1.0" AppVer="01.00" AppElf="eboot.bin" isEnabled="false">
        <PatchList>
            <Line Type="bytes" Address="0x004244f8" Value="e9f0000000"/>
            <Line Type="bytes" Address="0x004245ed" Value="488b03"/>
            <Line Type="bytes" Address="0x004245f0" Value="483b4308"/>
            <Line Type="bytes" Address="0x004245f4" Value="c6053589dd0200"/>
            <Line Type="bytes" Address="0x004245fb" Value="e9fffeffff"/>
        </PatchList>
    </Metadata>
    <Metadata Title="Shadow of the Colossus" Name="Skip Splash Logo + Demo Screens" Author="illusion" PatchVer="1.0" AppVer="01.00" AppElf="eboot.bin" isEnabled="true">
        <PatchList>
            <Line Type="bytes" Address="0x032fae20" Value="01"/>
            <Line Type="bytes" Address="0x008c95bc" Value="c683c000000001"/>
        </PatchList>
    </Metadata>
    <Metadata Title="Shadow of the Colossus" Name="60 FPS" Author="illusion" PatchVer="1.0" AppVer="01.00" AppElf="eboot.bin" isEnabled="false">
        <PatchList>
            <Line Type="bytes" Address="0x01390090" Value="31c0c3"/>
        </PatchList>
    </Metadata>
    <Metadata Title="Shadow of the Colossus" Name="Debug Menu" Author="illusion" PatchVer="1.0" AppVer="01.01" AppElf="eboot.bin" isEnabled="false">
        <PatchList>
            <Line Type="bytes" Address="0x004244f8" Value="e9f0000000"/>
            <Line Type="bytes" Address="0x004245ed" Value="488b03"/>
            <Line Type="bytes" Address="0x004245f0" Value="483b4308"/>
            <Line Type="bytes" Address="0x004245f4" Value="c6053589dd0200"/>
            <Line Type="bytes" Address="0x004245fb" Value="e9fffeffff"/>
        </PatchList>
    </Metadata>
    <Metadata Title="Shadow of the Colossus" Name="Skip Splash Logo + Demo Screens" Author="illusion" PatchVer="1.0" AppVer="01.01" AppElf="eboot.bin" isEnabled="true">
        <PatchList>
            <Line Type="bytes" Address="0x032fae20" Value="01"/>
            <Line Type="bytes" Address="0x008c95bc" Value="c683c000000001"/>
        </PatchList>
    </Metadata>
    <Metadata Title="Shadow of the Colossus" Name="60 FPS" Author="illusion" PatchVer="1.0" AppVer="01.01" AppElf="eboot.bin" isEnabled="false">
        <PatchList>
            <Line Type="bytes" Address="0x01390090" Value="31c0c3"/>
        </PatchList>
    </Metadata>
</Patch>
```
</details>

*Contains information obtained from [GoldHEN_Patch_Repository](https://github.com/illusion0001/PS4-PS5-Game-Patch)

## Contributing

If you would like to contribute new cheats or patches, please follow these guidelines:

1. Add your files to the appropriate folder (`cheats` or `patches`).
2. Update the `CHEATS_JSON.txt` file with the corresponding entry for new cheats.
3. Make sure your files are formatted correctly and follow the examples provided, and have been properly tested.
