# Where prediction stops

An interactive companion to the poster *Prediction of PAH Degradation in
Bioremediation Research: A Rule-Based Cheminformatics Analysis*.

Open it here: **https://bioremscope.github.io/OffenburgMeeting/**

## What it shows

In silico biotransformation prediction aims to name the first enzymatic attack
on a pollutant, the intermediates that follow, and the organisms that could
carry the reaction out. Polycyclic aromatic hydrocarbons are a hard case for
this. The page lets you run that chain yourself, on any of the 16 EPA priority
PAHs, and see where it holds and where it stops.

One gesture: **tap a node to open it, tap it again to close it.**

1. Sixteen compounds around a hub. Tap one.
2. Its predicted first-attack products bloom. Pale pink means nothing
   recognises the structure; salmon means a public pathway database already
   curates it.
3. Tap a product. Its own next-generation products appear, and where the
   structure is recognised, so do the enzymes that consume it — with the gene
   symbols the field knows them by, such as *nahB* and *nahC*.
4. Tap an enzyme to see the sequenced genera that carry it.
5. Tap a pale product and nothing opens. That is the finding, not a fault.

The **Lab degraders** button adds a separate ring: the genera that curated
primary literature reports degrading the compound itself. That evidence comes
from sources that never saw the pathway databases, so it is drawn as its own
route rather than as part of the chain.

## What it measures

Across the 16 compounds the rule engine proposes **306 transformations**. Public
pathway annotation recognises **26** of the resulting products, and every one of
them belongs to the **6** compounds already curated in KEGG. The other **10
PAHs yield 215 products and no recognitions at all** — so for those, no enzyme
and no genome can be reached from the chemistry, and the only organism evidence
left is observational.

Benzo[b]fluoranthene is the clearest case: 36 predicted products, none
recognised, and 7 genera known only from laboratory reports.

## Colours

The palette is the one used for the underlying Neo4j knowledge graph, so the
graph shown at the poster and the graph on your phone are visibly the same
thing.

| | Colour | Meaning |
|---|---|---|
| PAH | `#D64545` | the compound you picked |
| Predicted metabolite | `#F2A9A0` | proposed by the rules; unrecognised |
| KEGG compound | `#E8837A` | public annotation curates this structure |
| Enzyme | `#9575B8` | what consumes it, and what catalyses that |
| Genus (genome) | `#57A773` | carries the enzyme — mechanism |
| Genus (lab) | `#8FC99C` | observed degrading the compound — phenotype |

Saturation carries the argument: pale turns salmon only where annotation
confirms the structure.

## Sources

- Structures and identifiers: PubChem, ChEBI.
- Pathways, reactions and enzymes: KEGG maps map00624 and map00626.
- Genome carriage: KEGG Orthology.
- First-attack rules: re-derived from EAWAG-BBD chemistry and applied offline
  with RDKit. The rules reproduce that chemistry; the SMIRKS are not copied.
- Degrading organisms: curated from primary literature.

A predicted structure is a hypothesis about where a molecule can be attacked. It
is not a claim that the compound is degraded, nor that any named organism
degrades it. Genome carriage means a sequenced genome of that genus carries the
gene family; it does not predict removal.

## Reading it honestly

- Counts shown on screen are the true totals. Where the drawing is capped for
  legibility on a phone, the hidden remainder is stated on the node that carries
  it, and the status line under the graph always reports the full figures.
- Products that lead somewhere are drawn first. That is display order only; no
  count changes and nothing is omitted.
- A genus may appear under two enzymes. It carries both.
- Enzymes classified only to a sub-subclass (a partial EC number) match no
  genome, and the page says so rather than leaving the tap silent.

## Running it

Static files, no build step. The page fetches its data, so it needs to be
served over HTTP rather than opened from the filesystem:

```bash
python -m http.server 8777
```

Then open `http://localhost:8777/`.

`data/index.json` carries the 16 compounds and loads first; `data/pah/<slug>.json`
carries one compound's full chain and is fetched when you tap it. The data is
split that way so the first paint does not wait on three hundred structure
drawings over conference wifi.

Structure drawings are generated with RDKit and inlined as SVG. Cytoscape.js is
the only external dependency and loads from cdnjs.

## Snapshot

The data is a dated snapshot of literature corpora that are still being
extracted, so counts will grow. The snapshot date and commit are shown in the
page's own footer data.

---

Ahmet Yazıcıoğlu · University of Warmia and Mazury in Olsztyn
