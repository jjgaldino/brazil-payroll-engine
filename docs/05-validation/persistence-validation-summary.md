# Persistence Validation — Public Summary

Beyond validating the calculation, Ordo validated the **persistence** of the
validation chain: that what was calculated is exactly what persists, reloads,
replays, and survives a process restart — in an isolated staging environment.

## Results

```
ShadowRuns persisted ................. 24 / 24
Snapshots persisted .................. 24 / 24
Database reload match ................ 24 / 24
Baseline hash match .................. 24 / 24  (snapshot / facts / oracle / output / previous)
Hash-chain integrity ................. PASS  (GENESIS → final; previous[i] == snapshot[i-1])
Replay from database ................. PASS
Process-restart replay ............... PASS  (new process, reading only from the database)
Duplicate application ................ 0     (idempotent)
Duplicate run ........................ 0     (unique per run)
Previous-snapshot mutation ........... 0
Transactional partial state .......... 0     (a controlled failure rolls back cleanly)
Unresolved ........................... 0
```

## Process

Each competence followed the same loop: load the certified baseline, persist the
facts, oracle envelope, run and snapshot as a hash-chain node **inside a
transaction**, commit, **reload from the database**, replay the persisted state, and
compare against the in-process baseline. Critically, the final comparison always
happened **after a fresh read from the database** — never from in-memory objects.

## What was proven

- **Fidelity** — the persisted chain matches the in-process baseline on every hashed
  field (snapshot, facts, oracle, output, and the previous-hash link) for all 24
  nodes.
- **Integrity** — the hash-chain is intact from the genesis node to the last: each
  node's previous-hash equals the prior node's snapshot-hash.
- **Replay from storage** — recomputing each competence from the persisted
  previous-hash reproduces the stored snapshot-hash.
- **Restart survival** — after terminating the process, the chain was reconstructed
  and replayed in a **new process reading only from the database**, and the
  checkpoints matched.
- **Idempotency** — re-persisting the same competence does not create a second
  application; the existing run is recognized.
- **Transactional safety** — a controlled failure between calculation and persistence
  rolls back with no partial state.

## Honest framing

- This was validated **exclusively in an isolated staging environment**, on a
  namespaced, synthetic dataset. Production was not touched, and no external
  obligation was transmitted as part of this validation.
- Together with the [Master Shadow summary](master-shadow-summary.md), it means the
  engine's correctness and the durability of its record were both established — the
  calculation and its persistence.

## Conclusion

Ordo Payroll Core's validation is not only "the numbers are right" but also "the
record of the right numbers persists, reloads, replays, and survives restart, with
idempotency and transactional safety". Within the tested, isolated scope, the
operation is validated end to end.
