# Pydantic

## A model-level `before` or `wrap` validator turns strict JSON input into Python input
- Symptom: on a strict model, `model_validate_json` rejects a date string (`Input should be a valid date`) or a coordinate array (`Input should be a valid array`) that loaded fine before you added a `@model_validator(mode="before")` or `mode="wrap"`. It happens even when the validator passes its input through untouched.
- Rule: on a strict model, don't put a `before` or `wrap` validator on the model. Check across fields in a `mode="after"` validator instead. To accept and check an input key that a computed field also outputs, declare an input-only field with `alias="<name>", exclude=True`, compare it in the `after` validator, and use `model_fields_set` to tell an explicit null from a missing key. A field-level `BeforeValidator` on a `str` field is harmless, because a string is the same in both modes.
- Why: the validator receives the parsed JSON as a plain dict, and whatever it hands on is validated as Python input, where strict mode accepts only real `date`, `datetime`, enum and `tuple` objects.
- Seen: private project, 2026-10 · Pydantic 2.13.5 · verified (cost an hour, and a probe on a `list[int]` model didn't show it)
- Recheck: Pydantic 3
- Source: none yet

## `Annotated[float, X] | None` hides `X` from the field's metadata
- Symptom: code that reads a field's metadata, such as a unit or a tag, finds it on `x: Annotated[float, X]` but gets `[]` from `y: Annotated[float, X] | None`. Validation and serialization work, so nothing else fails.
- Rule: put the union inside: `Annotated[float | None, X]`. Test that every field you expect to carry metadata has it in `Model.model_fields[name].metadata`.
- Why: Pydantic lifts `Annotated` metadata into `FieldInfo` only from the outermost type. Inside a union, it stays buried in the annotation.
- Seen: private project, 2026-10 · Pydantic 2.13.5 · verified
- Recheck: Pydantic 3
- Source: none yet

## Python's `re` and a Pydantic `pattern` disagree about `$`
- Symptom: a string with a trailing newline (`"name@1\n"`) fails a field's `pattern`, but passes the same regex in Python with `re.match` or `re.search`. Or the reverse, once a pattern moves from a `Field` to plain Python.
- Rule: when a pattern is shared between Pydantic and Python code, match it with `re.fullmatch` in Python, or end it with `\Z` there. Keep `^…$` anchors in the Pydantic `pattern`, which otherwise searches anywhere in the string.
- Why: in Python's `re`, `$` also matches just before a trailing newline. Pydantic's default regex engine is Rust's `regex` crate, where `$` matches only at the very end.
- Seen: private project, 2026-10 · Pydantic 2.13.5, Python 3.14.4 · verified
- Recheck: stable (it's each engine's design)
- Source: https://docs.python.org/3/library/re.html (regular expression syntax, `$`); https://docs.rs/regex/latest/regex/#syntax

## NumPy scalars slip into models, and fail late or change value
- Symptom: a strict `float` field turns `np.float32(5.04)` into `5.039999961853027` without complaint. A `dict[str, Any]` field accepts NumPy scalars, and then `model_dump_json` raises `PydanticSerializationError` in the cache write or the HTTP response, far from the code that put them there.
- Rule: hand models Python numbers: `float(x)` and `int(x)` at the boundary where NumPy work ends. Type free-form JSON fields as `dict[str, JsonValue]`, not `dict[str, Any]`, so a NumPy scalar or a `date` is rejected when the model is built.
- Why: `np.float32` converts to a Python float exactly, exposing its binary value. `Any` skips validation, so nothing checks the value until it's serialized.
- Seen: private project, 2026-10 · Pydantic 2.13.5, NumPy 2.5.3 · verified
- Recheck: Pydantic 3
- Source: none yet

## `JsonValue` takes `NaN` and `Infinity` from JSON, then writes them out as `null`
- Symptom: a JSON field typed `JsonValue` rejects `float("nan")` built in Python, but accepts a `NaN` or `Infinity` token in text given to `model_validate_json`. `model_dump_json` then writes that value as `null`, so a number silently becomes absent.
- Rule: if non-finite numbers must never reach a `JsonValue` field, add a field-level `after` validator that walks the value and rejects any non-finite float. Test it with JSON text, not only with Python input.
- Why: Pydantic's JSON parser reads the `NaN` and `Infinity` tokens, and the model's `allow_inf_nan=False` isn't applied inside `JsonValue` for JSON input. The default serializer writes non-finite floats as `null`.
- Seen: private project, 2026-10 · Pydantic 2.13.5 · verified
- Recheck: Pydantic 3
- Source: none yet
