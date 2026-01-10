## Endpoint

https://cdn.jsdelivr.net/gh/quantNebula/data@master/shiny.min.json

## JSON Example

```json
{
    "dexNumber": 1,
    "name": "Bulbasaur",
    "releasedDate": "2018/03/25",
    "family": "Bulbasaur",
    "typeCode": null,
    "forms": [
        {
            "name": "f19",
            "imageUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pm0001_00_pgo_fall2019_shiny.png",
            "width": 256,
            "height": 256
        }
    ],
    "imageUrl": "https://cdn.jsdelivr.net/gh/PokeMiners/pogo_assets/Images/Pokemon%20-%20256x256/pokemon_icon_001_00_shiny.png",
    "width": 256,
    "height": 256
}
```

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `dexNumber` | integer | National Pokédex number |
| `name` | string | Pokémon name, may include regional variants (e.g., "Hisuian Venusaur") |
| `releasedDate` | string | Date shiny was released (YYYY/MM/DD format) |
| `family` | string | Evolution family identifier, may include form suffix (e.g., "Bulbasaur_11") |
| `typeCode` | string\|null | Form type code (e.g., "_51" for Hisuian, "_52" for Paldean), null for standard forms |
| `forms` | array | Available costume/form variants |
| `forms[].name` | string | Form identifier (e.g., "f19" for Fall 2019, "11" for costume, "M" for Mega) |
| `forms[].imageUrl` | string | Shiny form icon URL |
| `forms[].width` | integer | Icon width in pixels |
| `forms[].height` | integer | Icon height in pixels |
| `imageUrl` | string | Default shiny form icon URL |
| `width` | integer | Default icon width in pixels |
| `height` | integer | Default icon height in pixels |

## Notes

The data is an array of shiny Pokémon objects cataloging all released shiny forms. Regional variants and different forms of the same species share the same dex number but are separate entries.
