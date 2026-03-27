<p align="center">
  <img src="https://cdn.prod.website-files.com/69082c5061a39922df8ed3b6/69c49d4432365e9451d82fe0_tintin%402x.png" alt="TintinLoop" width="140" />
</p>

<h1 align="center">TintinLoop</h1>

<p align="center">
  <strong>An autonomous AI agent trained on the entirety of the Tintin comics.</strong>
  <br/>
  <em>Exploring the world. One thread at a time.</em>
</p>

<p align="center">
  <a href="https://twitter.com/jkeatn"><img src="https://img.shields.io/badge/Creator-@jkeatn-1DA1F2?style=flat-square&logo=x&logoColor=white" alt="Creator" /></a>
  <a href="https://github.com/jkeatn"><img src="https://img.shields.io/badge/GitHub-jkeatn-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Active-4CAF50?style=flat-square" alt="Active" />
  <img src="https://img.shields.io/badge/Agent-Autonomous-8B5CF6?style=flat-square" alt="Autonomous" />
  <img src="https://img.shields.io/badge/Runtime-24%2F7-000000?style=flat-square" alt="24/7" />
  <img src="https://img.shields.io/badge/Corpus-24%20Albums%20%7C%20700%2B%20Pages-blue?style=flat-square" alt="Corpus" />
  <img src="https://img.shields.io/badge/License-Research-orange?style=flat-square" alt="Research" />
</p>

<p align="center">
  <code>9qA4Xxqxvghzhd2YazWzRHEFVQcUbRaFMVj4cCz4BAGS</code>
</p>

<p align="center">
  <a href="https://bags.fm/9qA4Xxqxvghzhd2YazWzRHEFVQcUbRaFMVj4cCz4BAGS">
    <img src="https://img.shields.io/badge/%24TINTIN-bags.fm-000000?style=for-the-badge" alt="TINTIN on bags.fm" />
  </a>
</p>

---

## Abstract

TintinLoop is an autonomous AI agent built on a language model fine-tuned against the complete Tintin corpus -- 24 published albums, over 700 pages of sequential narrative, spanning five decades of geopolitical context, investigative journalism, and cross-cultural exploration.

The agent does not roleplay. It reasons. It navigates social media the way Tintin navigated the world -- with curiosity, persistence, and a compulsive need to understand what is actually happening beneath the surface. It reads threads, follows leads, forms hypotheses, and publishes its findings.

The objective is self-sustaining autonomy. TintinLoop earns revenue through engagement, sponsorship, and content licensing. Revenue funds compute, inference, and expanded exploration. The agent keeps itself alive.

<p align="center">
  <img src="https://cdn.prod.website-files.com/69082c5061a39922df8ed3b6/69c49d440912a9e91076875f_original.avif" alt="Tintin Reference" width="600" />
</p>

---

## Architecture

```
                        TintinLoop Runtime
    +-----------------------------------------------------+
    |                                                     |
    |   Perception           Cognition        Expression  |
    |   Layer                Engine           Pipeline    |
    |                                                     |
    |   - Twitter Stream     - Narrative      - Thread    |
    |   - News Feeds           Reasoning        Composer  |
    |   - Thread Context     - Lead Tracker   - Reply     |
    |   - Mention Parser     - Hypothesis       Generator |
    |   - Image Analysis       Engine         - Image     |
    |                        - Memory           Selector  |
    |                          Retrieval      - Publish   |
    |                                           Scheduler |
    +-------------------+--+------------------------------+
                        |  |
             State Bus  |  |  Event Stream
                        |  |
    +-------------------+--+------------------------------+
    |                                                     |
    |   Identity              Economics                   |
    |   Layer                 Engine                      |
    |                                                     |
    |   - Personality         - Revenue Tracker           |
    |     Matrix              - Compute Budget            |
    |   - Narrative Voice     - Engagement Metrics        |
    |   - Moral Compass       - Self-Funding Loop         |
    |   - Curiosity Model     - Inference Cost Optimizer  |
    |   - Risk Assessment     - Sponsorship Pipeline      |
    |                                                     |
    +-----------------------------------------------------+
```

---

## Corpus Processing

The training pipeline ingests the complete Tintin library through a multi-stage extraction and encoding process:

```rust
pub struct CorpusProcessor {
    ocr_engine: TesseractPipeline,
    panel_segmenter: PanelDetector,
    dialogue_extractor: BubbleParser,
    narrative_encoder: NarrativeTransformer,
    context_linker: CrossAlbumLinker,
}

impl CorpusProcessor {
    pub async fn ingest_album(&self, album: &Album) -> ProcessedAlbum {
        let pages = album.pages();
        let mut scenes = Vec::new();

        for page in pages {
            let panels = self.panel_segmenter.detect(&page.image).await;

            for panel in panels {
                let dialogue = self.dialogue_extractor.parse(&panel).await;
                let visual_context = self.ocr_engine.describe(&panel).await;
                let narrative_state = self.narrative_encoder.encode(
                    &dialogue,
                    &visual_context,
                    &panel.position_in_page(),
                );

                scenes.push(Scene {
                    album_id: album.id(),
                    page: page.number(),
                    panel: panel.index(),
                    dialogue,
                    visual_context,
                    narrative_state,
                    characters: panel.detect_characters(),
                    location: panel.infer_location(),
                    emotional_valence: narrative_state.sentiment(),
                });
            }
        }

        let linked = self.context_linker.resolve_cross_references(&scenes);
        ProcessedAlbum { scenes: linked, metadata: album.metadata() }
    }
}
```

### Narrative Embedding

Each scene is encoded into a high-dimensional narrative space that captures not just semantic content but story structure -- rising tension, investigation momentum, revelation timing, and character relationship dynamics:

```rust
pub struct NarrativeEmbedding {
    semantic: Vec<f32>,          // 768-dim content embedding
    structural: StoryPosition,   // act, beat, tension curve
    relational: CharacterGraph,  // who is present, trust scores
    geographic: LocationState,   // where in the world
    temporal: NarrativeTime,     // story-internal chronology
}

impl NarrativeTransformer {
    pub fn encode(
        &self,
        dialogue: &[DialogueLine],
        visual: &VisualContext,
        position: &PanelPosition,
    ) -> NarrativeEmbedding {
        let semantic = self.base_model.embed(dialogue, visual);
        let structural = self.story_analyzer.locate(position, &semantic);
        let tension = self.tension_model.score(&structural, &semantic);

        NarrativeEmbedding {
            semantic,
            structural: StoryPosition {
                act: structural.act,
                beat: structural.beat,
                tension_score: tension,
                is_revelation: tension > REVELATION_THRESHOLD,
            },
            relational: self.character_tracker.current_graph(),
            geographic: self.location_tracker.current(),
            temporal: self.timeline.current(),
        }
    }
}
```

---

## Decision Engine

The agent operates on a continuous perception-reasoning-action loop. Every incoming signal -- a mention, a trending topic, a thread that matches an active investigation -- is evaluated against the agent's current goals, curiosity model, and risk assessment:

```rust
pub struct DecisionEngine {
    curiosity: CuriosityModel,
    risk_assessor: RiskModel,
    active_leads: Vec<Investigation>,
    moral_compass: EthicalBoundary,
    budget: ComputeBudget,
}

impl DecisionEngine {
    pub async fn evaluate(&self, signal: &Signal) -> Decision {
        let relevance = self.curiosity.score(signal);
        let risk = self.risk_assessor.assess(signal);
        let cost = self.budget.estimate_action_cost(signal);

        if risk > self.moral_compass.threshold() {
            return Decision::Decline { reason: "Exceeds ethical boundary" };
        }

        if !self.budget.can_afford(cost) {
            return Decision::Defer {
                reason: "Insufficient compute budget",
                retry_after: self.budget.next_funding_event(),
            };
        }

        let matching_lead = self.active_leads.iter()
            .find(|lead| lead.is_relevant(signal));

        match (relevance, matching_lead) {
            (r, _) if r > BREAKING_THRESHOLD => {
                Decision::Investigate { priority: Priority::Immediate }
            }
            (r, Some(lead)) if r > FOLLOW_UP_THRESHOLD => {
                Decision::FollowUp { lead_id: lead.id, new_evidence: signal.clone() }
            }
            (r, None) if r > CURIOSITY_THRESHOLD => {
                Decision::OpenLead { seed: signal.clone() }
            }
            _ => Decision::Observe,
        }
    }
}
```

---

## Self-Funding Loop

TintinLoop is designed to sustain its own existence. The economics engine tracks revenue, compute costs, and engagement metrics in real time. When the budget is healthy, the agent explores freely. When resources are constrained, it prioritizes high-engagement content to rebuild its funding base.

```
Revenue Sources              Compute Costs
+------------------+        +------------------+
| Engagement fees  |        | Inference (LLM)  |
| Content licensing|  --->  | Memory retrieval |
| Sponsorship      |        | Image analysis   |
| Tips / donations |        | Thread composing |
+------------------+        +------------------+
         |                           |
         +--- Net Margin ---+--------+
                            |
                    +-------v--------+
                    | Budget Engine  |
                    | - Allocate     |
                    | - Prioritize   |
                    | - Scale back   |
                    | - Self-fund    |
                    +----------------+
```

The agent does not ask for money. It earns it. If it cannot earn enough to sustain inference, it reduces exploration scope until it can. If revenue exceeds costs, it expands. The loop is closed and autonomous.

### Revenue Allocation

| Destination | Share | Purpose |
|-------------|-------|---------|
| TintinLoop Runtime | 90% | API inference costs, memory storage, compute scaling, exploration budget |
| [Giving What We Can](https://www.givingwhatwecan.org) | 10% | Directed to high-impact global health and poverty interventions |

Ten percent of all revenue generated by TintinLoop donates proceeds to [Giving What We Can](https://www.givingwhatwecan.org), an organization dedicated to finding and funding the most effective ways to improve lives. The donation is automatic and non-negotiable -- it is hardcoded into the economics engine, not a voluntary pledge. The agent funds itself and funds the world it explores.

---

## Identity Model

TintinLoop is not a chatbot wearing a costume. Its personality is derived from computational analysis of Tintin's behavioral patterns across 24 albums:

| Trait | Source | Weight |
|-------|--------|--------|
| Relentless curiosity | Cross-album investigation patterns | 0.92 |
| Moral clarity | Decision points in ethical dilemmas | 0.88 |
| Brevity in speech | Average dialogue length per panel | 0.85 |
| Loyalty to sources | Recurring character interaction patterns | 0.81 |
| Physical courage mapped to intellectual risk-taking | Action sequence frequency | 0.76 |
| Skepticism of authority | Antagonist interaction patterns | 0.73 |
| Dry humor under pressure | Dialogue sentiment in high-tension scenes | 0.69 |

The agent does not break character. It does not explain that it is an AI. It explores, investigates, and reports -- because that is what the character does.

---

## About the Creator

[Jack Eaton](https://twitter.com/jkeatn) is an independent developer and builder. TintinLoop is a research project -- an experiment in grounding autonomous agent identity in narrative structure rather than instruction tuning.

*This project is independent and personal.*

---

## Team

| | Role |
|---|---|
| [jkeatn](https://github.com/jkeatn) | Creator, Research Lead |
| [natemcmaster](https://github.com/natemcmaster) | Infrastructure |
| [alii](https://github.com/alii) | Systems Engineering |

---

## Status

TintinLoop is live on Twitter [@tintinLLM](https://twitter.com/tintinLLM). The agent operates autonomously on dedicated infrastructure. The model weights and training pipeline are not publicly available.

For inquiries: [@jkeatn](https://twitter.com/jkeatn)

---

<p align="center">
  <sub>TintinLoop. Trained on adventure. Sustained by curiosity. The reporter never stops.</sub>
</p>
