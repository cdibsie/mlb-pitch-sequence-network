# MLB Pitch Sequencing as a Markov Network

This project models how MLB pitchers sequence their pitches as a 36-state Markov chain, built from every pitch of the 2025 regular season (656,050 pitches from Statcast). I built it as a course project for my M.E. in Systems Engineering at the University of Virginia.

## What it does

- **Data pipeline:** pulls the full 2025 season from MLB Statcast through `pybaseball`, then cleans and labels each pitch by type (fastball, breaking ball, off-speed) and count.
- **Network model:** builds a directed, weighted network of pitch-to-pitch transitions within at-bats, with 36 states and 261 transitions.
- **Analysis:** PageRank and betweenness centrality, Louvain community detection, strong connectivity and clustering.
- **Reusable model:** the `PitchSequenceMarkovChain` class predicts the most likely next pitch from any pitch and count, and simulates full at-bats.

```python
markov = PitchSequenceMarkovChain(G)
markov.predict('Fastball', '3-0', top_n=3)
# → Fastball @ 3-1 (80.5%), Breaking Ball @ 3-1 (11.8%), Off-Speed @ 3-1 (6.6%)
```

## Key findings

- **Full counts dominate.** Fastball at 3-2 has the highest PageRank (0.42).
- **The count splits into three phases.** Community detection separates early counts, pitcher-ahead counts and hitter-ahead or deep counts.
- **Off-speed pitches link the phases.** Off-speed states have the highest betweenness centrality.

## Run it

Open `MLB_Pitch_Sequence_Network.ipynb` in Google Colab and choose **Runtime → Run all**. Pulling the data takes about 3–5 minutes.

**Built with:** Python, pandas, NumPy, NetworkX, python-louvain, matplotlib, seaborn, pybaseball
