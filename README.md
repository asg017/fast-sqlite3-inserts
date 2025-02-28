# Fast SQLite Inserts

To find out the fastest way to create an SQLite DB with one billion random rows.

Read this blog post for the more context - [Towards Inserting One Billion Rows in SQLite Under A Minute](https://avi.im/blag/2021/fast-sqlite-inserts/)

## Leaderboard

(for 100M insertions)

Variant       | Time
------------- | -------------
Rust  | 30 seconds
PyPy  | 126 seconds
CPython  | 210 seconds

## Current Benchmark

### Python

These are the current fastest CPython and PyPy numbers.

```shell
$ ./bench.sh

Sat May  8 19:42:44 IST 2021 [PYTHON] running sqlite3_opt_batched.py (100_000_000) inserts
      517.53 real       508.24 user         7.35 sys

Sat May  8 20:03:04 IST 2021 [PYPY] running sqlite3_opt_batched.py (100_000_000) inserts
      159.70 real       153.46 user         5.81 sys
```

### Rust

These are the current fastest Rust numbers

```
Sun Aug  8 13:52:32 IST 2021 [RUST] basic_batched.rs (100_000_000) inserts

real	0m32.424s
user	0m30.826s
sys	0m2.272s

Sun Aug  8 13:53:06 IST 2021 [RUST] threaded_batched.rs (100_000_000) inserts

real	0m30.094s
user	0m42.704s
sys	0m3.877s
```

### In Memory

Instead of writing to disk, I used a `:memory:` DB, these are the numbers

```
Mon May 10 17:40:39 IST 2021 [RUST] basic_batched.rs (100_000_000) inserts
       31.38 real        30.55 user         0.56 sys

Mon May 10 17:39:39 IST 2021 [RUST] threaded_batched.rs (100_000_000) inserts
       28.94 real        45.02 user         2.03 sys
```

### Busy loop time

The amount of time these scripts were taking in just to run the for loops (and no SQL insertion)

```
$ ./busy.sh

Sun May  9 13:16:01 IST 2021 [PYTHON] busy_loop.py (100_000_000) iterations
      351.14 real       347.53 user         3.39 sys

Sun May  9 13:21:52 IST 2021 [PYPY] busy_loop.py (100_000_000) iterations
       81.58 real        77.73 user         3.80 sys

Sun May  9 13:23:14 IST 2021 [RUST] busy.rs (100_000_000) iterations
       17.97 real        16.29 user         1.67 sys

Sun May  9 13:23:32 IST 2021 [RUST] threaded_busy.rs (100_000_000) iterations
        7.18 real        42.52 user         7.20 sys
```

## Community Contributions

| PR  | Author  | Result  |
|---|---|---|
| [#2](https://github.com/avinassh/fast-sqlite3-inserts/pull/2)  | [captn3m0](https://github.com/captn3m0)  | Reduced the CPython running time by half (from 7.5 minutes to 3.5 minute)  |
| [#12](https://github.com/avinassh/fast-sqlite3-inserts/pull/12)  | [red15](https://github.com/red15)  | saved 2s from Rust's running time (bringing it to 30s)  |


## Contributing

All contributions are welcome. If you have any ideas on increasing the performance, feel free to submit a PR. You may also check the current open issues to work on.

## License

Released under MIT License. Check `LICENSE` file more info.
