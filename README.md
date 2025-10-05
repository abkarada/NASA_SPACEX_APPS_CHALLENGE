Introduction

This work introduces an innovative approach towards anomalous detection in SAR data utilizing recursive searching techniques from the Stockfish chess program.
Traditional methods recognize only prominent outliers but tend to overlook feeble outliers, and time-series techniques involve very high computational expenses.

Our approach integrates iterative deepening, alpha-beta pruning, and quiescence refinement to effectively indicate strong and weak outliers efficiently.

Data Preprocessing

SAR data is preprocessed with ESA SNAP (snappy API):

Orbit correction → accurate geolocation.

Radiometric calibration → normalize backscatter (Sigma0).

Speckle filtering → suppress radar-specfic noise.

Terrain correction → remove geometric distortions.

Coregistration → aligning before/after pics for change detection.

A stack product that is ready for product and before/after bands for feature extraction.

Feature Extraction

From each of the tile (window) of the SAR stack, multiple features are extracted:

Log-difference (responsive to small backscatter variations)

Ratio (proportional variation from before/after

Coherence (phases stability in multiple acquisitions)

Texture (structural variation) (GLCM contrast)

Each tile is described in terms of its corresponding feature vector, in basis for anomaly scoring.

Base Anomaly Score

is calculated for each variant in simple form as the weighted sum of the z-scores:

score = 0.35⋅z(log-diff) + 0.35⋅z(ratio) + 0.20⋅z(texture) + 0.10⋅z(1−coherence)

High IPI = High Anomaly Probability

Used for primitive ranking and move ordering.

Search Strategy (Following Stockfish)

Searching procedure swaps the brute-force scanning for a recursive decision tree:

Iterative deepening → shallow first, deep analysis only on suspicious tiles.

Alpha-beta pruning → trim branches that cannot achieve more optimistic outcomes.

Move ordering → reduce high-scoring tile analysis for effective pruning.

Quiescence refinement → local re-evaluation of borderlines.

Transposition table → memoization to prevent redundant computation.

This ensures both strong and week anomalies are picked in small time.

Iterative Deepening Controller

Starts at all root tiles.

At every iteration:

Evaluate tiles with recursive SEARCH(node, depth, α, β) trips.

Retain only the highest 40% suspiciously associated tiles.

Add search depth.

Ends when time budget (e.g., 60s) is depleted.

Output

Pipe Line Generates:

High outliers → Top 5% (very strong, obvious changes).

Weak anomalies → 80–95th percentile (weak but genuine changes).

Final anomaly map → visualization-level scores for tile-level mapping.

Mer

Efficiency → avoids brute-force scanning of all tiles.

Sensitivity → captures shallow anomaly hidden in noise.

Scalability → Mouldable for time-series SAR with multi-acquisition.

Innovation → first time SAR anomaly detection is integrated with chess-engine search strategies.

Further Work

Add to time-series SAR with more dates.

Utilize machine learning (autoencoders, isolation forests) for preliminary anomaly grading.

Parallelize on GPU/cluster for real-time anomaly detection.

Use in disaster tracking: floods, forest fires, deforestation, urbanization.

 Summary in “This algorithm highlights weak anomalies using recursive chess-engine search algorithms and SAR data.”
# NASA_SPACEX_APPS_CHALLENGE
