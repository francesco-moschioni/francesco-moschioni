# Interactive exhibit integration

The main portfolio does not hardcode the publication status of individual
research exhibits. It fetches:

```text
/nyc-urban-mobility-public/interactives/manifest.json
```

and renders cards only for entries with `ready: true`.

This creates a deliberate three-stage workflow:

1. the private thesis repository generates aggregates from local data;
2. the public research repository receives the generated HTML/JSON and marks
   the corresponding manifest entries ready;
3. the portfolio automatically reveals the ready exhibits without requiring a
   second portfolio edit.

If the manifest is unavailable, malformed, or contains no ready entries, the
portfolio silently retains its existing layout. This prevents an incomplete
local build from creating broken public links.

The exhibit section is inserted after the headline findings and spatial
interpretation, and before the research-design section. The four cards form an
editorial sequence from the empirical mobility system and corridor choice set
to model evidence and policy evidence. The adjustment and policy cards receive
additional visual emphasis. Styling reuses the portfolio's paper, wine, gold,
serif and mono tokens.

