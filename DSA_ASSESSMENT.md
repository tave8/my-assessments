# DSA Review — Interview-Prep Lens

Reframed around the question that matters here: **as preparation for a mid-level
backend technical interview, what does this code say about data structures &
algorithms ability?** Tests, packaging, and production concerns are intentionally
out of scope.

---

## DSA verdict against the mid-level bar

Mid-level backend interviews lean on a fairly fixed core: hashmaps/sets,
two-pointer, binary search, BFS/DFS on grids/graphs/trees, recursion +
backtracking, heaps, and basic DP — plus the reflex to state Big-O without being
asked. Measured against that bar, this project shows you are **solid and
genuinely above-average on the traversal/recursion/tree slice, and
unproven-to-weak on the rest.** Two specific habits would cost points in a live
interview. Details below, most important first.

---

## What this proves you can do well

**1. Recursion and traversal order are intuitive, not memorized.** The pre-order
vs post-order hook split (`afterOneFutureMove` per-branch vs `afterAllFutureMoves`
after the subtree is exhausted, `MatrixTraverser.py:270` and `:277`) is exactly
the mental model backtracking problems require. You reason about *what the call
stack is doing*, not pattern-match a template. This is the best DSA signal here.

**2. You got the BFS correctness detail that trips most people.** You mark a cell
as enqueued *at enqueue time*, not at dequeue time (`MatrixTraverser.py:334`–`348`),
with a comment explaining that same-level nodes can otherwise race to enqueue the
same child. That duplicate-enqueue bug is a classic interview failure; you avoided
it and articulated *why*.

**3. Tree algorithms are clean and correct.** `findOneByValueFrom`
(`components.py:216`) is a DFS that maintains the ancestor path by
append-before-recurse / pop-on-miss — the canonical backtracking-with-path
pattern. `findKAncestorsOf` walking parent pointers (`:86`) and `countNodesAt` as
a recursive post-order sum (`:330`) are all the right shape.

**4. You understand the visited-set trade-off.** The engine visits each cell once
(permanent `visited` matrix) and exposes `onMultipleVisitMustStop` to control
revisits. You correctly built a *traversal/spanning tree* and know this is for
reachability/one-path, not all-paths. Knowing *why* you mark visited — and what
you give up — is a level above "I always add to a visited set."

**5. Grid math is correct and slightly sophisticated.** `isDistant`
(`components.py:859`) uses `max(|Δrow|, |Δcol|)` — Chebyshev/king-move distance —
the *right* metric for 8-directional movement and consistent with your 8-move
set. Most people reach for Manhattan distance and get diagonal adjacency wrong.

So: on problems that reduce to "traverse a grid/graph/tree with DFS or BFS and
track some state," you're in good shape — arguably better than the median
mid-level candidate, because you can *generalize* the pattern, which is harder
than solving one instance.

---

## The two things that would actually lose points in an interview

**1. Efficiency reflexes aren't firing.** Both BFS loops dequeue with
`queue.pop(0)` (`MatrixTraverser.py:318`, `components.py:386`). `list.pop(0)` is
**O(n)**, so your "BFS" is **O(V²)**, not O(V+E). An interviewer will catch this
instantly. Fix: `collections.deque` + `popleft()` — and `deque` appears nowhere
in the project, which is the tell. Equally important: there is **not a single
Big-O annotation anywhere**. At mid-level you're expected to *volunteer*
complexity. Treat `pop(0)`, `list.insert(0, ...)`, and repeated `in` on a list as
code smells.

**2. You over-build, and interviews are time-boxed.** Your instinct is to build a
callback engine, a manager layer, an exception hierarchy. That's good engineering
and a positive signal for the *job* — but in a 35-minute interview, the candidate
who writes a tight 15-line BFS beats the one architecting an abstraction. Drill
the opposite of what this repo rewards: solve the *specific* problem in the
*fewest* lines, fast.

---

## Breadth: the honest gap

This project exercises one quadrant of the DSA map deeply and leaves the rest
untouched. Confirmed absent across the source: no heap (`heapq`), no binary
search (`bisect`), no DP/memoization, no sorting, no union-find, no weighted
shortest path. Categories with **no evidence yet** that show up constantly at
mid-level:

- **Heaps / priority queues** → and by extension **Dijkstra / A\***. The natural
  next step from your BFS; weighted grids are a top-tier interview topic you're
  one concept away from.
- **Dynamic programming** — grid DP especially (min path sum, unique paths) sits
  right on top of the matrices you're already comfortable with. Highest-value gap
  given your existing grid fluency.
- **Binary search** (on arrays *and* on answer space).
- **Two-pointer / sliding window** and **hashmap-pattern** problems — the
  bread-and-butter array/string round.
- **Backtracking with state restoration** — your engine uses *permanent* visited
  marks, so you've never had to *unmark on the way out* (permutations, N-queens,
  word search). That undo-step is its own muscle.

---

## What to drill next (ordered by ROI)

1. **Re-implement BFS with `deque` and annotate Big-O on everything you've already
   written.** Fastest way to turn a weakness into a talking point.
2. **Grid DP** (min path sum, unique paths, coin change) — leverages grid comfort
   you already have; closes the biggest-value gap.
3. **Heaps → Dijkstra on a weighted grid** — literally your BFS with a priority
   queue; you're closer than you think.
4. **Binary search + two-pointer/sliding-window + hashmap drills** — the
   array/string round you have no reps on.
5. **Explicit backtracking with unmark** (word search, permutations) — fills the
   one hole inside your strongest area.

---

## Bottom line

The *depth* of understanding on display (recursion timing, BFS enqueue-marking,
the visited trade-off) is real and says you can genuinely *learn* DSA rather than
memorize it. The work now is **breadth + speed + complexity-as-a-reflex**, not
fundamentals. You're not starting from zero on the hard part — you're filling in
a map whose hardest region you've already shown you can navigate.
