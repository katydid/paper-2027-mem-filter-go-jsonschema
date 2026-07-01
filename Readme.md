# Memoized Derivatives for Fast Filtering and Schema Validation of Semi-Structured Data (JSON Schema Go)

JSON Schema in Go using Katydid underlying algorithm.
This project translates JSON Schema to a regular hedge grammar and then executes validation via Katydid's [validator-go](github.com/katydid/validator-go).

## Usage Example

```go
	schemaBytes := []byte(`
    { "title": "small jsonschema for a blogpost",
      "type":"object", "additionalProperties":false, "required": ["content"],
      "properties": {
        "content": { "type":"string" },
        "author": { "$ref":"#/definitions/user-profile" } },
      "definitions": { "user-profile": {
        "type": "object", "additionalProperties":false, "required": ["username"], 
        "properties": {
          "username": { "type":"string" },
          "email": { "type":"string", "format":"email" } } } } }`)
	compiled, err := Compile(schemaBytes)
	...
	input := []byte(`{"content": "Dragons", "author": {"username": "Khaleesi"}}`)
	matched, err := compiled.MatchBytes(input)
	...
```

## Test Suites passed

* Draft4 (excluding `uniqueItems` and `remoteRef`)

Running tests requires cloning:
* JSON Schema Test Suite https://github.com/json-schema-org/JSON-Schema-Test-Suite to `./src/github.com/json-schema-org/JSON-Schema-Test-Suite`, where `.` is the same `.` as in `./src/github.com/katydid/paper-2027-mem-filter-go-jsonschema`, where we expect this repo to be cloned to, so the relative folder would be `../../../../src/github.com/json-schema-org/JSON-Schema-Test-Suite`.
* JSON Schema Benchmarks https://github.com/katydid/paper-2027-mem-filter-benchmarks-jsonschema to `./src/github.com/katydid/validator-jsonschema-benchmarks`, where `.` is the same `.` as in `./src/github.com/katydid/paper-2027-mem-filter-go-jsonschema`, where we expect this repo to be cloned to, so the relative folder would be `../../../../src/github.com/katydid/validator-jsonschema-benchmarks`.

## Unsupported

* [uniqueItems](./decisions/uniqueItems.md)