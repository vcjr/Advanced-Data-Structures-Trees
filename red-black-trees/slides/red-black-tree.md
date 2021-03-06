@snap[midpoint span-100]

# Red-Black Tree Rules

@snapend

---

## Learning Goals

By the end of this module, students will be able to...

* **Define** the terms red-black tree, black-depth and rebalance
* **Explain** how, by following a set of rules, red-black trees can keep themselves balanced as they grow
* **List** the red-black tree rules

---

## Balance - Review

A tree is **balanced** if no leaf is more than twice as deep as any other

Balance directly affects the height `\(h\)` of a tree of size `\(n\)`:

`\[ h = \begin{cases} O(log(n)) & \quad \text{for a balanced tree} \\ O(n) & \quad \text{for an unbalanced tree} \end{cases} \]`

For a BST, `insert`, `lookup` and `delete` are `\(O(h)\)` time, and `forEach` is `\(O(h)\)` space

---

## Trigger Warning

Red-black trees are unfortunately named

<ul class="small">
<li>Nodes are red or black, we'll treat them differently based on color</li>
<li>We'll be talking about red parents, black children, etc</li>
</ul>

<p class="small fragment">Invented in 1972 by a German scientist named Rudolf Bayer</p>

---

## Complexity Warning

This is our first really complicated algorithm

<ul class="small">
<li>Big ideas first, then details</li>
<li>Draw a picture</li>
<li>Rewrite the steps (and their purpose) in your own words</li>
<li>Take a break if you need it</li>
</ul>

---

## Red-Black Trees

Goal: guarantee that the height of a BST is `\(O(log(n))\)`

<p class="small">In other words, keep the tree **balanced**</p>

How? **Rebalance** the tree after every `insert` and `delete`

To help us rebalance, we'll **color each node red or black**

We'll define rules about how colors interact

---

## RBT Rules

1. **BST Property** still holds
1. The root is always black
1. Leaves and the root's parent count as black
1. You can't have 2 red nodes in a row
   - 2 black nodes is OK
   - In other words, if a node is red, both its children must be black
1. Number of black nodes is the same for every path from a leaf to the root
   - We'll call this the **black-depth** of a node

<p class="small">After every insert or delete we'll "fix" the tree so it still follows the rules</p>

---

## RBT Rules

1. **BST Property** still holds

Same relationship between balance, height, size and complexity

We can keep the implementations of `lookup` and `forEach`

---

## RBT Rules

<ol start="2">
<li>The root is always black</li>
<li>Leaves and the root's parent count as black</li>
</ol>

This is bookkeeping to simplify the implementation

We'll use a **sentinel** node to represent empty nodes

---

## RBT Rules

<ol start="4">
<li>You can't have 2 red nodes in a row
<ul>
<li>2 black nodes is OK</li>
<li>In other words, if a node is red, both its children must be black</li>
</ul>
</li>
<li>Number of black nodes is the same for every path from a leaf to the root
<ul><li>We'll call this the **black-depth** of a node</li></ul>
</li>
</ol>

These are the key to keeping our tree balanced

---

## Example

![](binary-trees/images/rbt-black-depth.png)

Is this a red-black tree?

---

@snap[east span-40]
![](binary-trees/images/rbt-black-depth.png)
@snapend

@snap[northwest span-55]

## Balance

<p class="small">Because you can't have 2 red nodes in a row, the depth of a leaf is never more than twice its black-depth</p>

<p class="small fragment">The black-depth is the same for all leaves, so no leaf is more than twice as deep as any other</p>

<p class="small fragment">This means the tree is balanced, according to our definition</p>

<p class="small fragment">So the height is `\(O(log(n))\)`</p>
@snapend

---

## Rules Review

<ol class="small">
<li>**BST Property** still holds</li>
<li>The root is always black</li>
<li>Leaves, root's parent count as black</li>
<li>You can't have 2 red nodes in a row</li>
<li>Black-depth is the same for every leaf</li>
</ol>

Any tree that obeys these rules is a balanced BST

If we can find a way to make our trees follow these rules, we're set

---

@snap[midpoint span-100]

# Red-Black Tree Rebalance

@snapend

---

## Learning Goals

By the end of this module, students will be able to...

* **Define** the term induction
* **Describe** the algorithm to rebalance a red-black tree after inserting a node
* **Explain** why our rebalance algorithm will only ever encounter one rules violation

---

## Insert Rebalance

1. Do a **normal BST insert**, coloring the new node **red**
1. **Rebalance the tree** so that it follows the rules

We can assume that the **rules held before we started**

<p class="small fragment">We "clean as we go", never leaving the tree in a bad state</p>

<p class="fragment">This kind of reasoning is called **induction**</p>

<p class="fragment">Do the rules hold for a **tree of size one**?</p>

---

## Rebalance Induction

![](binary-trees/images/rbt-induction.png)

---

## Problem

We just inserted a **red** node

**Question:** Which of our RBT rules might be violated?

<ul class="small fragment">
<li>It's still a BST, and we haven't touched the root or leaves</li>
<li>We haven't added a black node, so it won't affect the black-depth</li>
</ul>

<p class="fragment">We may have violated rule 4 (2 red nodes in a row)</p>

---

## Procedure

Starting with the node we just inserted, work our way up the path to the root, making adjustments as we go

After each step we'll end up with one of two situations:

<ul class="small">
<li>The RBT now follows the rules</li>
<li>We still have 2 red nodes stacked, just further up the tree</li>
</ul>

We're always fixing a violation of rule 4

<!--
pesudocode:
while loop condition (node.parent is red)
iteration (set some ancestor of node to red, set it as our new node, continue)
-->

---

@snap[northwest span-60 text-left]

## Nodes

Each iteration, we look at 4 nodes:

<ul class="small">
<li class="fragment">`node`, `N`
<ul><li>We just set `node` to be red</li></ul></li>

<li class="fragment">`parent`, `P`
<ul>
<li>Was red before we started (rule 4)</li>
<li>Can't be the root or a sentinel (because it's red)</li>
</ul></li>

<li class="fragment">`grandparent`, `G`
<ul><li>Can't be a sentinel, must be black (why?)</li></ul></li>

<li class="fragment">`uncle`, `U`
<ul><li>May be a sentinel, may be either color</li></ul></li>

<li class="fragment">`a`, `b`, `c`, `d` are other nodes we know exist
<ul><li>`a`, `b` and `c` may be sentinels, must be black</li></ul></li>
</ul>
@snapend

@snap[east span-40]
![](binary-trees/images/rbt-node-names.png)
@snapend

---

@snap[northwest span-50]

## 2x2 Cases

Is `parent` the left or right child of `grandparent`?

<ul class="small">
<li>How can you tell?</li>
<li>These cases are symmetric, so we'll only show the left child case</li>
</ul>

Is `uncle` red or black?

<ul class="small">
<li>These cases are very different</li>
<li>How would you find `uncle`?</li>
</ul>

<p class="small">We don't care (much) whether `node` is a left or right child</p>

@snapend

@snap[east span-50]
![](binary-trees/images/rbt-cases.png)
@snapend

<!--
pesudocode:
grab parent and grandparent
check handed-ness of parent
get uncle
check color of uncle
 -->

---

@snap[northwest span-60]

## Red Uncle

`parent` and `uncle` are red, `grandparent` is black

Swap colors between the generations

We just colored a node red!

<p class="small">This might break rule 4 again</p>

<p class="small">Any other broken rules?</p>

@snapend

@snap[east span-40]
![](binary-trees/images/rbt-red-uncle.png)
@snapend

<!--
pseudocode
swap colors
update node
-->

---

@snap[northwest span-60]

## Resolution

<div>
<p>If `d` is black we're done!</p>
<p class="small">We fixed the only rules violation, and haven't introduced any new ones</p>
</div>

<div class="fragment">
<p>If `d` is red, we've moved the problem up the tree</p>
<p class="small">Repeat with `grandparent` as `node`</p>
</div>

<p class="small fragment">**Question:** If `grandparent` or `d` is the root, which resolution would we get?</p>

<p class="small fragment">`d` is either the root or a sentinel, so it must be black</p>
@snapend

@snap[east span-35]
![](binary-trees/images/rbt-red-uncle-resolution.png)
@snapend

---

## Final Cleanup

Color the root black

<!--
pseudocode - color root black after the loop
-->

---

@snap[northwest span-65]

## Black Uncle

If `uncle` is black, we can use a rotation to create some wiggle room

How we proceed depends on whether `node` is a left child or right child of `parent`
@snapend

@snap[east span-30]
![](binary-trees/images/rbt-black-uncle-cases.png)
@snapend

---

## Node is Left Child

<p class="small">Swap `parent` and `grandparent` colors, rotate `grandparent` right</p>

![](binary-trees/images/rbt-black-uncle-left.png)

Does this fix all our rules violations? <span class="fragment">**Yes!**</span>

<!--
pseudocode - left child
-->

---

## Node is Right Child

<p class="small">We can convert a right child to a left child via rotation</p>

![](binary-trees/images/rbt-black-uncle-right.png)

<p class="small">Double check: does this introduce any new rules violations?</p>

<!--
pseudocode - right child + condition
-->

---

## Rebalance Analysis

How long does each iteration take?

<p class="small fragment">Constant time</p>

How many iterations, in the worst case?

<p class="small fragment">`\(O(h)\)`</p>

<p class="fragment">Since `insert` was already `\(O(h)\)`, the complexity is the same</p>

---

## Summary

Any tree that follows the RBT rules is a balanced BST

<ul class="small">
<li>Every node is black or red</li>
<li>You can't have 2 red nodes in a row</li>
<li>The black-depth of every leaf is the same</li>
</ul>

To keep our BST balanced, we rebalance it after every insert so that it follows the rules again

<p class="small">We can use induction to reason about such a process</p>

Since rebalancing runs in `\(O(h)\)` time, adding it to `insert` doesn't affect the time complexity

---

## Vocab

| Term           | Definition                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------- |
| Red-black tree | A BST that rebalances itself to follow a set of rules after each `insert` to keep itself balanced |
| Rebalance      | Adjusting the tree so that it follows the rules                                                   |
| Black-depth    | The number of black nodes between a node and the root                                             |
| Rotation       | Swapping a node and its parent, while maintaining BST ordering for all children                   |
| Induction      | A technique for reasoning about iterative processes                                               |
