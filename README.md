# Greek Postal Codes

Open JSON datasets mapping Greek cities to postal codes, with optional street-level detail. Built for developers who need reliable place/postcode data in Greek applications.

## Files

| File | Description |
|---|---|
| [`greek_postal_codes.json`](greek_postal_codes.json) | Greek place names and their postal codes |
| [`greek_streets_by_postcode.json`](greek_streets_by_postcode.json) | Streets and number ranges grouped by postcode |

### Dataset size

- **604** place/postcode records
- **187** unique place names
- **594** unique postcodes
- **60,118** street entries

## Usage

### City and postcode lookup

Use `greek_postal_codes.json` for forms, validation, shipping, and autocomplete.

```json
{
  "place": "ΑΘΗΝΑ",
  "postal_code": "10431"
}
```

- One object per `place` + `postal_code` pair.
- A city may appear multiple times when it has more than one postcode.
- A postcode may be linked to one or more place names.

### Street detail

Use `greek_streets_by_postcode.json` when you need address-level data. Join on `postal_code`.

## Coverage

The dataset covers major Greek cities and urban areas (population 10,000+). It is intended as a practical developer reference, not an exhaustive catalogue of every settlement in Greece.

## Sources

This dataset was compiled from publicly available sources and manually verified where possible.

If you believe any source attribution is missing or incorrect, please open an issue.

## License

Data is provided as-is for open use. Verify critical addresses against official sources before production use.
