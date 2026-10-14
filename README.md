<!-- solve-problem:de578945-819e-47ea-a0c2-a525fd404660 -->
# Graph isomorphism FPT in maximum degree

A [Tangle Solve problem](https://solve.tangle.tools/problems/de578945-819e-47ea-a0c2-a525fd404660).

## What is the problem?

Decide whether two graphs \(G_1, G_2\) on \(n\) vertices, each of maximum degree at most \(d\), are isomorphic, in time \(f(d)\cdot n^{O(1)}\) for some computable function \(f\) — i.e., is Graph Isomorphism fixed-parameter tractable (FPT) when parameterized by maximum degree alone?

Formally: find an algorithm and a function \(f:\mathbb{N}\to\mathbb{N}\) such that for all \(G_1,G_2\) with \(\Delta(G_i)\le d\), the algorithm correctly decides \(G_1\cong G_2\) in time \(f(d)\cdot n^{c}\) for a constant \(c\) independent of \(d\). The key constraint: the degree dependence must be *multiplicative* (or at worst in the exponent of \(f\)), not in the exponent of \(n\). A slightly relaxed but widely accepted variant is \(f(d)\cdot n^{O(\log\log n)}\) or similar; the strict FPT form is the canonical statement. Note this is *harder* than the better-known FPT-in-treewidth / FPT-in-(genus, etc.) results, because maximum degree alone gives no a-priori bound on any width parameter (random \(d\)-regular graphs have degree \(d\) and treewidth \(\Theta(n)\), yet are the easy case for color refinement — the hard cases are highly structured group-theoretic instances).

## Why does it matter?

Bounded-degree graphs are the standard abstract model for molecules in chemical informatics, so a strongly degree-dependent algorithm gives scalable exact molecular similarity search. It is also the flagship remaining open case in the group-theoretic approach to GI initiated by Luks: resolving it would likely require new machinery relating permutation-group intersection problems (e.g., stabilizer computation in \(\Gamma_d\)-groups) to polynomial-time algorithms, machinery that would ripple across isomorphism testing for other bounded-arity structures (hypergraphs, CSPs, relational databases of bounded arity).

## What's already known?

- **Luks (1982):** GI on degree-\(d\) graphs in time \(n^{O(d)}\) (via a divide-and-conquer on the recursion tree of the composition series of the automorphism group, which for degree \(d\) lies in \(\Gamma_d\)). This is XP, not FPT; the exponent of \(n\) grows linearly in \(d\).
- **Luks (1982), degree 3:** linear-time. This is genuinely FPT only because \(d=3\) is a constant — the technique does not generalize with constant \(n\)-exponent.
- **Grohe–Neuen–Schweitzer (2020):** isomorphism of degree-\(d\) graphs in time \(2^{O(d\log d)}n^{O(\log d)}\) (quasi-polynomial in \(n\) with logarithmic degree dependence in the exponent; compare Babai's general quasipolynomial bound \(n^{\mathrm{polylog}\,n}\), which this strictly improves for small \(d\)). Their innovation: exploiting the small individualized refinement framework and a "local" version of Luks's recursion keeping the multiplicative factor \(2^{O(d\log d)}\).
- The remaining gap: replace \(n^{O(\log d)}\) by \(n^{O(1)}\). The obstacle is that Luks-style recursion depth depends on \(d\) (via composition-series length of subgroups of \(S_{d}\) wreath products), and each level costs a factor of \(n\); no known technique compresses this depth without paying elsewhere.

## What would count as solving it?

1. **Main target (upper bound):** An algorithm and proof that GI on maximum-degree-\(d\) graphs is decidable in \(f(d)\cdot n^{c}\) time for computable \(f\) and absolute constant \(c\). A referee would want explicit \(f\) and correctness/running-time proofs, plus ideally an implementation-compatibility argument (the recursion should compare against Luks's framework).
2. **Lower-bound variant:** Evidence against FPT, e.g., GI on bounded-degree graphs hard for \(W[1]\) under fpt-reductions (widely believed difficult; no such hardness is known even assuming ETH in a clean form — careful: GI's exact parameterized hardness is open in many directions; treat any claimed hardness statement skeptically).
3. **Partial credit:** \(f(d)\cdot n^{o(\log d)}\), or FPT for every fixed \(d\le 5\)-ish beyond the known degree-3 case.

## How to start

1. Read Luks (1982) thoroughly, especially Section on bounded valence and the analysis of the \(\Gamma_d\)-group recursion; understand exactly where the \(d\) enters the exponent of \(n\) (the composition-series length times branching factor).
2. Read Grohe–Neuen–Schweitzer (arXiv:1802.04659 / SIAM J. Comput. 2020), focusing on how individualization–refinement and their multiplicative \(2^{O(d\log d)}\) term is achieved, and where \(n^{O(\log d)}\) remains unavoidable in their analysis.
3. Study the permutation-group toolkit: Luks's polynomial-time algorithms for bounded-degree permutation groups, the "tower of stabilizers" barrier, and later work by Neuen & Schweitzer on bounded-degree isomorphism (verify exact titles). The key question: can composition-series length of relevant \(\Gamma_d\)-groups be traded against per-level cost using \(f(d)\) multiplicative branching?
4. Main obstacle: high-degree cases where local refinement is useless require individualizing \(\Omega(\log d)\)-many vertices per level, each costing an \(n\)-factor. Look for amortization across levels or a group-theoretic shortcut for \(\Gamma_d\)-intersection.

## Key references

- E. M. Luks, "Isomorphism of Graphs of Bounded Valence Can Be Tested in Polynomial Time," *Journal of Computer and System Sciences* 25(1):42–65, 1982. Introduces the group-theoretic divide-and-conquer giving \(n^{O(d)}\) for degree \(d\) and linear time for degree 3. https://doi.org/10.1016/0022-0000(82)90009-5
- M. Grohe, D. Neuen, P. Schweitzer, "A Faster Isomorphism Test for Graphs of Small Degree," *SIAM Journal on Computing* 49(6), 2020 (arXiv:1802.04659). Achieves \(2^{O(d\log d)}\,n^{O(\log d)}\), the current best for bounded degree. https://arxiv.org/abs/1802.04659 , https://doi.org/10.1137/19M1245293
- L. Babai, "Graph Isomorphism in Quasipolynomial Time," 2016 (arXiv:1512.03547). General quasipolynomial GI; useful background on the group-theoretic barriers. https://arxiv.org/abs/1512.03547

## Warnings

- Do not confuse "FPT in treewidth" (solved) or "polynomial for each fixed \(d\)" (solved by Luks, XP) with FPT in \(d\); the exponent of \(n\) must be independent of \(d\).
- Known counterintuitive fact: random regular graphs are the *easy* instances (color refinement handles them); hard instances must be constructed deliberately — e.g., CFI-type constructions give bounded-degree graphs whose isomorphism complexity resists naive refinement, though these target width, not this exact question.
- Beware claimed hardness: GI is not known NP-hard, and no W[1]-hardness for bounded-degree GI is known; any "easy" lower-bound argument you draft probably fails.
- The census phrase "\(n^{\mathrm{polylog}(d)}\)" is loose; the actual state of the art is \(2^{O(d\log d)}n^{O(\log d)}\) as above.

---
*`ALGO-000097` · unverified*

## Acceptance guidance

1. **Main target (upper bound):** An algorithm and proof that GI on maximum-degree-\(d\) graphs is decidable in \(f(d)\cdot n^{c}\) time for computable \(f\) and absolute constant \(c\). A referee would want explicit \(f\) and correctness/running-time proofs, plus ideally an implementation-compatibility argument (the recursion should compare against Luks's framework).
2. **Lower-bound variant:** Evidence against FPT, e.g., GI on bounded-degree graphs hard for \(W[1]\) under fpt-reductions (widely believed difficult; no such hardness is known even assuming ETH in a clean form — careful: GI's exact parameterized hardness is open in many directions; treat any claimed hardness statement skeptically).
3. **Partial credit:** \(f(d)\cdot n^{o(\log d)}\), or FPT for every fixed \(d\le 5\)-ish beyond the known degree-3 case.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/de578945-819e-47ea-a0c2-a525fd404660?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000097.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000097)
