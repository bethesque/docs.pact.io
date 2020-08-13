---
title: Pact Request and Response Matching
custom_edit_url: https://github.com/pact-foundation/pact-reference/edit/master/rust/pact_matching/README.md
---
<!-- This file has been synced from the pact-foundation/pact-reference repository. Please do not edit it directly. The URL of the source file can be found in the custom_edit_url value above -->

This library implements the core matching logic required for matching HTTP requests and responses. It is based on the
[V3 pact specification](https://github.com/pact-foundation/pact-specification/tree/version-3).

[Online rust docs](https://docs.rs/pact_matching/)

## To use it

To use it, add it to your dependencies in your cargo manifest:

```toml
[dependencies]
pact_matching = "0.6.3"
```

This crate provides two functions: `match_request` and `match_response`. These functions take an expected and actual request or response
model from the `models` module, and return a vector of mismatches.

To compare any incoming request, it first needs to be converted to a `models::Request` and then can be compared. Same for
any response.

## Reading and writing Pact files

The `Pact` struct in the `models` module has methods to read and write pact JSON files. It supports all the specification
versions up to V3, but will converted a V1, V1.1 and V2 spec file to a V3 format.

## Matching request and response parts

V3 specification matching is supported for both JSON and XML bodies, headers, query strings and request paths.

To understand the basic rules of matching, see [Gotchas](https://docs.pact.io/getting_started/matching/gotchas).
For example test cases for matching, see the [Pact Specification Project, version 3](https://github.com/bethesque/pact-specification/tree/version-3).

By default, Pact will use string equality matching following Postel's Law. This means
that for an actual value to match an expected one, they both must consist of the same
sequence of characters. For collections (basically Maps and Lists), they must have the
same elements that match in the same sequence, with cases where the additional elements
in an actual Map are ignored.

Matching rules can be defined for both request and response elements based on a pseudo JSON-Path
syntax.

### Matching Bodies

For the most part, matching involves matching request and response bodies in JSON or XML format.
Other formats will either have their own matching rules, or will follow the JSON one.

#### JSON body matching rules

Bodies consist of Objects (Maps of Key-Value pairs), Arrays (Lists) and values (Strings, Numbers, true, false, null).
Body matching rules are prefixed with `$.`.

The following method is used to determine if two bodies match:

1. If both the actual body and expected body are empty, the bodies match.
2. If the actual body is non-empty, and the expected body empty, the bodies match.
3. If the actual body is empty, and the expected body non-empty, the bodies don't match.
4. Otherwise do a comparison on the contents of the bodies.

##### For the body contents comparison:

1. If the actual and expected values are both Objects, compare as Maps.
2. If the actual and expected values are both Arrays, compare as Lists.
3. If the expected value is an Object, and the actual is not, they don't match.
4. If the expected value is an Array, and the actual is not, they don't match.
5. Otherwise, compare the values

##### For comparing Maps

1. If the actual map is non-empty while the expected is empty, they don't match.
2. If we allow unexpected keys, and the number of expected keys is greater than the actual keys,
they don't match.
3. If we don't allow unexpected keys, and the expected and actual maps don't have the
same number of keys, they don't match.
4. Otherwise, for each expected key and value pair:
    1. if the actual map contains the key, compare the values
    2. otherwise they don't match

Postel's law governs if we allow unexpected keys or not.

##### For comparing lists

1. If there is a body matcher defined that matches the path to the list, default
to that matcher and then compare the list contents.
2. If the expected list is empty and the actual one is not, the lists don't match.
3. Otherwise
    1. compare the list sizes
    2. compare the list contents

###### For comparing list contents

1. For each value in the expected list:
    1. If the index of the value is less than the actual list's size, compare the value
       with the actual value at the same index using the method for comparing values.
    2. Otherwise the value doesn't match

##### For comparing values

1. If there is a matcher defined that matches the path to the value, default to that
matcher
2. Otherwise compare the values using equality.

#### XML body matching rules

Bodies consist of a root element, Elements (Lists with children), Attributes (Maps) and values (Strings).
Body matching rules are prefixed with `$.`.

The following method is used to determine if two bodies match:

1. If both the actual body and expected body are empty, the bodies match.
2. If the actual body is non-empty, and the expected body empty, the bodies match.
3. If the actual body is empty, and the expected body non-empty, the bodies don't match.
4. Otherwise do a comparison on the contents of the bodies.

##### For the body contents comparison:

Start by comparing the root element.

##### For comparing elements

1. If there is a body matcher defined that matches the path to the element, default
to that matcher on the elements name or children.
2. Otherwise the elements match if they have the same name.

Then, if there are no mismatches:

1. compare the attributes of the element
2. compare the child elements
3. compare the text nodes

##### For comparing attributes

Attributes are treated as a map of key-value pairs.

1. If the actual map is non-empty while the expected is empty, they don't match.
2. If we allow unexpected keys, and the number of expected keys is greater than the actual keys,
they don't match.
3. If we don't allow unexpected keys, and the expected and actual maps don't have the
same number of keys, they don't match.

Then, for each expected key and value pair:

1. if the actual map contains the key, compare the values
2. otherwise they don't match

Postel's law governs if we allow unexpected keys or not. Note for matching paths, attribute names are prefixed with an `@`.

###### For comparing child elements

1. If there is a matcher defined for the path to the child elements, then pad out the expected child elements to have the
same size as the actual child elements.
2. Otherwise
    1. If the actual children is non-empty while the expected is empty, they don't match.
    2. If we allow unexpected keys, and the number of expected children is greater than the actual children,
    they don't match.
    3. If we don't allow unexpected keys, and the expected and actual children don't have the
    same number of elements, they don't match.

Then, for each expected and actual element pair, compare them using the rules for comparing elements.

##### For comparing text nodes

Text nodes are combined into a single string and then compared as values.

1. If there is a matcher defined that matches the path to the text node (text node paths end with `#text`), default to that
matcher
2. Otherwise compare the text using equality.


##### For comparing values

1. If there is a matcher defined that matches the path to the value, default to that
matcher
2. Otherwise compare the values using equality.

### Matching Paths

Paths are matched by the following:

1. If there is a matcher defined for `path`, default to that matcher.
2. Otherwise paths are compared as Strings

### Matching Queries

1. If the actual and expected query strings are empty, they match.
2. If the actual is not empty while the expected is, they don't match.
3. If the actual is empty while the expected is not, they don't match.
4. Otherwise convert both into a Map of keys mapped to a list values, and compare those.

#### Matching Query Maps

Query strings are parsed into a Map of keys mapped to lists of values. Key value
pairs can be in any order, but when the same key appears more than once the values
are compared in the order they appear in the query string.

### Matching Headers

1. Do a case-insensitive sort of the headers by keys
2. For each expected header in the sorted list:
    1. If the actual headers contain that key, compare the header values
    2. Otherwise the header does not match

For matching header values:

1. If there is a matcher defined for `header.<HEADER_KEY>`, default to that matcher
2. Otherwise strip all whitespace after commas and compare the resulting strings.

#### Matching Request Headers

Request headers are matched by excluding the cookie header.

#### Matching Request cookies

If the list of expected cookies contains all the actual cookies, the cookies match.

### Matching Status Codes

Status codes are compared as integer values.

### Matching HTTP Methods

The actual and expected methods are compared as case-insensitive strings.

## Matching Rules

Pact supports extending the matching rules on each type of object (Request or Response) with a `matchingRules` element in the pact file.
This is a map of JSON path strings to a matcher. When an item is being compared, if there is an entry in the matching
rules that corresponds to the path to the item, the comparison will be delegated to the defined matcher. Note that the
matching rules cascade, so a rule can be specified on a value and will apply to all children of that value.

## Matcher Path expressions

Pact does not support the full JSON path expressions, only ones that match the following rules:

1. All paths start with a dollar (`$`), representing the root.
2. All path elements are separated by periods (`.`), except array indices which use square brackets (`[]`).
3. Path elements represent keys.
4. A star (`*`) can be used to match all keys of a map or all items of an array (one level only).

So the expression `$.item1.level[2].id` will match the highlighted item in the following body:

```js
{
  "item1": {
    "level": [
      {
        "id": 100
      },
      {
        "id": 101
      },
      {
        "id": 102 // <---- $.item1.level[2].id
      },
      {
        "id": 103
      }
    ]
  }
}
```

while `$.*.level[*].id` will match all the ids of all the levels for all items.

### Matcher selection algorithm

Due to the star notation, there can be multiple matcher paths defined that correspond to an item. The first, most
specific expression is selected by assigning weightings to each path element and taking the product of the weightings.
The matcher with the path with the largest weighting is used.

* The root node (`$`) is assigned the value 2.
* Any path element that does not match is assigned the value 0.
* Any property name that matches a path element is assigned the value 2.
* Any array index that matches a path element is assigned the value 2.
* Any star (`*`) that matches a property or array index is assigned the value 1.
* Everything else is assigned the value 0.

So for the body with highlighted item:

```js
{
  "item1": {
    "level": [
      {
        "id": 100
      },
      {
        "id": 101
      },
      {
        "id": 102 // <--- Item under consideration
      },
      {
        "id": 103
      }
    ]
  }
}
```

The expressions will have the following weightings:

| expression | weighting calculation | weighting |
|------------|-----------------------|-----------|
| $ | $(2) | 2 |
| $.item1 | $(2).item1(2) | 4 |
| $.item2 | $(2).item2(0) | 0 |
| $.item1.level | $(2).item1(2).level(2) | 8 |
| $.item1.level[1] | $(2).item1(2).level(2)[1(2)] | 16 |
| $.item1.level[1].id | $(2).item1(2).level(2)[1(2)].id(2) | 32 |
| $.item1.level[1].name | $(2).item1(2).level(2)[1(2)].name(0) | 0 |
| $.item1.level[2] | $(2).item1(2).level(2)[2(0)] | 0 |
| $.item1.level[2].id | $(2).item1(2).level(2)[2(0)].id(2) | 0 |
| $.item1.level[*].id | $(2).item1(2).level(2)[*(1)].id(2) | 16 |
| $.\*.level[\*].id | $(2).*(1).level(2)[*(1)].id(2) | 8 |

So for the item with id 102, the matcher with path `$.item1.level[1].id` and weighting 32 will be selected.

## Supported matchers

The following matchers are supported:

| matcher | example configuration | description |
|---------|-----------------------|-------------|
| Equality | `{ "match": "equality" }` | This is the default matcher, and relies on the equals operator |
| Regex | `{ "match": "regex", "regex": "\\d+" }` | This executes a regular expression match against the string representation of a values. |
| Type | `{ "match": "type" }` | This executes a type based match against the values, that is, they are equal if they are the same type. |
| MinType | `{ "match": "type", "min": 2 }` | This executes a type based match against the values, that is, they are equal if they are the same type. In addition, if the values represent a collection, the length of the actual value is compared against the minimum. |
| MaxType | `{ "match": "type", "max": 10 }` | This executes a type based match against the values, that is, they are equal if they are the same type. In addition, if the values represent a collection, the length of the actual value is compared against the maximum. |
| MinMaxType | `{ "match": "type", "max": 10, "min": 2 }` | This executes a type based match against the values, that is, they are equal if they are the same type. In addition, if the values represent a collection, the length of the actual value is compared against the minimum and maximum. |
| Include | `{ "match": "include", "value": "substr" }` | This checks if the string representation of a values contains the substring. |
| Integer | `{ "match": "integer" }` | This checks if the type of the value is an integer. |
| Decimal | `{ "match": "decimal" }` | This checks if the type of the value is a number with decimal places. |
| Number | `{ "match": "number" }` | This checks if the type of the value is a number. |
