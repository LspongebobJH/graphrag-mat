# AI4Materials KG Construction Pipeline

This page summarizes the construction pipelines documented in the cited papers and public repositories. Tool names below refer to tools explicitly described by the project documentation or visible in its code repository. An LLM is included only where the project actually uses one for that stage.

## 1. MatKG

1. **Collect literature and metadata:**
       - **Input:** MatKG corpus, DOI records, and the reported collection of more than 5 million materials-science papers.
       - **Process:** Gather literature identifiers and metadata from the corpus and associated release records.
       - **Output:** DOI-indexed literature corpus distributed through Zenodo.
2. **Extract entities and relations:**
       - **Input:** Literature records and article text processed by the MatKG extraction workflow.
       - **Process:** Apply the paper's automated NLP and statistical relation-construction methods; no LLM extraction is documented in the repository.
       - **Output:** Entity and relation CSV records such as `ENTDOI.csv`.
3. **Create graph files:**
       - **Input:** Entity/relation CSV files and subject-relation-object records.
       - **Process:** Run the repository notebooks to convert CSV data into N-Triples and subject-relation-object files.
       - **Output:** `ENTNERPTDOI.nt` and graph-ready relation files.
4. **Analyze and link entities:**
       - **Input:** N-Triples, subject-relation-object files, Wikidata, and Materials Project identifiers.
       - **Process:** Analyze graph records and link material entities to external identifiers using the documented notebooks.
       - **Output:** Enriched entities with Wikidata and Materials Project links.
5. **Publish for querying:**
       - **Input:** Graph files, linked identifiers, and downloadable data artifacts.
       - **Process:** Release the graph through GitHub and Zenodo for literature-scale retrieval.
       - **Output:** Public material, property, method, and application graph resources.

**Tool profile:** Automated NLP and statistical relation construction in the paper; Jupyter notebooks, CSV, N-Triples, Wikidata, Materials Project, GitHub, and Zenodo in the public implementation. No LLM-based extraction is documented in the repository.

## 2. Materials Knowledge Graph via LLM

1. **Prepare training and expert data:**
       - **Input:** `TrainSet1.0.json`, `TrainSet2.0.json`, Name/Formula data, `Property.xlsx`, and `Keywords.xlsx`.
       - **Process:** Organize labeled examples, classification data, properties, and expert keyword dictionaries for training.
       - **Output:** Training, classification, and domain-knowledge inputs.
2. **Fine-tune the extractor:**
       - **Input:** Prepared training sets, Meta's LLaMA-7B, and Hugging Face checkpoint configuration.
       - **Process:** Fine-tune the LLM with `train.py`, PyTorch distributed training, and checkpoint saving.
       - **Output:** Domain-adapted LLaMA-7B extraction model.
3. **Extract entities and relations:**
       - **Input:** Materials corpus and trained model checkpoint.
       - **Process:** Run `inference_KG.py` for NERRE extraction of names, formulas, properties, applications, and relations.
       - **Output:** Raw entity and relation predictions.
4. **Resolve and clean entities:**
       - **Input:** Raw predictions, Mat2vec representations, ChemDataExtractor results, and entity-resolution notebooks.
       - **Process:** Normalize material mentions, formulas, properties, and aliases with `Entity Resolution.ipynb`.
       - **Output:** Cleaned and canonicalized entities and relations.
5. **Build and extend the KG:**
       - **Input:** Cleaned triples and relation candidates.
       - **Process:** Build graph triples and run `Graph completion (non-embedding).ipynb`; use PyKEEN TransE prediction for link completion.
       - **Output:** Materials KG with completed or predicted links.

**Tool profile:** LLaMA-7B and PyTorch for model training/inference; `inference_KG.py`, Jupyter notebooks, Mat2vec, ChemDataExtractor, and PyKEEN. This is the clearest example where an LLM performs the primary extraction step.

## 3. Propnet

1. **Load material data:**
       - **Input:** Material properties from Materials Project or another supported database.
       - **Process:** Import source values into Propnet's source-agnostic material representation.
       - **Output:** Material records with measured quantities and metadata.
2. **Represent graph objects:**
       - **Input:** Materials, quantities, units, properties, and model definitions.
       - **Process:** Use the Propnet Python package to create typed graph nodes; this is computational modeling, not LLM prose extraction.
       - **Output:** Unit-aware property graph objects.
3. **Register property models:**
       - **Input:** Tested property equations and model implementations such as density from mass and volume.
       - **Process:** Register model inputs, outputs, units, and dependencies in the Python library.
       - **Output:** Executable, unit-aware property models.
4. **Calculate and validate:**
       - **Input:** Compatible quantities, registered models, and unit definitions.
       - **Process:** Execute calculations, convert units, retain dependencies, and run repository tests or demo notebooks.
       - **Output:** Calculated properties with derivation and validation results.
5. **Inspect derivations:**
       - **Input:** Calculated properties, input quantities, model nodes, and dependencies.
       - **Process:** Use the Python API, Binder notebook, or web app to inspect the derivation graph.
       - **Output:** Explainable property calculation and input/model graph.

**Tool profile:** Propnet's Python package, unit-aware property models, Materials Project integration, tests, Sphinx documentation, Binder notebooks, and web app. This is a computational property graph, not an LLM extraction pipeline.

## 4. Materials Experiment Knowledge Graph (MEKG)

1. **Obtain structured database records:**
       - **Input:** Materials Provenance Store PostgreSQL dump and MEKG Neo4j dump.
       - **Process:** Download releases with `scripts/download-releases.sh` and prepare the repository's loader services.
       - **Output:** Reproducible PostgreSQL and Neo4j source releases.
2. **Restore the source databases:**
       - **Input:** Database dumps, Docker configuration, and loader services.
       - **Process:** Use Docker Compose, `postgres-loader`, `neo4j-loader`, and `neo4j-admin` to restore both databases.
       - **Output:** Running PostgreSQL and Neo4j source databases.
3. **Migrate into the experiment graph:**
       - **Input:** Instantiated MPS database, migration scripts, and Cypher assets.
       - **Process:** Transform provenance-store records into the Neo4j MEKG schema.
       - **Output:** Neo4j experiment and provenance graph.
4. **Query and inspect workflows:**
       - **Input:** MEKG nodes for samples, procedures, instruments, and measurements.
       - **Process:** Use Neo4j Browser, ports 7474/7687, and Cypher workflow queries.
       - **Output:** Workflow paths and traceable experiment results.
5. **Run analysis notebooks:**
       - **Input:** Restored databases, query results, and `src/QueryTimings.ipynb`.
       - **Process:** Run Dockerized Jupyter analysis and scripted access through `mps-client`.
       - **Output:** Query timing measurements and reproducible workflow analyses.

**Tool profile:** PostgreSQL, Neo4j, Docker, Docker Compose, shell scripts, `neo4j-admin`, Cypher, Jupyter, and `mps-client`. The public repository documents database setup and querying rather than an LLM extraction step.

## 5. KGQA4MAT / MOF-KG

1. **Assemble the MOF graph:**
       - **Input:** Structured MOF databases, literature-derived records, and repository CSV triples under `data`.
       - **Process:** Integrate MOF entities, linkers, metal nodes, and properties into graph triples.
       - **Output:** MOF-KG data files and domain graph.
2. **Prepare benchmark queries:**
       - **Input:** Jupyter notebooks, benchmark questions, expected answers, and MOF graph triples.
       - **Process:** Encode entities, linkers, metal nodes, properties, comparison, aggregation, and multi-hop constraints.
       - **Output:** KGQA4MAT benchmark query set.
3. **Probe language models:**
       - **Input:** MOF text and candidate models BERT, SciBERT, RoBERTa, MatBERT, and MatSciBERT.
       - **Process:** Compare pretrained models for extracting knowledge triples; these are not treated as generative KG builders.
       - **Output:** Model-specific triple extraction results.
4. **Translate questions into graph queries:**
       - **Input:** Natural-language benchmark question and its entities or constraints.
       - **Process:** Use ChatGPT to translate the question into a formal KG query.
       - **Output:** Generated SPARQL/Cypher-style query candidate.
5. **Evaluate answers:**
       - **Input:** Generated queries, MOF-KG, and benchmark expected answers.
       - **Process:** Execute queries and compare results for comparison, aggregation, and multi-hop cases.
       - **Output:** QA accuracy and query-translation evaluation.

**Tool profile:** Structured MOF databases, literature-derived CSV triples, Jupyter notebooks, BERT/SciBERT/RoBERTa/MatBERT/MatSciBERT, ChatGPT for KGQA query translation, and formal KG queries.

## 6. NR-KG

1. **Prepare alloy datasets:**
       - **Input:** HEA-HD hardness data, HEA-CRD corrosion-resistance data, and repository KG directories.
       - **Process:** Organize original alloy records and cross-modal graph datasets by prediction task.
       - **Output:** Training and evaluation dataset paths.
2. **Load graph data:**
       - **Input:** `entity.pkl`, `relation.pkl`, and `dataset/HEA-HD-KG` or `dataset/HEA-CRD-KG`.
       - **Process:** Deserialize entity and relation mappings with Python `pickle`.
       - **Output:** In-memory alloy entities, relations, and graph structures.
3. **Represent numerical and graph features:**
       - **Input:** Alloy compositions, known properties, entities, relations, and graph neighborhoods.
       - **Process:** Run project utilities and `GCN_model.py` to combine numerical features with graph structure.
       - **Output:** Model-ready graph and numerical feature tensors.
4. **Train the predictor:**
       - **Input:** Feature tensors, target properties, dataset path, GPU settings, and hyperparameters.
       - **Process:** Run `main.py` with PyTorch 1.13.1 and documented cross-validation settings.
       - **Output:** Trained GCN predictor and checkpoints.
5. **Test and save predictions:**
       - **Input:** Test features, checkpoints under `model/pre_trained`, and evaluation configuration.
       - **Process:** Run `main.py --state test` and compare predictions with held-out properties.
       - **Output:** Prediction metrics and result files under `res`.

**Tool profile:** Python 3.8, PyTorch 1.13.1, pickle graph files, project data utilities, `GCN_model.py`, `main.py`, pretrained checkpoints, and GPU execution. NR-KG uses graph learning, not an LLM for extraction.

## 7. SolarGraph AI

1. **Define the semantic schema:**
       - **Input:** OWL concepts, PMDco/QUDT vocabulary, photovoltaic domain terms, and validation requirements.
       - **Process:** Define `ontology.ttl`, RDF classes/relations in `data.ttl`, and SHACL constraints in `shapes.ttl`.
       - **Output:** OWL/RDF schema, photovoltaic facts, and validation shapes.
2. **Build and validate the graph:**
       - **Input:** Turtle files, OWL schema, RDF data, and SHACL shapes.
       - **Process:** Run `build_graph.py`; RDFLib loads the graph, SHACL validates it, and the build writes `graph.pkl`.
       - **Output:** Validated serialized RDF graph and validation report.
3. **Ingest optional literature:**
       - **Input:** Literature records, OpenAlex API access, and the existing RDF graph.
       - **Process:** Run `ingest_literature.py`; treat the OpenAlex/LLM route as an extension to the hand-authored base graph.
       - **Output:** Optional literature-derived entities and relations.
4. **Retrieve evidence:**
       - **Input:** User question, RDF graph, SPARQL patterns, and provenance metadata.
       - **Process:** Use `query_engine.py`, RDFLib SPARQL, and `provenance.py` to retrieve supporting triples and cited entities.
       - **Output:** Evidence graph, SPARQL query, and provenance record.
5. **Generate grounded answers:**
       - **Input:** Retrieved triples, provenance, user question, and answer-generation context.
       - **Process:** Use `react_agent.py` or `llm_agent.py`; the documented Groq `llama3-70b-8192` agent calls SPARQL.
       - **Output:** Grounded photovoltaic answer with supporting triples and citations.

**Tool profile:** OWL/Turtle, RDFLib 7, SPARQL 1.1, SHACL, QUDT, PROV-O, Python, the build/query/provenance modules, and a Groq Llama 3 ReAct agent. The base KG is ontology/data-file driven.

## 8. MMKG

1. **Collect linked data:**
       - **Input:** DBpedia and Wikipedia records about metallic materials.
       - **Process:** Extract material entities, relations, compositions, and descriptive statements from both linked-data sources.
       - **Output:** Raw metallic-material records with source identifiers.
2. **Normalize records:**
       - **Input:** Raw records, names, identifiers, compositions, and properties.
       - **Process:** Align equivalent entities and normalize names, identifiers, compositions, and descriptive properties.
       - **Output:** Canonical metallic-material records linked across sources.
3. **Publish graph facts:**
       - **Input:** Canonical entities, relations, and source references.
       - **Process:** Represent the records as linked graph facts and preserve source links.
       - **Output:** Queryable metallic-material knowledge graph.
4. **Validate links:**
       - **Input:** Graph entities, identifiers, relation edges, and source URLs.
       - **Process:** Check duplicates, incompatible identifiers, missing links, and broken source references.
       - **Output:** Validated graph release and an error list for unresolved records.
5. **Support retrieval:**
       - **Input:** Validated material, composition, property, and provenance relations.
       - **Process:** Traverse linked-data relations for entity and property lookup.
       - **Output:** Retrieved metallic-material facts with source context.

**Tool profile:** DBpedia, Wikipedia, linked-data identifiers, and graph-based entity integration. No public implementation repository was located.

## 9. NanoMine

1. **Define the ontology:**
       - **Input:** Nanocomposite concepts, component types, workflow terms, and property definitions.
       - **Process:** Specify classes and relations for components, formulations, processing, experiments, and properties.
       - **Output:** Nanocomposite ontology and schema constraints.
2. **Load material records:**
       - **Input:** Nanocomposite compositions, processing records, experiment records, and measurements.
       - **Process:** Map each record to ontology classes and convert observations into typed graph relations.
       - **Output:** Ontology-compliant nanocomposite graph records.
3. **Connect evidence:**
       - **Input:** Formulation details, processing conditions, measurements, and source records.
       - **Process:** Link processing and composition nodes to measured material behavior and evidence.
       - **Output:** Provenance-aware formulation-process-property paths.
4. **Validate the model:**
       - **Input:** Populated ontology, graph records, and schema constraints.
       - **Process:** Check that component, process, experiment, and property relations conform to the model.
       - **Output:** Validated nanocomposite KG and identified constraint violations.
5. **Support reuse:**
       - **Input:** Validated records with workflow and measurement provenance.
       - **Process:** Query comparable formulations, processes, and behaviors across studies.
       - **Output:** Reusable nanocomposite evidence for discovery and comparison.

**Tool profile:** Nanocomposite ontology and structured materials/workflow records. No public implementation repository was located.

## 10. Knowledge Graph-Empowered Materials Discovery

1. **Assemble discovery data:**
       - **Input:** Candidate materials, descriptors, known relationships, and discovery evidence.
       - **Process:** Load candidate and evidence records into a graph representation.
       - **Output:** Initial materials-discovery graph.
2. **Normalize candidates:**
       - **Input:** Material names, identifiers, compositions, and descriptor values.
       - **Process:** Resolve equivalent materials and make descriptors comparable across records.
       - **Output:** Canonical candidate nodes and normalized feature relations.
3. **Support recommendation:**
       - **Input:** Candidate graph neighborhoods, descriptors, and source evidence.
       - **Process:** Traverse related materials and rank candidates using graph context and evidence.
       - **Output:** Ranked discovery recommendations with graph explanations.
4. **Evaluate candidates:**
       - **Input:** Recommendations, known materials, and reported discovery outcomes.
       - **Process:** Compare recommended candidates with reference materials and observed results.
       - **Output:** Recommendation evaluation metrics and failure cases.
5. **Retain evidence:**
       - **Input:** Ranked candidates, descriptors, relations, and source records.
       - **Process:** Attach supporting evidence and provenance to each recommendation.
       - **Output:** Traceable discovery results suitable for review.

**Tool profile:** Materials knowledge graph, discovery records, recommendation workflow, and graph-based retrieval. No public implementation repository was located.

## 11. Semantic KG for Li-Ion Battery Cathodes

1. **Collect cathode knowledge:**
       - **Input:** Cathode literature, material records, synthesis descriptions, and performance measurements.
       - **Process:** Load material, process, characterization, and electrochemical records into a semantic graph.
       - **Output:** Initial cathode knowledge records with source references.
2. **Extract relationships:**
       - **Input:** Text passages, procedures, measurements, properties, and article metadata.
       - **Process:** Extract and link each cathode to preparation, characterization, performance, and evidence nodes.
       - **Output:** Cathode synthesis-property relation graph.
3. **Enable comparison:**
       - **Input:** Linked cathode materials, synthesis routes, property values, and conditions.
       - **Process:** Query graph paths connecting preparation choices to electrochemical outcomes.
       - **Output:** Comparable cathode candidates and supporting paths.
4. **Check values:**
       - **Input:** Electrochemical values, units, measurement conditions, and source records.
       - **Process:** Normalize units and retain conditions with each measured property.
       - **Output:** Comparable, condition-aware cathode properties.
5. **Trace results:**
       - **Input:** Candidate rankings, graph paths, synthesis records, and characterization evidence.
       - **Process:** Retrieve the evidence supporting each recommendation.
       - **Output:** Traceable cathode recommendation with citations.

**Tool profile:** Semantic materials KG, battery-cathode literature, synthesis and property records. No public implementation repository was located.

## 12. Compound KG-Enabled AI Assistant

1. **Ingest compound records:**
       - **Input:** Compound identities, related materials, property records, and literature evidence.
       - **Process:** Load compounds and their relations into an assistant-oriented knowledge graph.
       - **Output:** Initial compound graph with source links.
2. **Resolve entities:**
       - **Input:** Compound names, formulas, aliases, and identifiers.
       - **Process:** Merge equivalent names and connect normalized compound-property relations.
       - **Output:** Canonical compound nodes and resolved relations.
3. **Ground assistance:**
       - **Input:** User discovery question and relevant compound graph.
       - **Process:** Retrieve graph context matching entities, properties, and relations in the question.
       - **Output:** Assistant context containing graph facts and evidence.
4. **Rank answers:**
       - **Input:** Retrieved compounds, constraints, relations, and literature relevance scores.
       - **Process:** Select context that best satisfies the user constraints.
       - **Output:** Ranked evidence set for response generation.
5. **Cite evidence:**
       - **Input:** Generated answer, selected graph facts, and source records.
       - **Process:** Attach source records to the claims returned by the assistant.
       - **Output:** Discovery answer with traceable citations.

**Tool profile:** Compound knowledge graph, entity resolution, literature evidence, and AI-assistant retrieval. No public implementation repository was located.

## 13. Materials Terminology KG

1. **Process the corpus:**
       - **Input:** Materials-science text corpus and document metadata.
       - **Process:** Extract terminology mentions and semantic relations with corpus-level NLP.
       - **Output:** Raw term and relation candidates.
2. **Normalize concepts:**
       - **Input:** Term candidates, synonyms, aliases, and relation candidates.
       - **Process:** Link equivalent terms and normalize concept identifiers.
       - **Output:** Canonical materials terminology graph.
3. **Provide terminology search:**
       - **Input:** Canonical concept nodes and semantic relations.
       - **Process:** Traverse related concepts and retrieve connected terminology.
       - **Output:** Searchable concept neighborhoods.
4. **Check concept quality:**
       - **Input:** Graph nodes, relation frequencies, and source passages.
       - **Process:** Review duplicate, ambiguous, and weakly connected concepts.
       - **Output:** Cleaned terminology graph and quality flags.
5. **Preserve provenance:**
       - **Input:** Accepted terms, relations, and source text locations.
       - **Process:** Attach extraction evidence to each concept and relation.
       - **Output:** Provenance-aware terminology records.

**Tool profile:** Materials-science text corpus, terminology extraction, concept normalization, and knowledge-graph search. No public implementation repository was located.

## 14. Knowledge Graph Representation of Zeolitic Materials

1. **Collect structural facts:**
       - **Input:** Crystal records, zeolite frameworks, guest-species descriptions, and property reports.
       - **Process:** Load structural and chemical facts with their source metadata.
       - **Output:** Raw zeolitic-material records.
2. **Build graph relations:**
       - **Input:** Framework, guest, crystal, property, and source entities.
       - **Process:** Represent framework membership, guest inclusion, structure, and source relations.
       - **Output:** Domain graph of zeolitic materials.
3. **Connect question answering:**
       - **Input:** User question and relevant graph entities.
       - **Process:** Retrieve graph paths and provide them as context to an LLM.
       - **Output:** Evidence context for a generated answer.
4. **Validate structure:**
       - **Input:** Graph relations and corresponding source records.
       - **Process:** Check framework, guest-species, crystal, and property assertions against sources.
       - **Output:** Validated zeolitic-material graph.
5. **Ground responses:**
       - **Input:** Generated answer, graph paths, and source records.
       - **Process:** Attach supporting paths and sources to the answer.
       - **Output:** Grounded zeolitic-material response.

**Tool profile:** Zeolitic-material records, crystal and guest-species relations, KG retrieval, and LLM-based QA. No public implementation repository was located.

## 15. Upper-Level Ontologies for Materials KGs

1. **Define test cases:**
       - **Input:** Representative materials, processes, observations, measurements, and provenance examples.
       - **Process:** Model the examples as competency questions and ontology test cases.
       - **Output:** Shared evaluation cases for upper-level ontology comparison.
2. **Compare foundations:**
       - **Input:** Test cases and candidate BFO, EMMO, and PROVO concepts.
       - **Process:** Map entities and activities and evaluate how each foundation represents them.
       - **Output:** Comparative ontology mappings.
3. **Select schema guidance:**
       - **Input:** Ontology mappings, competency questions, and modeling results.
       - **Process:** Identify which foundation best supports required materials KG relations.
       - **Output:** Ontology-selection guidance.
4. **Test interoperability:**
       - **Input:** Representative materials concepts and mappings across candidate ontologies.
       - **Process:** Check whether concepts and provenance assertions can be translated consistently.
       - **Output:** Interoperability results and mapping limitations.
5. **Document tradeoffs:**
       - **Input:** Comparison results, limitations, and provenance implications.
       - **Process:** Record modeling strengths and risks for future KG construction.
       - **Output:** Reusable ontology design recommendations.

**Tool profile:** BFO, EMMO, PROVO, ontology mappings, and materials-science modeling examples. No public implementation repository was located.

## 16. LLM Extraction from Materials Tables

1. **Parse table structure:**
       - **Input:** Scientific tables, headers, rows, units, conditions, and document metadata.
       - **Process:** Identify table boundaries and associate each cell with its row, column, and material context.
       - **Output:** Structured table representation with coordinates.
2. **Extract knowledge:**
       - **Input:** Structured tables and cell-level context.
       - **Process:** Use an LLM to extract entities, values, units, conditions, and relations.
       - **Output:** Candidate material-property and process records.
3. **Validate provenance:**
       - **Input:** Candidate records, table coordinates, and source document.
       - **Process:** Normalize values and attach each fact to its supporting table location.
       - **Output:** Provenance-linked table facts.
4. **Check associations:**
       - **Input:** Extracted facts, row/column associations, and material identifiers.
       - **Process:** Verify that values remain attached to the correct material and experimental condition.
       - **Output:** Corrected and validated table triples.
5. **Export structured data:**
       - **Input:** Validated entities, relations, values, and table provenance.
       - **Process:** Serialize the records into graph-compatible data for retrieval.
       - **Output:** Queryable table-derived knowledge graph.

**Tool profile:** Scientific tables, LLM extraction, materials entities, property values, units, and table-level provenance. No public implementation repository was located.

## 17. Framework Material KG

1. **Collect framework literature:**
       - **Input:** Framework-material papers, abstracts, metadata, and domain terminology.
       - **Process:** Gather reports about composition, structure, synthesis, characterization, and applications.
       - **Output:** Framework-material literature corpus.
2. **Extract and resolve:**
       - **Input:** Literature passages and framework-material dictionaries.
       - **Process:** Use LLM-supported extraction and resolve repeated framework and property mentions.
       - **Output:** Normalized framework entities and relations.
3. **Publish research graph:**
       - **Input:** Normalized entities, relations, article identifiers, and passages.
       - **Process:** Build graph edges and retain article provenance.
       - **Output:** Framework-material knowledge graph.
4. **Validate claims:**
       - **Input:** Graph claims and cited literature passages.
       - **Process:** Check composition, synthesis, characterization, and application assertions.
       - **Output:** Validated claims with review flags.
5. **Index for retrieval:**
       - **Input:** Validated framework, property, method, and application relations.
       - **Process:** Build searchable graph paths for domain questions.
       - **Output:** Framework-material retrieval index.

**Tool profile:** Framework-material literature, LLM extraction, entity resolution, graph relations, and article provenance. No public implementation repository was located.

## 18. CLASS MATE

1. **Collect multilingual text:**
       - **Input:** Materials-science documents, terminology, and language metadata.
       - **Process:** Build aligned document and concept collections in the supported languages.
       - **Output:** Multilingual materials corpus.
2. **Align concepts:**
       - **Input:** Language-specific terms, entities, and graph relations.
       - **Process:** Link equivalent concepts and relations across languages.
       - **Output:** Cross-lingual concept mappings.
3. **Retrieve semantically:**
       - **Input:** User query, aligned concepts, and multilingual documents.
       - **Process:** Use graph-aligned semantic representations to retrieve relevant results.
       - **Output:** Cross-lingual materials-science results.
4. **Evaluate alignment:**
       - **Input:** Test queries, equivalent concepts, and relevance judgments.
       - **Process:** Measure concept alignment and document retrieval across languages.
       - **Output:** Cross-lingual retrieval evaluation.
5. **Preserve language links:**
       - **Input:** Retrieved documents, aligned terms, and source metadata.
       - **Process:** Return the language mappings and documents supporting each result.
       - **Output:** Traceable cross-lingual search response.

**Tool profile:** Multilingual materials text, concept alignment, knowledge-graph representations, and semantic retrieval. No public implementation repository was located.

## 19. Materials Property KG: NIMS Case Study

1. **Extract property claims:**
       - **Input:** Materials-property literature, records, values, units, and source metadata.
       - **Process:** Identify materials, properties, values, conditions, and relationships with NLP/LLM methods.
       - **Output:** Candidate material-property claims.
2. **Normalize and link:**
       - **Input:** Candidate claims, material names, property names, and identifiers.
       - **Process:** Resolve names and connect every value to its source record.
       - **Output:** Canonical property graph.
3. **Support analysis:**
       - **Input:** Linked material-property records and search or prediction constraints.
       - **Process:** Traverse graph relations for property search and prediction.
       - **Output:** Search results or prediction inputs with provenance.
4. **Validate units:**
       - **Input:** Property values, units, value types, and experimental conditions.
       - **Process:** Check unit compatibility and separate measured, inferred, and predicted values.
       - **Output:** Validated property records.
5. **Trace predictions:**
       - **Input:** Predicted values, model inputs, graph relations, and source records.
       - **Process:** Record the model context and evidence associated with each prediction.
       - **Output:** Provenance-aware material-property prediction.

**Tool profile:** Materials-property literature, NLP/LLM extraction, value and unit normalization, provenance, search, and prediction. No public implementation repository was located.

## 20. LLM-Driven Knowledge Graphs for Materials Science

1. **Prepare literature:**
       - **Input:** Materials-science documents, abstracts, metadata, and preprocessing rules.
       - **Process:** Clean and segment documents for entity and relation extraction.
       - **Output:** Extraction-ready literature corpus.
2. **Construct graph:**
       - **Input:** Prepared passages and extraction prompts or schemas.
       - **Process:** Use an LLM to extract normalized entities, relations, values, and source links.
       - **Output:** Literature-derived graph triples.
3. **Query naturally:**
       - **Input:** User question, graph entities, relations, and provenance.
       - **Process:** Resolve question entities and retrieve relevant graph facts.
       - **Output:** Structured context for answer generation.
4. **Evaluate extraction:**
       - **Input:** Representative literature examples and expected entities or relations.
       - **Process:** Check entity resolution, relation quality, value extraction, and source attribution.
       - **Output:** Extraction evaluation and error cases.
5. **Ground answers:**
       - **Input:** Retrieved graph path, publication evidence, and generated response.
       - **Process:** Attach relevant graph facts and source passages to the answer.
       - **Output:** Natural-language answer with provenance.

**Tool profile:** Materials literature, LLM extraction, entity normalization, provenance links, and natural-language querying. No public implementation repository was located.

## 21. Materials Literature KG Reconstruction

1. **Segment publications:**
       - **Input:** Materials-science publications, sections, passages, and metadata.
       - **Process:** Segment documents into extraction units while retaining document and passage identifiers.
       - **Output:** Structured literature passages for reconstruction.
2. **Reconstruct relations:**
       - **Input:** Passage-level mentions of materials, properties, processes, and conditions.
       - **Process:** Resolve repeated mentions and connect facts distributed across passages.
       - **Output:** Reconstructed material-property-process relations.
3. **Retain evidence:**
       - **Input:** Reconstructed claims and publication passage identifiers.
       - **Process:** Attach each claim to its publication and supporting passage.
       - **Output:** Evidence-linked reconstructed claims.
4. **Validate reconstruction:**
       - **Input:** Cross-passage relations, subjects, conditions, values, and sources.
       - **Process:** Check that reconstruction preserves scientific context and provenance.
       - **Output:** Validated literature graph and reconstruction errors.
5. **Expose queries:**
       - **Input:** Validated material-property-process paths and source links.
       - **Process:** Index graph paths for literature-scale retrieval.
       - **Output:** Queryable reconstructed literature KG.

**Tool profile:** Materials-science publications, preprocessing, LLM-assisted extraction, entity resolution, relation reconstruction, and source provenance. No public implementation repository was located.

## 22. ChemRxiv Materials Literature KG

1. **Preprocess the corpus:**
       - **Input:** ChemRxiv materials-literature preprint and document metadata.
       - **Process:** Apply advanced preprocessing, segmentation, and normalization before extraction.
       - **Output:** Extraction-ready preprint text.
2. **Extract and normalize:**
       - **Input:** Preprocessed text, extraction prompts, and entity schema.
       - **Process:** Use LLM-based methods to identify entities and relations and map them to canonical nodes.
       - **Output:** Candidate preprint graph triples.
3. **Validate the preprint graph:**
       - **Input:** Candidate triples and supporting preprint passages.
       - **Process:** Check relation structure, entity identity, and source association.
       - **Output:** Validated preprint graph.
4. **Review extraction quality:**
       - **Input:** Normalized entities, relations, and representative passages.
       - **Process:** Compare extraction output with the source text and flag errors.
       - **Output:** Extraction quality assessment.
5. **Publish provenance:**
       - **Input:** Preprocessing status, triples, source locations, and review results.
       - **Process:** Store provenance with every graph record.
       - **Output:** Auditable ChemRxiv literature KG.

**Tool profile:** ChemRxiv materials literature, advanced preprocessing, LLM extraction, triple validation, and preprint provenance. No public implementation repository was located.

## 23. TopoMAS

1. **Retrieve design knowledge:**
       - **Input:** Topological-materials papers, databases, structures, and property records.
       - **Process:** Retrieve evidence relevant to the design objective.
       - **Output:** Design context for candidate generation.
2. **Generate and validate:**
       - **Input:** Retrieved context, design constraints, and agent prompts.
       - **Process:** Use agents to propose candidates and first-principles calculations to validate them.
       - **Output:** Candidate materials with computational results.
3. **Refine the graph:**
       - **Input:** Candidates, calculations, validation outcomes, and source evidence.
       - **Process:** Record new nodes and relations in the self-refining knowledge graph.
       - **Output:** Updated topological-materials KG.
4. **Compare iterations:**
       - **Input:** Previous candidates, failures, validation results, and current design objective.
       - **Process:** Use feedback to guide subsequent agent actions.
       - **Output:** Refined candidate set and next-step decisions.
5. **Track provenance:**
       - **Input:** Agent actions, calculations, input evidence, and validation status.
       - **Process:** Attach workflow provenance to every candidate and graph update.
       - **Output:** Auditable agentic design history.

**Tool profile:** Literature and database retrieval, multi-agent orchestration, candidate generation, first-principles validation, and self-refining KG. No public implementation repository was located.

## 24. TopoChat

1. **Integrate sources:**
       - **Input:** Topological-materials databases, papers, records, and identifiers.
       - **Process:** Combine sources and normalize material, property, and topology identifiers.
       - **Output:** Integrated topological-materials knowledge base.
2. **Retrieve evidence:**
       - **Input:** User question, graph entities, properties, and topology relations.
       - **Process:** Find graph-supported paths satisfying the question constraints.
       - **Output:** Evidence set for answer generation.
3. **Generate an answer:**
       - **Input:** Retrieved graph paths, source passages, and user question.
       - **Process:** Use an LLM to compose a response grounded in the retrieved sources.
       - **Output:** Draft topological-materials answer.
4. **Check retrieval:**
       - **Input:** Question constraints, selected sources, and generated context.
       - **Process:** Evaluate coverage of material, property, and topology requirements.
       - **Output:** Retrieval-quality assessment and corrected context.
5. **Cite context:**
       - **Input:** Final response, source records, and graph relations.
       - **Process:** Attach source records and graph paths used to generate the response.
       - **Output:** Grounded answer with citations.

**Tool profile:** Multi-source topological-materials knowledge, entity normalization, graph retrieval, LLM question answering, and source evidence. No public implementation repository was located.

## 25. GraphAgents

1. **Expose graph context:**
       - **Input:** Materials entities, graph relations, design constraints, and evidence records.
       - **Process:** Retrieve and package graph context for participating agents.
       - **Output:** Agent-accessible design context.
2. **Guide design:**
       - **Input:** Agent goals, graph context, constraints, and candidate spaces.
       - **Process:** Use graph retrieval to support planning, candidate generation, and decisions.
       - **Output:** Agent design actions and candidate materials.
3. **Record workflow state:**
       - **Input:** Agent decisions, retrieved facts, generated candidates, and evidence.
       - **Process:** Persist decisions and updated knowledge context for subsequent agents.
       - **Output:** Shared agent workflow state.
4. **Validate decisions:**
       - **Input:** Generated candidates, graph constraints, and cross-domain requirements.
       - **Process:** Check candidate decisions against graph facts and design constraints.
       - **Output:** Validated or rejected agent decisions.
5. **Trace collaboration:**
       - **Input:** Agent logs, retrieved facts, transformations, and final decisions.
       - **Process:** Record which agent used or transformed each graph fact.
       - **Output:** Agent collaboration and evidence trace.

**Tool profile:** Knowledge-graph retrieval, agentic planning, cross-domain materials data, design constraints, and evidence tracking. No public implementation repository was located.

## 26. Biodegradable-Polymer Expert System

1. **Index publications:**
       - **Input:** Biodegradable-polymer publications, polymer names, claims, conditions, and evidence passages.
       - **Process:** Extract and index polymer identities and literature-derived biodegradability facts.
       - **Output:** Searchable polymer evidence collection.
2. **Retrieve domain context:**
       - **Input:** Polymer question, normalized polymer entities, and indexed passages.
       - **Process:** Retrieve relevant passages and graph facts using domain-aware search.
       - **Output:** Polymer-specific evidence context.
3. **Generate grounded answers:**
       - **Input:** Retrieved passages, graph facts, and user question.
       - **Process:** Use retrieval-augmented generation to compose a domain answer.
       - **Output:** Draft biodegradable-polymer response.
4. **Check claim scope:**
       - **Input:** Draft answer, biodegradability claims, conditions, and source passages.
       - **Process:** Distinguish supported evidence from experimental limitations and unsupported generalizations.
       - **Output:** Scope-checked answer.
5. **Return citations:**
       - **Input:** Final answer, source passages, and polymer entities.
       - **Process:** Attach relevant publication citations and entity links.
       - **Output:** Evidence-grounded polymer answer.

**Tool profile:** Biodegradable-polymer publications, literature retrieval, domain entity normalization, RAG, and evidence passages. No public implementation repository was located.

## 27. Reified KG for Emerging Materials

1. **Create claim nodes:**
       - **Input:** Material-science statements, subject entities, relation types, and object entities.
       - **Process:** Represent each statement as an explicit reified claim object.
       - **Output:** Claim nodes instead of unqualified direct triples.
2. **Attach qualifiers:**
       - **Input:** Claims, evidence, conditions, uncertainty, and source metadata.
       - **Process:** Link qualifiers and provenance to each claim node.
       - **Output:** Qualified, evidence-bearing claim graph.
3. **Retrieve for RAG:**
       - **Input:** User question and reified claims about emerging materials.
       - **Process:** Retrieve claim-level evidence and graph paths for generation.
       - **Output:** Grounded RAG context.
4. **Check qualifiers:**
       - **Input:** Retrieved claims, conditions, polarity, uncertainty, and evidence strength.
       - **Process:** Preserve and check qualifiers during retrieval and answer construction.
       - **Output:** Qualifier-aware answer context.
5. **Audit answers:**
       - **Input:** Generated answer, claim nodes, evidence, and provenance paths.
       - **Process:** Return the claim and evidence chain used by the generator.
       - **Output:** Auditable emerging-materials answer.

**Tool profile:** Reified knowledge-graph claims, evidence and provenance relations, qualifiers, retrieval, and RAG. No public implementation repository was located.

## 28. Polymer Biodegradability VectorRAG/GraphRAG

1. **Prepare the corpus:**
       - **Input:** More than 1,000 polymer publications, aliases, and document metadata.
       - **Process:** Process the corpus and canonicalize equivalent polymer entities.
       - **Output:** Normalized polymer literature corpus.
2. **Build retrieval representations:**
       - **Input:** Canonical polymer entities, claims, evidence, and source publications.
       - **Process:** Build graph relations and vector indexes over the same evidence.
       - **Output:** Comparable GraphRAG and VectorRAG retrieval representations.
3. **Compare retrieval:**
       - **Input:** Test questions, vector results, graph paths, and evidence sources.
       - **Process:** Evaluate entity disambiguation, multi-hop reasoning, and grounding.
       - **Output:** Retrieval results for both methods.
4. **Measure results:**
       - **Input:** Retrieved contexts, generated answers, and evaluation references.
       - **Process:** Compare coverage, correctness, source attribution, and multi-hop completion.
       - **Output:** VectorRAG/GraphRAG comparison metrics.
5. **Analyze failures:**
       - **Input:** Failed queries, unresolved aliases, missing links, and incorrect answers.
       - **Process:** Identify corpus, canonicalization, and graph-connectivity causes.
       - **Output:** Retrieval failure analysis and improvement targets.

**Tool profile:** Polymer literature, entity canonicalization, vector retrieval, graph retrieval, multi-hop QA, and source evaluation. No public implementation repository was located.

## 29. Multi-Source Heterogeneous Materials KG

1. **Ingest sources:**
       - **Input:** Heterogeneous materials databases, documents, datasets, and structured records.
       - **Process:** Load source-specific entities, properties, processes, and metadata.
       - **Output:** Source-partitioned materials records.
2. **Align and validate:**
       - **Input:** Source schemas, identifiers, values, and provenance fields.
       - **Process:** Normalize schemas and align equivalent records across sources.
       - **Output:** Integrated heterogeneous graph with alignment links.
3. **Expose QA paths:**
       - **Input:** User question, aligned entities, and cross-source relations.
       - **Process:** Traverse multi-hop paths connecting the required source types and facts.
       - **Output:** Complex question-answering context.
4. **Validate integration:**
       - **Input:** Equivalent records, identifiers, values, and provenance assertions.
       - **Process:** Check for contradictory identifiers, values, and source attribution.
       - **Output:** Validated integration and conflict report.
5. **Return source paths:**
       - **Input:** Answer context, source types, aligned relations, and provenance.
       - **Process:** Include the source path supporting each answer claim.
       - **Output:** Multi-source answer with traceable evidence.

**Tool profile:** Heterogeneous materials data, schema and identifier alignment, provenance normalization, graph validation, and complex QA. No public implementation repository was located.

## 30. OptoMatKG

1. **Collect optoelectronic knowledge:**
       - **Input:** Optoelectronic materials literature, material records, device records, and property reports.
       - **Process:** Gather material, device, optical, electronic, and application information.
       - **Output:** Optoelectronic source corpus.
2. **Build traceable relations:**
       - **Input:** Source entities, properties, devices, claims, and provenance metadata.
       - **Process:** Normalize semantic entities and attach every graph fact to its supporting source.
       - **Output:** Provenance-aware optoelectronic KG.
3. **Retrieve semantically:**
       - **Input:** User question, graph entities, properties, and device relations.
       - **Process:** Index and retrieve semantically related graph facts and source passages.
       - **Output:** Optoelectronic evidence context.
4. **Validate properties:**
       - **Input:** Optical/electronic values, units, device context, and source relationships.
       - **Process:** Check value types, units, material-device associations, and source support.
       - **Output:** Validated optoelectronic property records.
5. **Report provenance:**
       - **Input:** Retrieved facts, graph paths, source records, and literature passages.
       - **Process:** Return supporting paths and source links with each result.
       - **Output:** Traceable optoelectronic retrieval answer.

**Tool profile:** Optoelectronic materials literature and data, semantic KG construction, provenance, semantic retrieval, and evidence-grounded QA. No public implementation repository was located.

## Common Construction Pattern

Across these projects, the construction pipeline usually contains the following stages:

```text
source documents, datasets, experiments, or rules
                    |
                    v
       extraction or structured-data loading
                    |
                    v
       normalization, validation, and linking
                    |
                    v
       entities, relations, values, and provenance
                    |
                    v
          graph storage and indexing
                    |
                    v
       retrieval, reasoning, QA, or prediction
```

Their common points are:

- **Domain entities are made explicit:** materials, elements, properties, processes, samples, devices, or experiments become identifiable graph objects.
- **Relations carry the scientific meaning:** composition, synthesis, measurement, application, derivation, and support are represented as links.
- **Normalization is essential:** names, units, identifiers, and values must be made comparable across sources.
- **Provenance is retained:** papers, datasets, experiments, models, and extraction activities are linked to the facts they support.
- **The graph enables more than lookup:** projects use graph structure for multi-hop QA, property derivation, workflow tracing, grounded retrieval, or prediction.
- **Validation improves trust:** schemas, ontologies, unit checks, constraints, and separate measured/predicted values help prevent ambiguous or unsupported results.

In general, an AI4Materials KG is constructed by converting heterogeneous scientific inputs into normalized, typed, provenance-aware entities and relations, then using those relations as the foundation for retrieval, reasoning, or machine-learning tasks.
