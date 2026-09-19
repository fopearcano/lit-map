# Literature Map: Research Charter and Decision Register

**Status:** Discussion draft  
**Last updated:** 2026-09-19

This document is the shared reference for product research, data design, and later
implementation. It deliberately contains no application design or code. Items marked
**Accepted** are constraints stated in the project brief; recommendations remain
**Proposed** until explicitly approved.

## 1. Accepted starting decisions

| ID | Decision | Status | Rationale |
| --- | --- | --- | --- |
| D-001 | The product is an interactive map of literature. | Accepted | This is the product brief. |
| D-002 | The near-term phase is discussion and research, not implementation. | Accepted | Premature implementation would encode unresolved editorial and data assumptions. |
| D-003 | Product, research, and data decisions must be recorded in the repository. | Accepted | A durable decision trail prevents later work from silently changing the model. |

No interpretation of “comprehensive,” “map,” or “literature” has yet been accepted.

## 2. The question the map should answer

“A comprehensive map of literature” is not a sufficiently testable objective. A single
view cannot optimize equally for discovery, historical explanation, bibliography, and
canon critique. The first product decision should be a primary user promise.

### Candidate promises

| Promise | Typical user question | Strength | Main failure mode |
| --- | --- | --- | --- |
| **Discovery** | “What should I read after this?” | Immediately useful and easy to test with users | Similarity can become a popularity loop or an unexplained black box |
| **Historical influence** | “How did this work or movement develop?” | Makes relationships meaningful and supports scholarship | Influence is contested, directional, and expensive to evidence |
| **Cultural orientation** | “What was being written across places and periods?” | Best match for a broad, plural map | Coverage gaps can masquerade as historical absence |
| **Bibliographic exploration** | “Which edition, translation, or adaptation is this?” | Precise and source-friendly | A catalog is not necessarily an engaging map |
| **Argument about canons** | “Who is visible, and who has been excluded?” | Makes selection and uncertainty part of the experience | Requires editorial framing and careful demographic claims |

**Proposed decision (P-001):** make the primary promise **cultural orientation and
discovery**, with historical influence available only where an explicit source supports
it. The map should help a person move from one work to nearby works and understand why
they are connected. It should not imply a universal ranking or a complete causal history.

## 3. What “literature” could include

Scope should be based on declared inclusion rules, not intuition. These dimensions are
independent and should not be collapsed into a single “literary/not literary” flag:

- **Form:** novels, short fiction, poetry, drama, oral literature, essays, memoir,
  graphic literature, children's literature, and hybrid forms.
- **Tradition:** written, performed, and oral works; sacred or philosophical texts when
  treated as literature; folklore and collectively authored traditions.
- **Publication state:** published, manuscript, serialized, performed, recorded, or
  transmitted orally.
- **Audience and market:** literary, popular, genre, young adult, children's, and other
  categories that overlap rather than form a hierarchy.
- **Geography and language:** place written, place first published or performed, setting,
  author affiliation, original language, and translation language are distinct facts.
- **Time:** composition, first performance, serialization, and publication dates can
  differ; uncertain and approximate dates must remain uncertain.

**Proposed minimum initial unit:** individually identifiable works of narrative fiction,
poetry, and drama, including works originating in oral traditions when a citable record
identifies them. Begin with a deliberately diverse sample rather than claiming global
completeness. Expand forms through explicit scope decisions.

### Assumptions to challenge

1. **More records means more comprehensive.** It may instead mean duplicating editions,
   over-representing well-catalogued languages, and importing institutional bias.
2. **A book is the obvious unit.** Users encounter works through editions, translations,
   performances, and adaptations. Conflating them corrupts dates, languages, and credits.
3. **Geography is a pin.** Nationality, residence, publication place, setting, and cultural
   tradition are neither interchangeable nor always singular.
4. **Genre is a taxonomy.** Genre labels are overlapping, time-bound, market-dependent,
   and disputed; they should allow multiple assertions and provenance.
5. **Influence follows similarity.** Textual or thematic similarity is not evidence that
   one author knew or influenced another.
6. **A neutral canon can be computed.** Availability, translation, digitization, citation,
   and sales all encode unequal access and institutional choices.
7. **One layout can be “the map.”** A geographic projection, timeline, and relationship
   graph answer different questions and introduce different distortions.
8. **An author has one identity or language.** Names, identities, languages, and cultural
   affiliations change and may be sensitive, uncertain, or externally imposed.

## 4. Structural options

### Option A: geographic atlas

Place works or people on a world map and filter by period, language, or form.

- **Good for:** regional exploration and revealing geographic coverage gaps.
- **Poor for:** diasporas, multilingual traditions, placeless works, and relationships.
- **Risk:** the interface makes modern borders and a single location look authoritative.

### Option B: chronological landscape

Use time as the primary axis, with lanes or clusters for traditions, languages, or forms.

- **Good for:** contemporaneity, movements, publication waves, and historical context.
- **Poor for:** uncertain dates, long oral histories, and rich many-to-many relationships.
- **Risk:** a single linear chronology can imply a universal progression.

### Option C: relationship graph

Represent works, people, movements, languages, and places as nodes joined by typed edges.

- **Good for:** browsing translations, adaptations, shared movements, and evidenced links.
- **Poor for:** orientation at scale; dense graphs quickly become unreadable.
- **Risk:** visually prominent nodes can be mistaken for the “most important” literature.

### Option D: embedding or similarity field

Place works according to machine-derived textual or metadata similarity.

- **Good for:** serendipitous discovery and finding cross-taxonomy affinities.
- **Poor for:** works without usable text, multilingual comparability, and explanation.
- **Risk:** copyright-constrained and digitized corpora determine what can be represented;
  distance looks objective even when model and corpus choices are editorial decisions.

### Option E: coordinated views over a shared knowledge graph

Model entities and sourced assertions once, then offer an atlas, timeline, and focused
neighborhood view. Search and filters coordinate the views; no global “hairball” is the
default.

- **Good for:** acknowledging that literature has spatial, temporal, linguistic, and
  relational dimensions without forcing them into one geometry.
- **Poor for:** simplicity of implementation and a single iconic overview.
- **Risk:** scope expands unless the first user journey is tightly constrained.

**Proposed decision (P-002):** use **Option E** as the conceptual structure. The initial
experience should be a work-centered neighborhood, supported by timeline and geographic
views. Similarity may later produce suggestions, but it must be labeled as computed and
must not be stored as historical influence.

## 5. Conceptual data boundaries

The research model should distinguish:

- **Work:** the abstract intellectual or artistic creation.
- **Expression:** a language version, translation, revision, or performance realization.
- **Manifestation:** a particular publication or release.
- **Item:** a specific physical or digital copy; normally out of product scope.
- **Agent:** a person, group, community, or organization with a typed and qualified role.
- **Place, language, period/movement, form/genre, and subject:** reference entities whose
  labels and hierarchies may be plural, historical, and contested.
- **Assertion:** a claim connecting entities, with source, method, confidence, contributor,
  and review state. Assertions—not bare edges—are the foundation of the map.

Candidate relationship types include `created by`, `translated by`, `version of`,
`adapted from`, `cites`, `reviewed alongside`, `associated with movement`, `set in`,
`published in`, `shares subject`, and `computed similar to`. Each relation needs its own
evidence rule. An umbrella `related to` edge should not be accepted into curated data.

## 6. Source requirements

### 6.1 Source classes and appropriate use

| Tier | Source class | Appropriate claims | Caveat |
| --- | --- | --- | --- |
| 1 | National libraries, archival finding aids, scholarly editions, original publication records | Identity, dates, edition history, credited roles | Catalog records can conflict or reproduce historical description |
| 2 | Peer-reviewed scholarship and specialist reference works | Influence, movement membership, reception, contested attribution | Interpretive claims must retain attribution and page-level citation |
| 3 | Maintained authority files and open bibliographic/knowledge datasets | Identifiers, aliases, baseline relationships, reconciliation | Aggregated claims still require provenance and may not share one license |
| 4 | Publisher, author, estate, or cultural-institution pages | Current descriptions and first-party context | Interested parties are not independent evidence for evaluative claims |
| 5 | Community encyclopedias, reviews, reading lists, and user contributions | Leads, discovery signals, explicitly labeled community opinion | Not sufficient alone for strong historical claims |
| 6 | Algorithmic inference | Similarity, clustering, candidate links | Must retain model, corpus, version, date, and score; never present as fact |

Tier is not a universal truth ranking. A community-maintained source may be the best
authority for its own tradition; a national catalog may be wrong about that tradition.
The correct source depends on the claim and whose knowledge is being represented.

### 6.2 Admission checklist

Before a source or dataset is ingested, record:

1. Stable name, owner, URL or identifier, access date, and version or snapshot date.
2. Exactly which fields or claims it supports and whether it is primary, secondary, or
   aggregated for those claims.
3. License and terms for data, text, images, and user-contributed content separately;
   attribution, share-alike, redistribution, and commercial-use obligations.
4. Retrieval method, rate limits, update cadence, expected persistence, and a reproducible
   snapshot or query where permitted.
5. Geographic, linguistic, chronological, and format coverage, including known omissions.
6. Record-linkage quality, identifier scheme, duplicate behavior, and error history.
7. Whether sensitive identity fields are self-described, inferred, historical, or disputed.
8. A removal/correction route and whether upstream changes can be detected and replayed.

“Publicly accessible” must never be treated as synonymous with “licensed for reuse.”
Bibliographic metadata, full text, cover art, portraits, reviews, and embeddings require
separate rights analysis.

### 6.3 Claim-level provenance minimum

Every curated or imported assertion should be able to answer:

- Who or what made the claim?
- Which source record and, for scholarship, which page or passage supports it?
- When was it retrieved, and which source version was used?
- Was it quoted, transformed, reconciled, inferred by a person, or computed by a model?
- What confidence and review state does it have?
- Are there competing assertions, and can the interface show them without erasing one?

## 7. Coverage and quality

“Comprehensive” should be redefined as **transparent, multi-dimensional coverage with a
documented expansion process**, not “every work ever created.” Suggested public metrics:

- works and sourced assertions by original language, period, region, form, and source;
- percentage with edition/translation separation rather than flattened book records;
- percentage of strong claims with claim-level citations;
- unresolved duplicates and conflicts;
- records relying on a single source;
- missingness shown as unknown, not converted to “none”; and
- correction turnaround and provenance completeness.

Raw counts should always appear beside the relevant denominator and collection scope.
Demographic coverage should be reported only when the categories, sourcing policy,
privacy implications, and “unknown” handling have been reviewed.

## 8. Proposed first research slice

Before choosing technology, build a hand-audited research set of roughly 100–200 works
that deliberately crosses languages, centuries, regions, forms, oral/written traditions,
translation histories, anonymous/collective authorship, and uncertain dates.

Use it to test three journeys:

1. Start with a known work and find three explainable, non-obvious connections.
2. Explore what was created across several regions and languages during a chosen period.
3. Follow one work through translations, editions, adaptations, and reception without
   confusing those entities.

This is a research sample, not an MVP database and not a claim of representativeness. Its
purpose is to expose failures in definitions and evidence rules before implementation.

## 9. Decisions needed next

The next discussion should resolve these in order:

1. **Primary audience and task:** curious general readers, students, educators,
   researchers, librarians, or another group; and which candidate promise leads.
2. **Scope rule:** included forms and traditions, plus explicit exclusions for the first
   research slice.
3. **Meaning of “comprehensive”:** target dimensions and acceptable coverage thresholds.
4. **Editorial stance:** whether the map primarily reflects established scholarship,
   community knowledge, user interpretation, or a visibly separated combination.
5. **Relationship policy:** which edge types may appear and the minimum evidence for each.
6. **Source and rights policy:** acceptable licenses, attribution requirements, and whether
   copyrighted text or images are necessary at all.
7. **Correction governance:** who can propose, review, contest, and retire assertions.

## 10. Decision-record process

When a proposal is approved, change its status to **Accepted**, record the date and
rationale, and note which earlier decision it supersedes. Meaningful reversals should be
preserved rather than silently overwritten. Research findings should link back to the
decision they inform; later schemas and features should cite the accepted decision that
justifies them.

