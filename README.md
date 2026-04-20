# 🛠️ Compiler Design Lab — Complete Repository

> Implementation of core Compiler Design concepts in Python, covering all phases from Lexical Analysis to Code Generation and Optimization.

---

## 📁 Repository Structure

```
COMPILER-DESIGN-LAB-/
├── Lab 1 Lexical analyzer/
│   ├── lexical.py
│   └── Readme.md
├── Lab 2 conversion from Regular Expression to NFA/
│   ├── Re_to_nfa.py
│   └── Readme.md
├── Lab 3 Conversion from NFA to DFA/
│   ├── Nfa_dfa.py
│   └── Readme.md
├── Lab 4 Elimation of Ambiguity, Left Recursion and Left Factoring/
│   ├── Lftrecursion.py
│   └── Readme.md
├── Lab 5 -FIRST AND FOLLOW computation/
│   ├── First-follow-func.py
│   └── Readme.md
├── Lab 6 Predictive Parsing Table/
│   ├── Parsetable.py
│   └── Readme.md
├── Lab 7 - Shift Reduce Parsing/
│   ├── Shiftreduce.py
│   └── Readme.md
├── Lab 8- Computation of LEADING AND TRAILING/
│   ├── lead&trailing.py
│   └── Readme.md
├── Lab 9 - Computation of LR(0) Items/
│   ├── lr0items.py
│   └── Readme.md
├── Lab 10 - Intermediate Code Generation Postfix Prefix/
│   ├── postfix_prefix.py
│   └── Readme.md
├── Lab 11 - Intermediate Code Generation Quadruple Triple/
│   ├── quad_triple.py
│   └── Readme.md
├── Lab 12 - Simple Code Generator/
│   ├── codegen.py
│   └── Readme.md
├── Lab 13 - Implementation of DAG/
│   ├── dag.py
│   └── Readme.md
├── Lab 14 - Global Data Flow Analysis/
│   ├── dataflow.py
│   └── Readme.md
├── Lab 15 - Storage Allocation Strategies/
│   ├── storage_allocation.py
│   └── Readme.md
└── README.md  ← You are here
```

---

## 🗂️ Lab Summary

### Lab 1 — Lexical Analyzer
**File:** `lexical.py`

The first phase of a compiler. Reads raw source code and breaks it into a stream of **tokens** — the smallest meaningful units of a program.

- Recognizes: Keywords, Identifiers, Integers, Floats, Operators, Delimiters, Strings
- Built using Python's `re` (regex) module with a master pattern
- Skips whitespace, flags unknown tokens

```
Input:  int x = 10 + y;
Output: KEYWORD(int)  IDENTIFIER(x)  OPERATOR(=)  INTEGER(10)  OPERATOR(+)  IDENTIFIER(y)  DELIMITER(;)
```

---

### Lab 2 — Regular Expression to NFA
**File:** `Re_to_nfa.py`

Converts a Regular Expression into a **Non-deterministic Finite Automaton (NFA)** using **Thompson's Construction Algorithm**.

- Supports: literals, concatenation, union `|`, Kleene star `*`, `+`, `?`, parentheses
- Builds NFA incrementally by combining smaller NFAs
- Prints all states and ε-transitions

```
Regex: (a|b)*abb
→ NFA with start state q0, accept state q7, using ε-transitions
```

---

### Lab 3 — NFA to DFA
**File:** `Nfa_dfa.py`

Converts an NFA to an equivalent **Deterministic Finite Automaton (DFA)** using the **Subset Construction (Powerset) Algorithm**.

- Computes ε-closure and move operations
- Each DFA state = a set of NFA states
- Simulates the resulting DFA on input strings (accept/reject)
- Prints full transition table for both NFA and DFA

```
NFA state {q0, q1, q3} on input 'a' → DFA state {q1, q2}
Input 'abb' → ACCEPTED ✓
```

---

### Lab 4 — Elimination of Ambiguity, Left Recursion and Left Factoring
**File:** `Lftrecursion.py`

Prepares a Context-Free Grammar (CFG) for parsing by applying three transformations:

**Part 1 — Ambiguity Elimination**
- Explains ambiguity with the Dangling Else problem and expression grammars
- Shows how to restructure grammars to remove multiple parse trees

**Part 2 — Left Recursion Elimination**
- Detects direct left recursion (`A -> Aα`)
- Transforms using the standard `A -> βA'`, `A' -> αA' | ε` rule

**Part 3 — Left Factoring**
- Detects common prefixes in productions
- Factors them out into new non-terminals so parsers can decide with one lookahead

```
E -> E + T | T        →     E  -> T E'
                             E' -> + T E' | ε
```

---

### Lab 5 — FIRST and FOLLOW Sets
**File:** `First-follow-func.py`

Computes **FIRST** and **FOLLOW** sets for all non-terminals — the foundation for building LL(1) parsing tables.

- **FIRST(A):** terminals that can start a derivation from A (handles ε-propagation)
- **FOLLOW(A):** terminals that can appear immediately after A in any sentential form
- Includes an **interactive mode** — enter any grammar and get results instantly

```
Non-Terminal    FIRST                   FOLLOW
E               { (, id }               { $, ) }
E'              { +, ε }                { $, ) }
T               { (, id }               { $, ), + }
```

---

### Lab 6 — Predictive Parsing Table (LL(1))
**File:** `Parsetable.py`

Constructs the **LL(1) Predictive Parsing Table** and simulates top-down parsing.

- Builds FIRST and FOLLOW internally
- Fills table using standard LL(1) rules
- Detects and reports **conflicts** (grammar not LL(1))
- Simulates **stack-based LL(1) parsing** with step-by-step trace

```
STACK                       INPUT                ACTION
$ E' T E                    id + id * id $       E -> T E'
$ E' T' F T                 id + id * id $       T -> F T'
$ E' T' id F                id + id * id $       F -> id
...                                              ACCEPT ✓
```

---

### Lab 7 — Shift-Reduce Parsing (SLR(1))
**File:** `Shiftreduce.py`

Implements a full **SLR(1) bottom-up parser** — builds the LR(0) automaton, constructs the parsing table, and simulates parsing.

- Augments grammar with `S' -> S`
- Builds **LR(0) item sets** using closure and goto operations
- Fills **ACTION** (shift/reduce/accept) and **GOTO** tables using FOLLOW sets
- Simulates shift-reduce parsing with full stack trace
- Detects shift-reduce and reduce-reduce conflicts

```
STACK                   INPUT              ACTION
$ 0                     id + id $          Shift id → state 5
$ 0 id 5                + id $             Reduce by F -> id
$ 0 F 3                 + id $             Reduce by T -> F
...                                        ACCEPT ✓
```

---

### Lab 8 — LEADING and TRAILING Sets
**File:** `lead&trailing.py`

Computes **LEADING** and **TRAILING** sets for Operator Grammars, and derives **Operator Precedence Relations**.

- **LEADING(A):** terminals that can appear as the leftmost terminal in derivations of A
- **TRAILING(A):** terminals that can appear as the rightmost terminal in derivations of A
- Derives the three precedence relations between operators:

| Relation | Symbol | Meaning |
|---|---|---|
| Yields precedence | `a ⋖ b` | b has higher precedence |
| Equal precedence  | `a ≐ b` | same precedence level |
| Takes precedence  | `a ⋗ b` | a has higher precedence |

```
Non-Terminal    LEADING              TRAILING
E               { (, +, *, id }      { ), +, *, id }
T               { (, *, id }         { ), *, id }
F               { (, id }            { ), id }
```

---

### Lab 9 — Computation of LR(0) Items
**File:** `lr0items.py`

Computes the **canonical collection of LR(0) item sets** — the foundation for all LR parsers.

- Augments the grammar with `S' -> S`
- Builds all item sets using **closure** and **GOTO** operations
- An LR(0) item tracks parsing progress with a dot `•` in productions
- Prints every state and its transitions

```
I0:
  E' -> • E
  E  -> • E + T
  E  -> • T
  F  -> • id
  GOTO(id) = I5,  GOTO(E) = I1 ...
```

---

### Lab 10 — Intermediate Code: Postfix & Prefix
**File:** `postfix_prefix.py`

Converts infix expressions to **Postfix** (Reverse Polish Notation) and **Prefix** (Polish Notation) as intermediate representations.

- Uses **Shunting-Yard algorithm** for postfix; reversed approach for prefix
- Handles operator precedence and associativity (`^` right-associative)
- Also evaluates numeric postfix expressions

```
Infix              Postfix          Prefix
a + b * c          a b c * +        + a * b c
(a+b) * (c-d)      a b + c d - *    * + a b - c d
3 + 4 * 2  =  11
```

---

### Lab 11 — Intermediate Code: Quadruple, Triple, Indirect Triple
**File:** `quad_triple.py`

Generates three standard forms of intermediate code from expressions.

- **Quadruples** `(op, arg1, arg2, result)` — each instruction is self-contained
- **Triples** `(op, arg1, arg2)` — result referenced by position index `(i)`
- **Indirect Triples** — pointer table into the triples list, supports reordering

```
Expression: a + b * c

Quadruples:          Triples:          Indirect Triples:
(*, b, c, t1)        (0): *, b, c      ptr 0 → (0)
(+, a, t1, t2)       (1): +, a, (0)    ptr 1 → (1)
```

---

### Lab 12 — Simple Code Generator
**File:** `codegen.py`

Translates **Three-Address Code (TAC)** into assembly-like target code using register and address descriptors.

- Maintains a **Register Descriptor** (which variable is in each register)
- Maintains an **Address Descriptor** (where each variable lives)
- Issues `LD`, `ST`, `ADD`, `SUB`, `MUL`, `DIV` instructions
- Spills registers to memory when all are occupied

```
TAC: t1 = b * c,  t2 = a + t1

Target Code:
  LD  R0, b
  LD  R1, c
  MUL R2, R0, R1   ; t1 = b * c
  LD  R0, a
  ADD R3, R0, R2   ; t2 = a + t1
```

---

### Lab 13 — Implementation of DAG
**File:** `dag.py`

Builds a **Directed Acyclic Graph (DAG)** for a basic block to detect and eliminate **common subexpressions**.

- Leaf nodes = variables/constants; interior nodes = operators
- Identical computations share the **same node** — computed only once
- Prints DAG node table showing which variables point to each node

```
t1 = a + b
t2 = a + b   ← same as t1, reuses node!
t3 = t1 * t2

DAG: only 4 nodes instead of 5 — one + node shared by t1 and t2
```

---

### Lab 14 — Global Data Flow Analysis
**File:** `dataflow.py`

Implements **Reaching Definitions** analysis — a classical iterative global data flow algorithm.

- Computes **GEN** (definitions generated) and **KILL** (definitions killed) per block
- Iteratively computes **IN** and **OUT** sets until convergence
- Works on any control flow graph (supports loops)

```
Block   GEN        KILL       IN              OUT
B1      {d1,d2}    {d1,d2}    {}              {d1,d2}
B2      {d3,d4}    {d3,d4}    {d1,d2,d3,d4}  {d1,d3,d4}
B3      {d5}       {d5}       {d1,d3,d4}     {d1,d3,d4,d5}
```

---

### Lab 15 — Storage Allocation Strategies
**File:** `storage_allocation.py`

Implements and demonstrates all three storage allocation strategies used in compilers — in a single file.

- **Static Allocation** — fixed addresses assigned at compile time; no overhead, no recursion
- **Stack Allocation** — activation records pushed/popped on call/return; supports recursion
- **Heap Allocation** — dynamic `malloc`/`free` with first-fit free list and block coalescing

| Feature | Static | Stack | Heap |
|---|---|---|---|
| Allocation time | Compile time | Runtime (call) | Runtime (explicit) |
| Supports recursion | ✗ | ✓ | ✓ |
| Fragmentation | None | None | Possible |

```
Heap: malloc(20) → block #1 at addr 0
      malloc(15) → block #2 at addr 20
      free(#2)   → coalesced back into free list
      malloc(10) → reuses freed space
```

---

## ⚙️ How to Run Any Lab

No external dependencies — pure Python 3.

```bash
# Clone the repo
git clone <your-repo-url>
cd COMPILER-DESIGN-LAB-

# Run any lab directly
python "Lab 1 Lexical analyzer/lexical.py"
python "Lab 2 conversion from Regular Expression to NFA/Re_to_nfa.py"
python "Lab 3 Conversion from NFA to DFA/Nfa_dfa.py"
python "Lab 4 Elimation of Ambiguity, Left Recursion and Left Factoring/Lftrecursion.py"
python "Lab 5 -FIRST AND FOLLOW computation/First-follow-func.py"
python "Lab 6 Predictive Parsing Table/Parsetable.py"
python "Lab 7 - Shift Reduce Parsing/Shiftreduce.py"
python "Lab 8- Computation of LEADING AND TRAILING/lead&trailing.py"
python "Lab 9 - Computation of LR(0) Items/lr0items.py"
python "Lab 10 - Intermediate Code Generation Postfix Prefix/postfix_prefix.py"
python "Lab 11 - Intermediate Code Generation Quadruple Triple/quad_triple.py"
python "Lab 12 - Simple Code Generator/codegen.py"
python "Lab 13 - Implementation of DAG/dag.py"
python "Lab 14 - Global Data Flow Analysis/dataflow.py"
python "Lab 15 - Storage Allocation Strategies/storage_allocation.py"
```

**Requirements:** Python 3.6+, no pip installs needed.

---

## 🔗 Concept Flow

```
Source Code
    ↓
[Lab 1]  Lexical Analyzer          →  Token Stream
    ↓
[Lab 2]  RE → NFA                  →  NFA (Thompson's Construction)
    ↓
[Lab 3]  NFA → DFA                 →  DFA (Subset Construction)
    ↓
[Lab 4]  Grammar Transformations   →  Clean CFG (no ambiguity / left recursion)
    ↓
[Lab 5]  FIRST & FOLLOW            →  Sets needed for parsing tables
    ↓
[Lab 6]  Predictive Parsing        →  LL(1) Table + Top-down Parser
    ↓
[Lab 7]  Shift-Reduce Parsing      →  SLR(1) Table + Bottom-up Parser
    ↓
[Lab 8]  LEADING & TRAILING        →  Operator Precedence Relations
    ↓
[Lab 9]  LR(0) Items               →  Canonical Item Sets for LR Parsing
    ↓
[Lab 10] Postfix / Prefix          →  Intermediate Representation
    ↓
[Lab 11] Quadruple / Triple        →  Three-Address Code Forms
    ↓
[Lab 12] Code Generator            →  Assembly-like Target Code
    ↓
[Lab 13] DAG                       →  Common Subexpression Elimination
    ↓
[Lab 14] Data Flow Analysis        →  Reaching Definitions (Global Optimisation)
    ↓
[Lab 15] Storage Allocation        →  Static / Stack / Heap Memory Management
```

---

## 📘 Subject
**Compiler Design** — Core CS undergraduate course covering the theory and implementation of compilers and language processors.
