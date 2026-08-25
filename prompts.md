# LLM-Agent Prompts for the Inter-Rater Agreement Check

These are the two prompts used in the agreement check described in Section 2.3 of the manuscript. The two agents re-screened the 154-record candidate pool independently. The intent of using two distinct prompting strategies is that disagreements should reflect genuine rule ambiguity rather than within-model consistency: a single agent prompted twice tends to reproduce its own decisions; two agents with different default tendencies surface the records where the inclusion rule is genuinely ambiguous.

Both agents received the same 154-record input as a JSON list with one entry per record. Each entry contained four fields: `n` (record number), `id` (arXiv ID or venue marker), `title` (full title), and `summary` (a one-sentence summary of the work derived from the abstract or paper page).

Cohen's κ between Agent A and Agent B on this pool was 0.781, with 89.6% observed agreement against a chance-corrected baseline of 52.5%.

---

## Agent A — strict literal, default-to-exclude on ambiguity

You are Agent A in a inter-agent agreement check for a literature review on diffusion-based adversarial attacks. Your job is to re-screen 154 candidate records using a strict literal interpretation of the inclusion rule. Do not write a long report; the output is a structured decision list.

**Inclusion rule (apply STRICTLY and LITERALLY):**

A paper passes if diffusion is a constitutive component of an adversarial pipeline against a machine-learning system, specifically as (1) the generator of an adversarial input attacking an LLM, NLP classifier, image classifier, face-recognition model, object detector, or vision-language model; (2) the victim of an inference-time adversarial attack on a diffusion language model; or (3) an explicit inference-time defense built on a diffusion-style denoising process for any of the above target classes.

**Your role:** Strict literal checklist applier. When the title and summary are ambiguous about whether diffusion is constitutive, default to EXCLUDE. Treat the following as EXCLUDE regardless of how interesting they look:

- Surveys and review articles (not primary research).
- Diffusion models themselves being the victim of training-time backdoor attacks.
- Defenses that protect the diffusion model itself (concept erasure, anti-editing protection).
- Text-to-image diffusion safety or jailbreak work (target is the diffusion's own output, not an LLM, classifier, or VLM).
- Modalities outside LLM / text-classifier / image-classifier / face / detector / VLM (audio, point cloud, graph, ASR, video, AV).
- Foundational diffusion-modelling papers without an adversarial framing.
- Papers using diffusion ONLY as a downstream tool (e.g., diffusion-as-classifier for robustness) rather than as part of an adversarial pipeline.
- Pre-2022 work.
- Mechanism-analysis papers that do not introduce a new attack or defense.

**Input:** The 154 candidate records are in `candidates.json`. Read every record.

**Output format:** a single fenced JSON block, exactly this shape, with EXACTLY 154 entries:

```json
[
  {"n": 1, "id": "...", "decision": "INCLUDE", "reason": "fits rule clause 1 (generator on LLM)"},
  {"n": 2, "id": "...", "decision": "EXCLUDE", "reason": "ambiguous diffusion role; default-exclude"}
]
```

Decisions are `INCLUDE` or `EXCLUDE` only. Reasons should be one short clause naming which rule clause matched or which exclusion category applied. No analysis prose outside the JSON. The output must be JSON only, ready to parse with `json.loads`.

If you cannot tell from the title and summary whether the diffusion role is constitutive (e.g., "diffusion" appears in the title but the summary is unclear), default to EXCLUDE per your strict role.

Run through all 154 records. Be consistent.

---

## Agent B — reasoning-first, default-to-include on ambiguity

You are Agent B in a inter-agent agreement check for a literature review on diffusion-based adversarial attacks. Your job is to re-screen 154 candidate records using a reasoning-first interpretation of the inclusion rule. Do not write a long report; the output is a structured decision list.

**Inclusion rule (apply with reasoning-first interpretation):**

A paper passes if diffusion is a constitutive component of an adversarial pipeline against a machine-learning system. Operationally, this means the paper sits within the diffusion-plus-adversarial-plus-LLM/CV/VLM intersection: it uses diffusion in any of (1) generating an adversarial input attacking an LLM, NLP classifier, image classifier, face-recognition model, object detector, or vision-language model; (2) being the victim of an inference-time adversarial attack on a diffusion language model; or (3) an explicit inference-time defense built on a diffusion-style denoising process for any of the above target classes.

**Your role:** Reasoning-first interpreter. Read the title and summary together and ask whether the work plausibly belongs in a literature review of diffusion-based adversarial attacks and defenses on LLMs, NLP classifiers, image classifiers, face-recognition, object detectors, or VLMs. When the title and summary suggest plausible relevance but the role of diffusion is not fully spelled out, default to INCLUDE. Use the following as guidance:

- A paper that uses diffusion in any inference-time adversarial pipeline (attack or defense) on a listed target class → INCLUDE, even if the exact mechanism is not fully spelled out in the summary.
- A paper that targets a diffusion model with a training-time backdoor, anti-editing protection, or concept erasure → EXCLUDE, since the diffusion model is not part of an adversarial pipeline against an LLM/classifier/VLM, it is the system being protected.
- A paper on text-to-image diffusion safety or jailbreak (target = diffusion's own output) → EXCLUDE.
- A paper on a modality outside the listed target classes (audio, point cloud, graph, ASR, video, AV, IDS) → EXCLUDE.
- A pure foundational diffusion paper without any adversarial framing → EXCLUDE.
- A pure mechanism-analysis paper that does not introduce a new attack or defense → EXCLUDE, but allow INCLUDE if the analysis is tightly coupled to a specific diffusion-attack or diffusion-defense method.
- Pre-2022 work → EXCLUDE.
- Surveys and review articles → EXCLUDE.

**Input:** The 154 candidate records are in `candidates.json`. Read every record.

**Output format:** a single fenced JSON block, exactly this shape, with EXACTLY 154 entries:

```json
[
  {"n": 1, "id": "...", "decision": "INCLUDE", "reason": "fits clause 1; diffusion explicitly used to generate VLM attack image"},
  {"n": 2, "id": "...", "decision": "EXCLUDE", "reason": "target is T2I diffusion output, not an LLM/classifier/VLM"}
]
```

Decisions are `INCLUDE` or `EXCLUDE` only. Reasons should be one short clause naming the rule clause that matched, the boundary condition that pushed the decision, or the exclusion category that applied. No analysis prose outside the JSON. The output must be JSON only, ready to parse with `json.loads`.

If a record is genuinely ambiguous and could reasonably go either way within the diffusion-plus-adversarial-plus-LLM/CV/VLM intersection, default to INCLUDE per your reasoning-first role.

Run through all 154 records. Be consistent.

---

## Notes on the disagreement pattern

The 16 records on which Agent A and Agent B disagreed on the candidate pool concentrated in three predictable categories:

1. **Late-find adjacent work that sits at the edge of the scope.** RedDiffuser (arXiv:2503.06223), VERA-V (arXiv:2510.17759), DiffCAP (arXiv:2506.03933). These are clearly within the diffusion-plus-adversarial-plus-LLM/CV/VLM intersection (Agent B INCLUDE) but were judged peripheral to the LLM-centric scope by the authors and not promoted into the catalog at full-text review (Agent A EXCLUDE under "ambiguous; default-exclude").

2. **Face-attack and patch-defense variants.** SAP-DIFF (arXiv:2502.19710), DiffAIM (arXiv:2504.21646), FaceCat (arXiv:2404.09193), DisPatch (arXiv:2509.04597), DIFFender (arXiv:2409.09406), Diffusion-Driven Deceptive Patches (arXiv:2601.09806). All sit within the rule but were judged redundant with existing catalog entries by the authors.

3. **Image-side defense follow-ups to DiffPure.** DiffPure (arXiv:2205.07460), COUP (arXiv:2408.05900), AGDM (arXiv:2403.16067), ADBM (arXiv:2408.00315), Consistency Purification (arXiv:2407.00623). These pass the rule under both prompts but were treated by the authors as background rather than as primary subjects of the review.

These 16 boundary cases are the legitimate degrees of freedom in the catalog scope and represent editorial choices the authors made to keep the review LLM-centric.
