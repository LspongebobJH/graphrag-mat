# AI4Materials Knowledge-Graph Projects: Concrete Content Guide

<details>
<summary>1. MatKG</summary>

**Paper:** [MatKG paper](https://doi.org/10.1038/s41597-024-03039-z)  
**Code/data:** [GitHub implementation](https://github.com/olivettigroup/MatKG) · [Zenodo data](https://zenodo.org/records/10022727)

**Focus:** A broad literature knowledge graph for materials science.

**Typical contents:**

```text
Paper
  ├─ reports ───────> Material
  ├─ studies ───────> Structure
  ├─ measures ──────> Property
  └─ uses ──────────> Synthesis method
```

**Schema:**

- **Nodes:** `Paper`, `Material`, `Structure`, `Property`, `SynthesisMethod`, `Application`, `EntityIdentifier`
- **Relations:** `Paper --reports--> Material`; `Paper --studies--> Structure`; `Paper --measures--> Property`; `Paper --uses--> SynthesisMethod`; `Material --used_in--> Application`; `Entity --linked_to--> EntityIdentifier`

**Likely queries:**

```text
Which materials have a band gap near a target value?
Which synthesis methods were used for a material?
```

</details>

<details>
<summary>2. Materials Knowledge Graph via LLM</summary>

**Paper:** [Materials Knowledge Graph via LLM](https://arxiv.org/abs/2404.03080)  
**Code/data:** [Code repository](https://github.com/MasterAI-EAM/Material-Knowledge-Graph)

**Focus:** LLM-assisted extraction of materials entities and relations from energy-materials abstracts.

**Typical contents:**

```text
MAPbI3
  ├─ used_in ────────> Perovskite solar cell
  ├─ has_property ───> Band gap: 1.6 eV
  ├─ processed_by ───> Spin coating
  └─ described_in ───> Abstract and DOI
```

**Schema:**

- **Nodes:** `Material`, `ChemicalFormula`, `PropertyValue`, `Application`, `Process`, `Paper`, `Abstract`, `Claim`
- **Relations:** `Material --has_formula--> ChemicalFormula`; `Material --has_property--> PropertyValue`; `Material --processed_by--> Process`; `Material --used_in--> Application`; `Material --described_in--> Abstract`; `Claim --supported_by--> Paper`

**Likely queries:**

```text
Which processes were used for a material?
Which paper supports a material-property claim?
```

</details>

<details>
<summary>3. Propnet</summary>

**Paper:** [Propnet paper](https://doi.org/10.1016/j.matt.2019.11.013)  
**Code:** [Propnet Python implementation](https://github.com/materialsintelligence/propnet)

**Focus:** A unit-aware graph of material properties, quantities, and property models.

**Typical contents:**

```text
Material: Silicon
  ├─ has_mass ────────> 5.0 g
  ├─ has_volume ──────> 2.0 cm^3
  └─ has_property ────> Density: 2.5 g/cm^3
                           └─ generated_by ─> density = mass / volume
```

**Schema:**

- **Nodes:** `Material`, `Quantity`, `Unit`, `Property`, `PropertyModel`, `Calculation`, `DataSource`
- **Relations:** `Material --has_mass/has_volume--> Quantity`; `Quantity --has_unit--> Unit`; `Material --has_property--> Property`; `Property --derived_from--> Quantity`; `Property --generated_by--> PropertyModel`; `PropertyModel --uses_input--> Quantity`

**Likely queries:**

```text
Which values were used to derive density?
Are the units compatible for this calculation?
```

</details>

<details>
<summary>4. Materials Experiment Knowledge Graph (MEKG)</summary>

**Paper:** [MEKG paper](https://doi.org/10.1039/D3DD00067B)  
**Code/data:** [MEKG migrations and database tooling](https://github.com/modelyst/mekg-migrations)

**Focus:** Experimental samples, instruments, procedures, measurements, workflows, and provenance.

**Typical contents:**

```text
Experiment: E-2026-014
  ├─ uses_sample ─────> Sample: S-001
  ├─ performs_step ───> Annealing at 800 C for 2 h
  ├─ uses_instrument ─> X-ray diffractometer
  └─ produces ────────> Phase-composition measurement
```

**Schema:**

- **Nodes:** `Experiment`, `Sample`, `Material`, `ProcessStep`, `Procedure`, `Instrument`, `Measurement`, `Property`, `Dataset`
- **Relations:** `Experiment --uses_sample--> Sample`; `Sample --composed_of--> Material`; `Experiment --performs_step--> ProcessStep`; `Experiment --uses_instrument--> Instrument`; `Experiment --produces--> Measurement`; `Measurement --measures--> Property`; `Experiment --part_of--> Dataset`

**Likely queries:**

```text
Which heat treatment produced a measurement?
Which instrument characterized a sample?
```

</details>

<details>
<summary>5. KGQA4MAT / MOF-KG</summary>

**Paper:** [KGQA4MAT / MOF-KG paper](https://arxiv.org/abs/2309.11361)  
**Code/data:** [MOF-KG notebooks and data](https://github.com/anyuanay/MOF_KG_LAMA)

**Focus:** Metal-organic frameworks, chemical features, properties, and natural-language-to-query evaluation.

**Typical contents:**

```text
MOF: UiO-66
  ├─ has_metal_node ─> Zirconium cluster
  ├─ has_linker ────> Terephthalate
  ├─ has_property ──> Surface area: 1,245 m^2/g
  └─ has_property ──> Water stability: stable
```

**Schema:**

- **Nodes:** `MOF`, `MetalNode`, `MetalIon`, `Linker`, `ChemicalComponent`, `SynthesisProcedure`, `Property`, `PropertyValue`, `Paper`, `Question`, `FormalQuery`, `Answer`
- **Relations:** `MOF --has_metal_node--> MetalNode`; `MOF --has_linker--> Linker`; `MOF --has_component--> ChemicalComponent`; `MOF --synthesized_by--> SynthesisProcedure`; `MOF --has_property--> PropertyValue`; `Paper --describes--> MOF`; `Question --translated_to--> FormalQuery`; `FormalQuery --returns--> Answer`

**Likely queries:**

```text
Which MOFs contain a linker and satisfy property constraints?
Can a natural-language question be translated into a formal query?
```

</details>

<details>
<summary>6. NR-KG</summary>

**Paper:** [NR-KG paper](https://doi.org/10.1016/j.asoc.2025.113776)  
**Code/data:** [NR-KG implementation and datasets](https://github.com/MatrixBrain/NR-KG)

**Focus:** High-entropy-alloy composition, graph neighborhoods, numerical properties, and prediction.

**Typical contents:**

```text
Alloy: Fe-Co-Ni-Cr
  ├─ contains ───────> Element nodes
  ├─ has_property ───> Measured yield strength: 920 MPa
  └─ predicted_by ───> Graph model: 900 MPa
```

**Schema:**

- **Nodes:** `Alloy`, `Element`, `Composition`, `MeasuredProperty`, `PredictedProperty`, `Model`, `Dataset`, `Feature`
- **Relations:** `Alloy --contains--> Element`; `Alloy --has_composition--> Composition`; `Alloy --has_measured_property--> MeasuredProperty`; `Alloy --has_predicted_property--> PredictedProperty`; `PredictedProperty --predicted_by--> Model`; `Alloy --has_feature--> Feature`; `Property --recorded_in--> Dataset`

**Likely queries:**

```text
Which graph neighbors support an alloy prediction?
How do measured and predicted values differ?
```

</details>

<details>
<summary>7. SolarGraph AI</summary>

**Project:** [SolarGraph AI repository](https://github.com/marblehub/solargraph-ai)

**Focus:** Photovoltaic materials and devices represented as an RDF/OWL graph for grounded GraphRAG.

**Typical contents:**

```text
MAPbI3
  ├─ used_in ────────> Perovskite solar cell
  ├─ has_property ───> Band gap: 1.6 eV
  ├─ supported_by ────> Paper
  └─ has_provenance ──> Extraction activity
```

**Schema:**

- **Nodes:** `PhotovoltaicMaterial`, `Device`, `Property`, `QuantityValue`, `Unit`, `Paper`, `Claim`, `ExtractionActivity`, `Agent`, `Dataset`
- **Relations:** `Material --used_in--> Device`; `Material --has_property--> QuantityValue`; `QuantityValue --has_unit--> Unit`; `Claim --supported_by--> Paper`; `Claim --generated_by--> ExtractionActivity`; `ExtractionActivity --associated_with--> Agent`; `Entity --derived_from--> Dataset`

**Likely queries:**

```text
Why is a material relevant to a photovoltaic device?
Which triples and sources support the answer?
```

</details>

<details>
<summary>8. MMKG</summary>

**Paper:** [MMKG paper](https://doi.org/10.1016/j.cpc.2016.07.005)

**Focus:** Metallic-materials graph assembled from DBpedia and Wikipedia, connecting material entities with elemental, compositional, and descriptive facts.

**Typical contents:**

```text
Metallic material
  ├─ has_composition ─> Element or alloy composition
  ├─ has_property ────> Materials property
  └─ described_in ────> Linked-data source
```

**Schema:**

- **Nodes:** `MetallicMaterial`, `Metal`, `Element`, `Alloy`, `Composition`, `Property`, `WikipediaPage`, `DBpediaResource`, `ExternalIdentifier`
- **Relations:** `Material --has_composition--> Composition`; `Composition --contains_element--> Element`; `Alloy --contains_metal--> Metal`; `Material --has_property--> Property`; `Material --described_in--> WikipediaPage`; `Material --represented_by--> DBpediaResource`; `Resource --same_as--> ExternalIdentifier`

**Likely queries:**

```text
Which metallic materials contain a selected element?
Which source describes a material property?
```

</details>

<details>
<summary>9. NanoMine</summary>

**Paper:** [NanoMine paper](https://doi.org/10.1007/978-3-030-62466-8_10)

**Focus:** Ontology and knowledge graph for nanocomposite components, processing workflows, experiments, and measured properties.

**Typical contents:**

```text
Nanocomposite
  ├─ has_component ─> Matrix or nanofiller
  ├─ processed_by ──> Processing step
  └─ has_property ──> Measured behavior
```

**Schema:**

- **Nodes:** `Nanocomposite`, `Matrix`, `Nanofiller`, `Component`, `Formulation`, `ProcessingStep`, `Experiment`, `Measurement`, `Property`, `Condition`, `DataSource`
- **Relations:** `Nanocomposite --has_matrix--> Matrix`; `Nanocomposite --has_filler--> Nanofiller`; `Nanocomposite --has_component--> Component`; `Nanocomposite --processed_by--> ProcessingStep`; `ProcessingStep --has_condition--> Condition`; `Experiment --uses_sample--> Nanocomposite`; `Experiment --produces--> Measurement`; `Measurement --measures--> Property`

**Likely queries:**

```text
Which processing conditions produced a target property?
Which nanocomposites contain a selected filler?
```

</details>

<details>
<summary>10. Knowledge Graph-Empowered Materials Discovery</summary>

**Paper:** [Knowledge Graph-Empowered Materials Discovery](https://doi.org/10.1109/bigdata52589.2021.9671503)

**Focus:** Graph-supported materials discovery and recommendation.

**Typical contents:**

```text
Candidate material
  ├─ has_descriptor ─> Composition or feature
  ├─ related_to ────> Neighboring material
  └─ supported_by ──> Discovery evidence
```

**Schema:**

- **Nodes:** `CandidateMaterial`, `TargetMaterial`, `Composition`, `Descriptor`, `Feature`, `RelatedMaterial`, `Recommendation`, `DiscoveryEvidence`, `Source`
- **Relations:** `Material --has_composition--> Composition`; `Material --has_descriptor--> Descriptor`; `Material --has_feature--> Feature`; `Material --related_to--> RelatedMaterial`; `Recommendation --concerns--> CandidateMaterial`; `Recommendation --supported_by--> DiscoveryEvidence`; `Evidence --sourced_from--> Source`

**Likely queries:**

```text
Which candidates are related to a target material?
Which evidence supports a recommendation?
```

</details>

<details>
<summary>11. Semantic KG for Li-Ion Battery Cathodes</summary>

**Paper:** [Battery-cathode KG paper](https://doi.org/10.1002/adfm.202201437)

**Focus:** Semantic links among cathode materials, synthesis procedures, characterization methods, and electrochemical properties.

**Typical contents:**

```text
Cathode material
  ├─ synthesized_by ──> Synthesis procedure
  ├─ characterized_by -> Characterization method
  └─ has_property ────> Electrochemical property
```

**Schema:**

- **Nodes:** `CathodeMaterial`, `Composition`, `SynthesisProcedure`, `ProcessingCondition`, `CharacterizationMethod`, `Measurement`, `ElectrochemicalProperty`, `PropertyValue`, `Paper`
- **Relations:** `Cathode --has_composition--> Composition`; `Cathode --synthesized_by--> SynthesisProcedure`; `SynthesisProcedure --has_condition--> ProcessingCondition`; `Cathode --characterized_by--> CharacterizationMethod`; `CharacterizationMethod --produces--> Measurement`; `Cathode --has_property--> PropertyValue`; `Claim --supported_by--> Paper`

**Likely queries:**

```text
Which synthesis methods were used for a cathode?
Which cathodes meet a performance condition?
```

</details>

<details>
<summary>12. Compound KG-Enabled AI Assistant</summary>

**Paper:** [Compound KG assistant paper](https://doi.org/10.1007/s40192-022-00286-z)

**Focus:** Compound knowledge exposed through an AI assistant for discovery questions.

**Typical contents:**

```text
Compound
  ├─ has_property ─> Property value
  ├─ related_to ───> Material or compound
  └─ described_in ─> Literature record
```

**Schema:**

- **Nodes:** `Compound`, `Material`, `ChemicalStructure`, `Property`, `PropertyValue`, `RelationClaim`, `Paper`, `Question`, `Answer`
- **Relations:** `Compound --has_structure--> ChemicalStructure`; `Compound --has_property--> PropertyValue`; `Compound --related_to--> Material`; `RelationClaim --supported_by--> Paper`; `Paper --describes--> Compound`; `Question --retrieves--> Answer`; `Answer --based_on--> RelationClaim`

**Likely queries:**

```text
Which compounds have a desired property?
What literature supports a compound-material relation?
```

</details>

<details>
<summary>13. Materials Terminology KG</summary>

**Paper:** [Materials terminology KG paper](https://doi.org/10.1038/s41597-024-03448-0)  
**Local article:** [Materials terminology KG](paper_html/materials-terminology-kg.html)

**Focus:** Automatically constructed terminology and concept graph for materials science.

**Typical contents:**

```text
Materials concept
  ├─ synonym_of ─────> Related term
  ├─ related_to ─────> Scientific concept
  └─ extracted_from ─> Text corpus
```

**Schema:**

- **Nodes:** `MaterialsConcept`, `Term`, `Synonym`, `RelatedConcept`, `TextPassage`, `Corpus`, `Language`
- **Relations:** `Term --denotes--> MaterialsConcept`; `Term --synonym_of--> Term`; `Concept --related_to--> RelatedConcept`; `Term --extracted_from--> TextPassage`; `TextPassage --part_of--> Corpus`; `Term --has_language--> Language`

**Likely queries:**

```text
Which terms are related to a concept?
Which source passages mention a selected term?
```

</details>

<details>
<summary>14. Knowledge Graph Representation of Zeolitic Materials</summary>

**Paper:** [Zeolitic materials KG paper](https://doi.org/10.1039/d4dd00166d)  
**Repository article:** [Zeolitic KG repository](paper_html/zeolitic-kg-repository.html)

**Focus:** Crystal, zeolite, guest-species, and structure information with LLM-based QA.

**Typical contents:**

```text
Zeolitic material
  ├─ has_framework ─> Crystal framework
  ├─ contains_guest -> Guest species
  └─ has_property ──> Structural property
```

**Schema:**

- **Nodes:** `ZeoliticMaterial`, `Zeolite`, `CrystalStructure`, `Framework`, `GuestSpecies`, `ChemicalSpecies`, `Property`, `PropertyValue`, `Paper`, `EvidencePassage`
- **Relations:** `Zeolite --has_framework--> Framework`; `Material --has_crystal_structure--> CrystalStructure`; `Material --contains_guest--> GuestSpecies`; `Material --has_property--> PropertyValue`; `Material --described_in--> Paper`; `Claim --supported_by--> EvidencePassage`

**Likely queries:**

```text
Which guest species occur in a framework?
Which structural facts support the answer?
```

</details>

<details>
<summary>15. Upper-Level Ontologies for Materials KGs</summary>

**Paper:** [Upper-level ontology evaluation](https://doi.org/10.1002/adem.202401534)

**Focus:** Evaluation of BFO, EMMO, and PROVO for materials-science ontologies and KGs.

**Typical contents:**

```text
Materials entity
  ├─ participates_in -> Process
  ├─ observed_by ────> Measurement activity
  └─ supported_by ────> Provenance record
```

**Schema:**

- **Nodes:** `MaterialEntity`, `Process`, `Measurement`, `Observation`, `ProvenanceRecord`, `Ontology`, `OntologyClass`, `OntologyMapping`
- **Relations:** `Entity --participates_in--> Process`; `Entity --observed_by--> Observation`; `Observation --has_measurement--> Measurement`; `Claim --supported_by--> ProvenanceRecord`; `Entity --classified_as--> OntologyClass`; `OntologyClass --mapped_to--> OntologyClass`

**Likely queries:**

```text
Which ontology best represents a process or observation?
How should provenance be typed in a materials KG?
```

</details>

<details>
<summary>16. LLM Extraction from Materials Tables</summary>

**Paper:** [Materials table extraction paper](https://doi.org/10.1039/d4dd00362d)

**Focus:** Extraction of structured materials knowledge from scientific tables.

**Typical contents:**

```text
Table row
  ├─ identifies ──────> Material
  ├─ reports ─────────> Property value and unit
  ├─ uses_condition ──> Experimental condition
  └─ located_in ──────> Table and row provenance
```

**Schema:**

- **Nodes:** `Table`, `TableRow`, `TableColumn`, `Material`, `Property`, `PropertyValue`, `Unit`, `ExperimentalCondition`, `Paper`, `EvidenceCell`, `ExtractionActivity`
- **Relations:** `Table --contains_row--> TableRow`; `Table --contains_column--> TableColumn`; `TableRow --identifies--> Material`; `TableRow --reports--> PropertyValue`; `PropertyValue --has_unit--> Unit`; `TableRow --uses_condition--> ExperimentalCondition`; `EvidenceCell --located_in--> Table`; `EvidenceCell --supports--> PropertyValue`

**Likely queries:**

```text
Which materials have a property above a threshold?
Which row and conditions support a value?
```

</details>

<details>
<summary>17. Framework Material KG</summary>

**Paper:** [Framework material KG paper](https://doi.org/10.1038/s41524-025-01540-6)  
**Local article:** [Framework material KG](paper_html/framework-material-kg.html)

**Focus:** LLM-enabled knowledge graph construction for framework materials.

**Typical contents:**

```text
Framework material
  ├─ has_composition ─> Chemical composition
  ├─ synthesized_by ─> Synthesis method
  ├─ characterized_by -> Characterization method
  └─ used_for ───────> Application
```

**Schema:**

- **Nodes:** `FrameworkMaterial`, `Composition`, `Element`, `Structure`, `SynthesisMethod`, `CharacterizationMethod`, `Property`, `Application`, `Paper`, `Claim`, `EvidencePassage`
- **Relations:** `Material --has_composition--> Composition`; `Composition --contains_element--> Element`; `Material --has_structure--> Structure`; `Material --synthesized_by--> SynthesisMethod`; `Material --characterized_by--> CharacterizationMethod`; `Material --has_property--> Property`; `Material --used_for--> Application`; `Claim --supported_by--> EvidencePassage`

**Likely queries:**

```text
Which methods produce a framework material?
Which applications are associated with a framework structure?
```

</details>

<details>
<summary>18. CLASS MATE</summary>

**Paper:** [CLASS MATE chapter](https://doi.org/10.1007/978-3-031-78952-6_42)

**Focus:** Cross-lingual semantic search for materials-science information using graph relations.

**Typical contents:**

```text
Materials concept in language A
  ├─ equivalent_to ─> Concept in language B
  └─ linked_to ────> Materials-science document
```

**Schema:**

- **Nodes:** `MaterialsConcept`, `Term`, `Language`, `EquivalentConcept`, `Document`, `Passage`, `SemanticRepresentation`
- **Relations:** `Term --expresses--> MaterialsConcept`; `Concept --has_language--> Language`; `Concept --equivalent_to--> EquivalentConcept`; `Concept --linked_to--> Document`; `Document --contains--> Passage`; `Concept --represented_by--> SemanticRepresentation`; `Term --translated_as--> Term`

**Likely queries:**

```text
Which documents in another language describe this concept?
Which terms are equivalent across languages?
```

</details>

<details>
<summary>19. Materials Property KG: NIMS Case Study</summary>

**Paper:** [NIMS materials-property KG paper](https://doi.org/10.3390/app151910511)  
**Repository article:** [NIMS repository article](paper_html/materials-property-kg-nims.html)

**Focus:** Materials-property relationships for search and prediction.

**Typical contents:**

```text
Material
  ├─ has_property ─> Measured property value
  ├─ related_to ───> Property or material
  └─ described_in ─> Source record
```

**Schema:**

- **Nodes:** `Material`, `Composition`, `Property`, `MeasuredValue`, `PredictedValue`, `Unit`, `ExperimentalCondition`, `PredictionModel`, `SourceRecord`, `Claim`
- **Relations:** `Material --has_composition--> Composition`; `Material --has_measured_property--> MeasuredValue`; `Material --has_predicted_property--> PredictedValue`; `Value --has_unit--> Unit`; `Measurement --has_condition--> ExperimentalCondition`; `PredictedValue --predicted_by--> PredictionModel`; `Claim --described_in--> SourceRecord`

**Likely queries:**

```text
Which materials satisfy a property constraint?
Which source supports a material-property relation?
```

</details>

<details>
<summary>20. LLM-Driven Knowledge Graphs for Materials Science</summary>

**Paper:** [LLM-driven materials KG paper](https://doi.org/10.55056/ceur-ws.org/vol-4053/s_7_kaverinskiy_palagin_nikitiuk_litvin_malakhov.pdf)  
**Local article:** [LLM-driven materials KG](paper_html/llm-driven-materials-kg.html)

**Focus:** LLM-based graph creation and natural-language querying for materials research.

**Typical contents:**

```text
Research paper
  ├─ mentions ─────> Material or process
  ├─ reports ──────> Property or result
  └─ supports ─────> Normalized graph claim
```

**Schema:**

- **Nodes:** `ResearchPaper`, `Passage`, `Material`, `Process`, `Property`, `Result`, `Claim`, `NormalizedEntity`, `EvidenceRecord`
- **Relations:** `Paper --contains--> Passage`; `Paper --mentions--> Material`; `Paper --reports--> Result`; `Material --associated_with--> Process`; `Material --has_property--> Property`; `Claim --supports--> NormalizedEntity`; `Claim --evidenced_by--> Passage`; `EntityMention --resolved_to--> NormalizedEntity`

**Likely queries:**

```text
Which papers report a material-process relation?
What graph evidence supports an answer?
```

</details>

<details>
<summary>21. Materials Literature KG Reconstruction</summary>

**Paper:** [Materials literature KG paper](https://doi.org/10.1038/s43246-025-01043-3)  
**Local article:** [Materials literature LLM](paper_html/materials-literature-llm.html)

**Focus:** Extraction and reconstruction of structured knowledge from materials literature.

**Typical contents:**

```text
Literature mention
  ├─ resolved_to ───> Canonical material
  ├─ linked_to ─────> Property or process
  └─ supported_by ──> Publication passage
```

**Schema:**

- **Nodes:** `LiteratureMention`, `CanonicalMaterial`, `Property`, `Process`, `Condition`, `Publication`, `Passage`, `ReconstructedClaim`, `EntityResolutionRecord`
- **Relations:** `Mention --resolved_to--> CanonicalMaterial`; `Mention --linked_to--> Property`; `Mention --linked_to--> Process`; `Claim --concerns--> CanonicalMaterial`; `Claim --has_condition--> Condition`; `Claim --supported_by--> Passage`; `Passage --located_in--> Publication`

**Likely queries:**

```text
Which passages support a reconstructed relation?
Which mentions resolve to the same entity?
```

</details>

<details>
<summary>22. ChemRxiv Materials Literature KG</summary>

**Paper:** [ChemRxiv preprint](https://doi.org/10.26434/chemrxiv.10001546/v1)

**Focus:** Materials-literature KG construction using advanced preprocessing and LLM extraction.

**Typical contents:**

```text
Preprocessed paper
  ├─ yields ────────> Entity and relation mentions
  ├─ normalized_to ─> Canonical graph nodes
  └─ sourced_from ──> Preprint text
```

**Schema:**

- **Nodes:** `PreprocessedPaper`, `TextSegment`, `EntityMention`, `CanonicalEntity`, `RelationMention`, `CanonicalRelation`, `Material`, `Property`, `Process`, `Preprint`, `ExtractionRecord`
- **Relations:** `Paper --contains--> TextSegment`; `TextSegment --yields--> EntityMention`; `TextSegment --yields--> RelationMention`; `EntityMention --normalized_to--> CanonicalEntity`; `RelationMention --normalized_to--> CanonicalRelation`; `CanonicalRelation --connects--> CanonicalEntity`; `ExtractionRecord --sourced_from--> Preprint`

**Likely queries:**

```text
Which normalized entities occur in the corpus?
Which source text supports an extracted relation?
```

</details>

<details>
<summary>23. TopoMAS</summary>

**Paper:** [TopoMAS paper](https://doi.org/10.1002/mgea.70045)

**Focus:** Multi-agent topological-materials design with retrieval, generation, validation, and a self-refining KG.

**Typical contents:**

```text
Candidate material
  ├─ generated_by ──> Design agent
  ├─ validated_by ──> First-principles calculation
  └─ recorded_in ───> Self-refining KG
```

**Schema:**

- **Nodes:** `DesignTask`, `TopologicalMaterial`, `CandidateMaterial`, `MaterialStructure`, `DesignAgent`, `ValidationAgent`, `FirstPrinciplesCalculation`, `Prediction`, `EvidenceRecord`, `SelfRefiningKG`
- **Relations:** `Task --targets--> TopologicalMaterial`; `Candidate --generated_by--> DesignAgent`; `Candidate --has_structure--> MaterialStructure`; `Candidate --validated_by--> FirstPrinciplesCalculation`; `Candidate --predicted_by--> Prediction`; `Candidate --recorded_in--> SelfRefiningKG`; `Evidence --supports--> Candidate`

**Likely queries:**

```text
Which candidates passed computational validation?
Which evidence led to a proposed material?
```

</details>

<details>
<summary>24. TopoChat</summary>

**Paper:** [TopoChat paper](https://doi.org/10.1007/s11390-025-5113-9)

**Focus:** LLM retrieval assistant for topological materials using multiple knowledge sources.

**Typical contents:**

```text
Topological material
  ├─ described_in ─> Literature source
  ├─ has_attribute -> Topological feature
  └─ retrieved_with -> User question
```

**Schema:**

- **Nodes:** `TopologicalMaterial`, `TopologicalFeature`, `MaterialProperty`, `LiteratureSource`, `DatabaseRecord`, `UserQuestion`, `RetrievedEvidence`, `Answer`
- **Relations:** `Material --has_attribute--> TopologicalFeature`; `Material --has_property--> MaterialProperty`; `Material --described_in--> LiteratureSource`; `Material --represented_by--> DatabaseRecord`; `Question --retrieves--> RetrievedEvidence`; `Evidence --supports--> Answer`

**Likely queries:**

```text
Which sources support a topological-materials answer?
What attributes distinguish two candidates?
```

</details>

<details>
<summary>25. GraphAgents</summary>

**Paper:** [GraphAgents preprint](https://arxiv.org/abs/2602.07491)  
**Local article:** [GraphAgents](paper_html/graphagents.html)

**Focus:** Knowledge-graph-guided agentic AI for cross-domain materials design.

**Typical contents:**

```text
Design task
  ├─ constrained_by ─> Graph relation or requirement
  ├─ explored_by ────> Agent
  └─ supported_by ───> Evidence record
```

**Schema:**

- **Nodes:** `DesignTask`, `DesignObjective`, `MaterialCandidate`, `GraphConstraint`, `Agent`, `Tool`, `EvidenceRecord`, `Decision`, `DesignOutcome`
- **Relations:** `Task --has_objective--> DesignObjective`; `Task --constrained_by--> GraphConstraint`; `Task --explored_by--> Agent`; `Agent --uses--> Tool`; `Agent --generates--> MaterialCandidate`; `Decision --based_on--> GraphConstraint`; `Decision --supported_by--> EvidenceRecord`; `Task --produces--> DesignOutcome`

**Likely queries:**

```text
Which graph constraints guided a design decision?
Which evidence supports a candidate?
```

</details>

<details>
<summary>26. Biodegradable-Polymer Expert System</summary>

**Paper:** [Biodegradable-polymer expert system](https://arxiv.org/abs/2602.16650)

**Focus:** Retrieval-augmented expert system built from biodegradable-polymer publications.

**Typical contents:**

```text
Polymer
  ├─ has_biodegradability_claim -> Claim
  ├─ studied_in ────────────────> Publication
  └─ supported_by ───────────────> Evidence passage
```

**Schema:**

- **Nodes:** `Polymer`, `BiodegradabilityClaim`, `BiodegradationMechanism`, `Property`, `Publication`, `EvidencePassage`, `RetrievalQuery`, `Answer`
- **Relations:** `Polymer --has_biodegradability_claim--> BiodegradabilityClaim`; `Claim --concerns--> Property`; `Claim --has_mechanism--> BiodegradationMechanism`; `Polymer --studied_in--> Publication`; `Claim --supported_by--> EvidencePassage`; `EvidencePassage --located_in--> Publication`; `Query --returns--> Answer`

**Likely queries:**

```text
What evidence supports a biodegradability claim?
Which publications discuss a selected polymer?
```

</details>

<details>
<summary>27. Reified KG for Emerging Materials</summary>

**Paper:** [Reified KG paper](https://doi.org/10.1145/3765611.3815335)

**Focus:** Reified claims and evidence used to enhance RAG over emerging materials science.

**Typical contents:**

```text
Claim node
  ├─ has_subject ──> Material
  ├─ has_object ───> Property or relation
  ├─ supported_by ──> Source
  └─ has_qualifier ─> Condition or uncertainty
```

**Schema:**

- **Nodes:** `Claim`, `Material`, `Property`, `Relation`, `Condition`, `UncertaintyQualifier`, `Source`, `EvidencePassage`, `ExtractionActivity`
- **Relations:** `Claim --has_subject--> Material`; `Claim --has_predicate--> Relation`; `Claim --has_object--> Property`; `Claim --has_qualifier--> Condition`; `Claim --has_uncertainty--> UncertaintyQualifier`; `Claim --supported_by--> Source`; `Source --contains--> EvidencePassage`; `Claim --generated_by--> ExtractionActivity`

**Likely queries:**

```text
Which sources support a claim and under what conditions?
Which claims concern a selected material?
```

</details>

<details>
<summary>28. Polymer Biodegradability VectorRAG/GraphRAG</summary>

**Paper:** [Polymer biodegradability retrieval paper](https://doi.org/10.1038/s43246-026-01267-x)

**Focus:** Comparison of vector and graph retrieval over a canonicalized polymer literature KG.

**Typical contents:**

```text
Polymer claim
  ├─ canonicalizes_to -> Polymer entity
  ├─ related_to ──────> Biodegradability evidence
  └─ reported_in ─────> Publication
```

**Schema:**

- **Nodes:** `Polymer`, `PolymerMention`, `CanonicalPolymer`, `BiodegradabilityClaim`, `BiodegradabilityEvidence`, `Publication`, `Property`, `RetrievalResult`
- **Relations:** `Mention --canonicalizes_to--> CanonicalPolymer`; `Polymer --has_claim--> BiodegradabilityClaim`; `Claim --related_to--> BiodegradabilityEvidence`; `Claim --reported_in--> Publication`; `Evidence --concerns--> Property`; `Result --retrieves--> Claim`

**Likely queries:**

```text
Which retrieval path answers a multi-hop question?
Which publications support the canonicalized claim?
```

</details>

<details>
<summary>29. Multi-Source Heterogeneous Materials KG</summary>

**Paper:** [Heterogeneous materials KG paper](https://doi.org/10.3390/electronics15163615)

**Focus:** Heterogeneous, multi-source graph for complex materials question answering.

**Typical contents:**

```text
Material record
  ├─ comes_from ───> Database or document source
  ├─ aligned_with ─> Equivalent record
  └─ linked_to ────> Property, process, or application
```

**Schema:**

- **Nodes:** `MaterialRecord`, `CanonicalMaterial`, `DatabaseSource`, `DocumentSource`, `Property`, `Process`, `Application`, `EquivalentRecord`, `AlignmentRecord`
- **Relations:** `Record --comes_from--> DatabaseSource`; `Record --comes_from--> DocumentSource`; `Record --aligned_with--> EquivalentRecord`; `Record --resolves_to--> CanonicalMaterial`; `Material --linked_to--> Property`; `Material --linked_to--> Process`; `Material --linked_to--> Application`

**Likely queries:**

```text
Which sources describe the same material?
Which path answers a complex materials question?
```

</details>

<details>
<summary>30. OptoMatKG</summary>

**Paper:** [OptoMatKG preprint](https://doi.org/10.26434/chemrxiv.15008315/v1)

**Focus:** Traceable knowledge graph and semantic retrieval framework for optoelectronic materials science.

**Typical contents:**

```text
Optoelectronic material
  ├─ has_property ─> Optical or electronic property
  ├─ used_in ──────> Device or application
  └─ supported_by ─> Source and provenance
```

**Schema:**

- **Nodes:** `OptoelectronicMaterial`, `Composition`, `OpticalProperty`, `ElectronicProperty`, `PropertyValue`, `Device`, `Application`, `Paper`, `Claim`, `ProvenanceRecord`, `RetrievalResult`
- **Relations:** `Material --has_composition--> Composition`; `Material --has_optical_property--> OpticalProperty`; `Material --has_electronic_property--> ElectronicProperty`; `Property --has_value--> PropertyValue`; `Material --used_in--> Device`; `Device --serves_application--> Application`; `Claim --supported_by--> Paper`; `Claim --has_provenance--> ProvenanceRecord`; `Result --returns--> Claim`

**Likely queries:**

```text
Which materials have a target optoelectronic property?
Which sources support a material-device relation?
```

</details>

<details>
<summary>Cross-Project View</summary>

```text
MatKG
  literature entities and relations

Materials Knowledge Graph via LLM
  extracted entities, normalized names, and article provenance

Propnet
  quantities, units, properties, and derivation models

MEKG
  samples, procedures, instruments, measurements, and workflows

MOF-KG
  MOFs, linkers, metal nodes, properties, and question-answer queries

NR-KG
  alloy compositions, element graphs, numerical properties, and predictions

SolarGraph AI
  photovoltaic materials, devices, RDF/OWL relations, provenance, and retrieval

Additional projects
  linked data, domain ontologies, table extraction, specialized retrieval,
  agentic design, polymers, and traceable evidence graphs
```

</details>

A combined AI4Materials system could connect these contents through shared identifiers:

```text
Paper DOI
  -> Material
  -> Property value
  -> Process or experiment
  -> Application or device
  -> Provenance and supporting evidence
```
