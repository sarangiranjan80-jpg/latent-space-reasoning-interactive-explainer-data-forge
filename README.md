Recurrent Latent-Space
Reasoning — Interactive
Explainer
DataForge 2026 × Pathway × rime — Pathway
Track submission

LIVE ARTIFACT-https://sarangiranjan80-jpg.github.io/latent-space-reasoning-interactive-explainer-data-forge/
 

The claim
A model can improve an answer by repeatedly
transforming a fixed-size hidden state a variable
number of times — without ever converting an
intermediate step into a word or token.
The interactive artifact ( index.html ) lets a learner
test this directly: turn the reasoning-effort dial down to
zero and the model's belief stays flat and its answer is
poor; turn it up and the belief sharpens on the correct
hypothesis and accuracy rises. If this claim were false,
the accuracy curve would not respond to the dial —
that's the falsification test built into the page.

Who this is for

An audience with basic ML familiarity: someone who
knows what a hidden state and a Transformer are, but
has not seen recurrent/latent reasoning (as opposed
to chain-of-thought) before.
Prerequisites: basic familiarity with neural network
terminology (parameters, hidden state, softmax). No
prior knowledge of BDH, BDH-CQ, or ARC-AGI is
assumed — both are explained in-page.

Learning objectives

After using this artifact, a learner should be able to:
1. Explain the structural difference between verbal
chain-of-thought and recurrent latent reasoning.
2. Describe, in their own words, what a "reasoning
effort" dial actually controls (iteration count over a
fixed-size state — not model size).
3. State how BDH provides the underlying fixed-size,
continuously-updatable state, and how BDH-CQ
uses that state for two distinct jobs (contextual
memory from demonstrations, and iterative latent-
space query solving).

4. Name at least one documented limitation of BDH-
CQ's approach (e.g. its reported weaknesses in
ordering, nesting, and certain compositions).

Architecture of the artifact

Single self-contained HTML file ( index.html ), vanilla
JavaScript, no external dependencies or build step
required.

COMPONENT	ROLE	STATUS
Demonstration
panel	Shows 2
input→output
example grids that
encode an unstated
transformation rule
(a positional shift + a
color swap)	Live — grids
Generated deterministically
at page load
Reasoning-
effort control	LOW/MEDIUM/HIGH
buttons or an exact
0–20 slider	Live — drives
real
recomputation
Latent belief
bars	Visualizes the top-5
Hypothesis probabilities as
iteration count change	Live — real
softmax-over-
evidence
computation,
recalculated on
every interaction,
never converted to
text
Query comparison (input / model
answer /
ground truth)	Applies the model's
current best hypothesis to a new
grid and shows it
beside the correct
answer	Live
Accuracy
readout	Cell-match
percentage between
model answer and
ground truth	Live, computed
each interaction
"Verbal chain-
of-thought"
panel	Shows what a token-
based reasoning
trace on the same
task would look like	Illustrative,
static — explicitly labeled in-page as not generated live
BDH / BDH-CQ module	Explains architecture
provenance and
cites verified
published numbers	Reference
data, sourced
from the primary papers,
not live model
output
Limitations callout	States a real documented BDH-
CQ weakness	Reference data



The underlying algorithm (what
"live" actually means here)

1. A small hypothesis space is built: 9 possible grid
shifts × 6 possible color permutations = 54
candidate rules.
2. Each hypothesis is scored against the two
demonstration pairs by counting matching cells.
3. As the effort dial increases, evidence for each
hypothesis accumulates linearly with iteration
count, then passes through a softmax — so more
"iterations" genuinely sharpen the belief
distribution toward the best-supported
hypothesis. This mirrors, at toy scale, how a
continuous hidden state can be fed back and
repeatedly refined instead of decoded to text at
every step, as first demonstrated for autoregressive LLMs by Hao et al.,2024(arXiv:2412.06769), and how recurrent-depth
models repeatedly apply a shared transformation
to improve reasoning with more iterations, as
studied by Geiping et al., 2025 (arXiv:2502.05171).
4. The current best hypothesis (by belief probability)
is applied to a held-out query grid, and compared
cell-by-cell against the true answer. The
theoretical basis for why iterative latent
computation can substitute for additional
depth/parameters — rather than needing a bigger
model — follows the looped-transformer analysis
in Saunshi et al., 2025 (arXiv:2502.17416).

Falsifiability

The claim above is written so the artifact can disprove
it: if recurrent latent iteration did not improve
reasoning, the accuracy readout would stay flat
regardless of the effort-dial position. The in-page
callout above the demo states this test explicitly and
invites the learner to run it (set effort to 0, then HIGH,
and compare accuracy).
This is an original toy model built for teaching
purposes. It is not BDH-CQ, does not use BDH-CQ's
weights or unpublished implementation details, and
does not reproduce BDH-CQ's reported benchmark
numbers.

BDH / BDH-CQ integration

BDH (Dragon Hatchling) provides a brain-inspired
sequence-model substrate: attention emerges
from local neuron-to-neuron interactions on a
sparse, scale-free graph, updated through
Hebbian-style rules, giving a persistent state
instead of a growing token cache.

BDH-CQ builds two things on top of that
substrate: a recurrent contextual memory
updated by demonstrations (analogous to our two
demo grids), and a separate latent workspace
transformed repeatedly to solve a query
(analogous to our effort dial) — with no parameter
updates at inference and no decoding of
intermediate states into language.

Reported results (from the primary paper,
arXiv:2608.09888): a 150M-parameter
configuration reaches 29.5% pass@2 / 24.25%
pass@1 on the public ARC-AGI-1 evaluation set at
a computed cost of ~$0.00070/task. Reasoning
effort scales accuracy: LOW 21% → MEDIUM 27%
→ HIGH 29.5% pass@2. 
Documented limitation: BDH-CQ extrapolates
well on boundary-propagation and copying tasks,
but performance drops sharply on long ordering
sequences, deep nesting, conditional rule
selection, and certain operation compositions (e.g.
color-swap composed with relocation was solved
0/72 times).

Full citation list with exact placement guidance: see
CITATIONS.md .

Reproducing / running this artifact
No build step, no dependencies, no server required.

1. git clone - https://sarangiranjan80jpg.github.io/recurrent-latent-reasoning/
2. cd <repo-folder>
3. open index.html # or double-click it, or

serve it with: python3 -m http.server
Everything runs client-side in vanilla JavaScript. There
is no notebook or backend component to configure for
this submission.

Credits and licenses

See DISCLOSURE.md for the full AI-assistance, code,
data, and asset log, and license details for each
component.
Primary sources
Engdahl, B. et al. (2026). BDH-CQ: In-Context
Learning with Recurrent Latent Reasoning.
arXiv:2608.09888.
Kosowski, A. et al. (2025). The Dragon Hatchling:
The Missing Link between the Transformer and
Models of the Brain. arXiv:2509.26507.
Full list with placement guidance in
CITATIONS.md .
