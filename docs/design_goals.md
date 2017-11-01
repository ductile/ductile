# Design Goals

Ductile should:

1. Parse: Be able to read JSON as text, and convert it to an appropriate native Rust data object
2. Stringify: Be able to output native representations to a JSON string
3. Be order preserving: Javascript objects have keys. JSON cannot guarantee that the order of those keys will be preserved, but Ductile can.
4. Be format preserving: The JSON it outputs should be formatted like the JSON it inputs (or otherwise controllable). It should know how and when and where to put tabs and white spaces.
5. Be quirk-preserving: [JSON is quirky](https://esdiscuss.org/topic/json-duplicate-keys). Ductile will respect that.
6. Our bias will lean towards [modern interpretations of the spec](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas#Trailing_commas_in_JSON)
7. Provide filters and decorators like [JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
8. Be a good exmaple of Rust Done Right
9. Be [free](https://www.gnu.org/philosophy/free-sw.en.html)

##  Intermediate data type

As we convert to and from JSON while preserving structure, a need to support a superset of JSON seems to emerge. It's properties should be well-defined. Let's call that Structure Preserving JSON. Additionally, we need to support annotations that inform how the object is formatted, but don't affect underlying data. Let's call that Annotated Structure Preserving JSON.

Given the following cases:

### exhibit A (well-formed JSON)

```json
{
  "private": true,
  "devDependencies": [
    "angular": "~3.4",
    "underscore": "~5.9",
    "gulp": "~3.9.1"
  ]
}
```

### exhibit B (arrays preserve order, but objects don't)

we can say that it is equivilant to

```json
{
  "devDependencies": [
    "angular": "~3.4",
    "underscore": "~5.9",
    "gulp": "~3.9.1"
  ],
  "private": true

}
```

### exhibit C (minified)

and also

```json
{"private":true,"devDependencies":["angular":"~3.4","underscore":"~5.9","gulp":"~3.9.1"]}
```

### exhibit D (funky formatting)

and also

```json
{
  "private": true, "devDependencies":    [
    "angular": "~3.4",
      "underscore": "~5.9" ,
    "gulp": "~3.9.1"]
  }
```

From the point of view of our three observers, we can say that:

1. Vanilla JSON sees all exhibits as equal
2. SP-JSON sees exhibit A,C,D as equal and B as different
3. SP-JSON<sup>a</sup> sees all exhibits as different.

##  Value proposition

- To avoid cognitive dissonance, it's nice to be able to manipulate JSON files without screwing up their formatting (think npm or yarn or anything with a visual component).
- When viewing your changes using git or anything that leverages `diff`, you want as little noise as possible.
- As a superset of JSON, SP-JSON may be appropriate for cases where you need a map, and the order of it's keys are important.
- Similarily, you may need a data-structure that can support multiple instances of the same key with different values (think HTTP headers).

##  Project Goal

1. Build something useful
2. Have fun learning Rust


