# Adapting `changelog-release` for spec/schema repositories (e.g. GKS products)

The base skill assumes a code API contract. When the "contract" is a **spec + schema**
(VA-Spec, Cat-VRS style products), three substitutions make it fit:

## 1. Contract source: read the schema diff, not the PR titles

Step 2 of the base workflow ("decide the version by contract math") changes its evidence
source. For a spec repo:

- MAJOR: schema elements removed/renamed, validation tightened, requirement level
  upgraded (MAY → SHOULD → MUST), or term semantics redefined in the spec prose
- MINOR: additive schema elements, new optional fields, new allowed values in enums
- PATCH: clarifications that don't change what a conformant implementation must do

The classification step should run on `git diff <last>..HEAD -- '*schema*' '*spec*'`
first, and only fall back to PR titles for changes with no schema/spec footprint.

## 2. Grounding: every release-note claim cites its PR

Add a hard rule to the notes-formatting step: each bullet ends with `(#123)` for the PR
that introduced it. Acceptability review then becomes a lookup instead of a re-read —
which is the difference between the 90-minute manual pass and a reviewable draft.

## 3. Audience split: implementers vs consumers

Spec products have two note audiences, so the draft gets two sections:

- **For implementers** — conformance-relevant changes (what validators/consumers must update)
- **For product users** — capability changes at the product level

Keeping them separate prevents the classic spec-release failure where prose about
conformance detail buries the one thing a downstream product team needs to act on.

---

## Suggested think-tank agenda (if starting from this skill)

1. Paste the last release's merged PR list into a Claude Code session with this skill
   installed; compare its draft against the human 90-minute version
2. Decide acceptability criteria: is citation-complete + impact-ordered enough?
3. Codify the deltas above into your own repo-owned skill (fork freely — MIT)
