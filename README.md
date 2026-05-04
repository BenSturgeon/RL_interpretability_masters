# RL Interpretability — Master's Thesis

Mechanistic interpretability of a maze-solving RL agent that pursues multiple sequential goals. The thesis investigates how the agent internally represents, switches between, and executes those goals.

📄 **Thesis PDF:** [main.pdf](main.pdf)
📝 **Public write-up:** [How Does an Agent with Multiple Goals Choose a Target? (LessWrong)](https://www.lesswrong.com/posts/XWmqSTX89rgqeqyfv/how-does-an-agent-with-multiple-goals-choose-a-target)
💻 **Code:** [BenSturgeon/ai-safety-camp-2024-model-agents](https://github.com/BenSturgeon/ai-safety-camp-2024-model-agents) — most of the experimental work lives here. This repo holds the writing and the parts I was primarily responsible for.

## Setting

- **Environment:** Procgen Heist — the agent collects keys in a fixed order (blue → green → red → gem) to unlock doors.
- **Architecture:** Reduced IMPALA CNN (5 conv layers).
- **Training:** ~800M environment steps with PPO; 60 checkpoints retained for analysis.

## Key findings

1. **Shared-channel encoding.** The network does not dedicate channels to specific targets. Channels are reused across entities, with activation *magnitude* encoding which target is currently active (correlations >0.93).
2. **Offset steering works.** A uniform scalar offset to all activations in a layer redirects behaviour: positive offsets push the agent toward the green key (94% success), negative toward the red key (52%).
3. **Spatial gating.** Negative-activation regions trace the maze structure, marking the path the agent still needs to traverse. As objectives complete, those regions shift positive.
4. **Two-phase architecture.** Conv layers (conv2a–conv4a) handle goal identification (up to 99.3% entity accuracy). Fully connected layers translate the active goal into navigation actions.
5. **SAE corroboration.** Sparse autoencoders reproduce the same patterns, suggesting magnitude-based encoding is a real feature of the representation rather than polysemantic compression.

## Layout

- `main.pdf` — the full thesis.
- `notes/` — working notes and supporting material.
