# Contributing

Thank you for helping improve the Greek postal code datasets. Contributions that
correct, add, or clarify data are welcome.

## Before You Start

- Check existing issues and pull requests to avoid duplicate work.
- Open an issue first for broad changes to the schema, coverage, or data sources.
- For a small factual correction, you can submit a pull request directly.
- Use a reliable source and include a link or citation in the issue or pull
  request. Do not include personal or otherwise sensitive data.

## Dataset Formats

Keep the existing JSON schemas unchanged unless a schema change has been
discussed first.

`greek_postal_codes.json` is an array of objects:

```json
{
  "place": "ΑΘΗΝΑ",
  "postal_code": "10431"
}
```

- `place` is a non-empty Greek place name.
- `postal_code` is a five-character string. Keep leading zeroes if present.
- Each `place` and `postal_code` pair must be unique.

`greek_streets_by_postcode.json` is an array of postcode objects:

```json
{
  "postal_code": "10431",
  "places": ["ΑΘΗΝΑ"],
  "streets": [
    {
      "street": "28ης Οκτωβρίου",
      "number_range": "1-11"
    }
  ]
}
```

- Each postcode must appear only once.
- `places` contains one or more non-empty place names.
- `streets` contains objects with a non-empty `street` and a string
  `number_range`. An empty number range is valid when no range is known.
- Preserve Greek spelling, accents, and capitalization as used by the
  surrounding records.

## Making Changes

1. Fork and clone the repository.
2. Create a focused branch, such as `fix-athens-postcode`.
3. Edit only the records relevant to the contribution.
4. Keep files as UTF-8 JSON with two-space indentation.
5. Preserve the existing record and key ordering. Avoid reformatting an entire
   dataset for a small change.
6. Update `README.md` if the documented record counts, coverage, or schema
   changes.

## Validation

First, confirm that both files contain valid JSON:

```sh
python3 -m json.tool greek_postal_codes.json >/dev/null
python3 -m json.tool greek_streets_by_postcode.json >/dev/null
```

Then run this schema and uniqueness check:

```sh
python3 - <<'PY'
import json
import re

with open("greek_postal_codes.json", encoding="utf-8") as f:
    postal_codes = json.load(f)
with open("greek_streets_by_postcode.json", encoding="utf-8") as f:
    street_groups = json.load(f)

postcode_pattern = re.compile(r"^\d{5}$")

pairs = []
for row in postal_codes:
    assert set(row) == {"place", "postal_code"}
    assert isinstance(row["place"], str) and row["place"].strip()
    assert postcode_pattern.fullmatch(row["postal_code"])
    pairs.append((row["place"], row["postal_code"]))
assert len(pairs) == len(set(pairs)), "duplicate place/postcode pair"

group_codes = []
for group in street_groups:
    assert set(group) == {"postal_code", "places", "streets"}
    assert postcode_pattern.fullmatch(group["postal_code"])
    assert isinstance(group["places"], list) and group["places"]
    assert all(isinstance(place, str) and place.strip() for place in group["places"])
    assert isinstance(group["streets"], list)
    for street in group["streets"]:
        assert set(street) == {"street", "number_range"}
        assert isinstance(street["street"], str) and street["street"].strip()
        assert isinstance(street["number_range"], str)
    group_codes.append(group["postal_code"])
assert len(group_codes) == len(set(group_codes)), "duplicate postcode group"

print("Validation passed")
PY
```

Review the diff before committing. Large unexpected changes usually indicate
that a formatter rewrote the dataset:

```sh
git diff --stat
git diff
```

## Pull Requests

Keep each pull request focused on one correction or related set of additions.
In the description, include:

- what changed and why;
- the source used to verify the data;
- any effect on coverage or the counts documented in `README.md`;
- confirmation that the validation commands pass.

By contributing, you agree that your changes may be distributed under the
repository's MIT License.
