# weave-README

## Weave is a concurrency intelligence engine for Java. 

### Status
In active development.
Full source is private during development.


---

## What Weave Does

Most tools detect race conditions. Weave detects them, verifies them at runtime, and explains them in terms a developer can actually act on.

Weave has three goals:

**Predict** — Using static analysis and Java Memory Model happens-before reasoning, Weave reads your source code and identifies every place where a race condition, deadlock, starvation, or lock gap could occur. No execution required.

**Verify** — Using the Lockset algorithm and vector clocks at runtime, Weave observes what actually happened during execution and confirms whether locking discipline was maintained.

**Explain** — Every finding is explained in two modes. Beginner mode uses plain language and metaphors. Expert mode uses precise JMM terminology, happens-before reasoning, and actionable technical detail.

---

## The Mathematics

### Happens-Before

Weave implements the Java Memory Model happens-before relation formally. Two accesses form a race condition if and only if:

- They are in different threads
- At least one is a write
- There is no happens-before relationship between them in either direction

The rules Weave checks statically:

- **Rule 2** — An unlock happens-before every subsequent lock of the same monitor
- **Rule 3** — A volatile write happens-before every subsequent read of that variable
- **Rule 4** — `Thread.start()` happens-before any action in the started thread
- **Rule 5** — All actions in a thread happen-before `Thread.join()` returns

### Vector Clocks

Weave tracks happens-before relationships using vector clocks. Each thread maintains a scoreboard of how much work every other thread has done. When threads synchronize they merge scoreboards by taking the maximum at every slot. A write records a snapshot of the clock at that moment. A read checks whether the write snapshot fits inside the reader's current clock. If it does not fit — race condition.

### Lockset Algorithm

The dynamic engine tracks for every shared variable the intersection of locks held across every access. If that intersection becomes empty — no lock has consistently protected the variable — that is a Lockset violation and a confirmed race.

---
## Stack

Java — JavaParser — JUnit 5 — Maven

---
