# NumPy

## NumPy 2's scalar `repr` isn't a number, and `float32` isn't a `float`
- Symptom: `Decimal(repr(x))` raises `InvalidOperation` on a NumPy float, because `repr(np.float64(0.15))` is `np.float64(0.15)`. An `isinstance(x, float)` check rejects `np.float32`, and `isinstance(x, int)` rejects `np.int64`, so a formatter that worked on `np.float64` raises `TypeError` once the array is `float32`.
- Rule: convert at the boundary where NumPy work ends: `float(x)` and `int(x)` before formatting, serializing or building a model. Cast arrays you reduce to `float64` first when the result has to be exact enough to print.
- Why: NumPy 2 changed scalar `repr` to name the type (NEP 51). `np.float64` subclasses `float`; `np.float32` and the integer types don't subclass `float` or `int`.
- Seen: private project, 2026-10 · NumPy 2.5.3 · verified
- Recheck: NumPy 3
- Source: https://numpy.org/neps/nep-0051-scalar-representation.html
