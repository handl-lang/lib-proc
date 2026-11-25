# lib-proc

Process identity **contract** for Handl — `argv`, `exit`, `pid`.

## Layout

| path | role |
| --- | --- |
| `src/index.hnl` | the contract |
| `src/index.test.hnl` | the .hnl test suite |
| `atoms.yaml` | atom declarations |
| `pkg.hnl`, `ci.sh`, `scripts/hnl-gate.sh` | manifest / bupha entry / gate |

## Contract

- Port/impl split (P-012): contract only — the ops are host atoms
  implemented in `host/crates/plugin-lib-stdlib/src/ops/process.rs`
  (`std::env::args` / `std::process::exit` / `std::process::id`),
  registered as the `proc` module in the stdlib package table.
- `lib-posix` stays strict POSIX.1 — argv/exit/pid are launch/runtime
  data, not POSIX functions, so they live here.
- API: `proc.argv() ~> [String]`, `proc.exit(n) ~> a` (never
  returns), `proc.pid() ~> Int`. Primary consumer:
  `lib-cli-router`'s `run`.

## Verification

```bash
./ci.sh   # bupha entry → scripts/hnl-gate.sh
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
