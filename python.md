# Python

## `StrEnum` members sort alphabetically, not in definition order
- Symptom: `sorted(statuses)`, `max(statuses)` or `a < b` on `StrEnum` members gives an order that ignores how the enum is defined. `max` over `PASS`, `FLAG`, `FAIL` returns `pass`.
- Rule: iterate the enum (`list(MyEnum)`) for definition order, and rank with an explicit tuple, such as `RANKED = (FAIL, FLAG, PASS)`. Never rank with `sorted()`, `min()`, `max()` or `<`. Say so in the enum's docstring.
- Why: a `StrEnum` member is a `str`, so comparisons are string comparisons of its value.
- Seen: private project, 2026-10 · Python 3.14.4 · verified
- Recheck: stable
- Source: https://docs.python.org/3/library/enum.html#enum.StrEnum

## `\d` matches any Unicode digit
- Symptom: a pattern meant for ASCII digits, such as a 5-digit code, accepts Arabic-Indic `٣٣` or Devanagari digits.
- Rule: write `[0-9]` when you mean ASCII digits. This holds in Python's `re` and in Rust's `regex`, which Pydantic uses for `pattern`.
- Why: for `str` patterns, both engines define `\d` as any Unicode decimal digit.
- Seen: private project, 2026-10 · Python 3.14.4, Pydantic 2.13.5 · verified
- Recheck: stable
- Source: https://docs.python.org/3/library/re.html (`\d`); https://docs.rs/regex/latest/regex/#syntax
