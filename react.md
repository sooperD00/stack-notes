# React

## Don't use `React.FormEvent` for submit handlers
- Symptom: a deprecation warning on the handler's type
- Rule: type submit handlers with the newer handler types (`SubmitEventHandler`), not `React.FormEvent`. The Actions pattern (`FormData`, `useFormStatus`) is the longer-term answer; take it only when the form needs it.
- Seen: Nicole's notes · React 19 · unverified: check against the project's @types/react version
