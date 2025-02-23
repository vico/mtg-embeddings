# mtg-embeddings

![](docs/3ab329bcc6d768373c843562745906bc.webp)

Code used to create

## How The Embeddings Were Created

Using the data exports from from [MTGJSON](https://mtgjson.com), the data was deduped to the most recent card printing, and aggregated as a prettified JSON object string (including indentation, which is relevant to embedding quality). An example string:

```txt
{
  "name": "Marhault Elsdragon",
  "manaCost": "{3}{R}{R}{G}",
  "type": "Legendary Creature — Elf Warrior",
  "text": "Rampage 1 (Whenever this creature becomes blocked, it gets +1/+1 until end of turn for each creature blocking it beyond the first.)",
  "power": "4",
  "toughness": "6",
  "rarities": [
    "uncommon"
  ],
  "sets": [
    "LEG",
    "BCHR",
    "CHR",
    "ME3"
  ]
}
```

This JSON string was then encoded and unit-normalized using the [Alibaba-NLP/gte-modernbert-base](https://huggingface.co/Alibaba-NLP/gte-modernbert-base) text embedding model.

JSON schema definitions:

- `name`: Card name
- `manaCost`: Card mana cost
- `type`: Card type (full textline)
- `text`: Gatherer card text. Exact matches to card names are replaced with `~` for better similarities.
- `power`: Card power. Omitted if N/A.
- `toughness`: Card toughness. Omitted if N/A.
- `loyalty`: Card loyalty. Omitted if N/A.
- `rarities`: List of all card rarities for all printings of the card. In order: `["common", "uncommon", "rare", "mythic", "bonus", "special"]`
- `sets`: List of all sets the card was printed in. Sets are roughly in chronological order of printing. Basic lands have a special case of `[*]`.

It took 1:17 to encode all ~33k cards using a preemptible L4 GPU on Google Cloud Platform (`g2-standard-4`), costing < $0.01.

## Maintainer/Creator

Max Woolf ([@minimaxir](https://minimaxir.com))

_Max's open-source projects are supported by his [Patreon](https://www.patreon.com/minimaxir) and [GitHub Sponsors](https://github.com/sponsors/minimaxir). If you found this project helpful, any monetary contributions to the Patreon are appreciated and will be put to good creative use._

## License

MIT
