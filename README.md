# lib-proc

Process identity contract for Handl — `argv`, `exit`, `pid`.

Port/impl split (P-012): this repo is the **contract** only. The ops are
host atoms implemented in `host/crates/plugin-lib-stdlib/src/ops/process.rs`
(`std::env::args` / `std::process::exit` / `std::process::id`) and registered
as the `proc` module in the stdlib package table.

`lib-posix` stays strict POSIX.1 — argv/exit/pid are launch/runtime data,
not POSIX functions, so they live here instead.

## API

```handl
from "proc" import * as proc

proc.argv()   -- ~> [String]  own-process argv, argv[0] included
proc.exit(n)  -- ~> a         terminate with exit code n (never returns)
proc.pid()    -- ~> Int       own process id
```

Primary consumer: `lib-cli-router`'s `run` — `proc.argv()` supplies
own-process argv and `proc.exit(code)` replaces the `opts.exit` hook.
