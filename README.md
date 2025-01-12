jqjs is a JavaScript implementation of the [jq] query language. It
implements the core language features in pure JavaScript.

The main entry point to jqjs is the compile function, which turns a jq
program string into a generator function:

    import jq from './jq.js'
    let filter = jq.compile(".x[].y")
    for (let v of filter({x:[{y:2}, {y:4}]}) { ... }

The module also has a prettyPrint function for rendering an object to
text.

Features
--------

jqjs supports *most* of the core jq language features, but lacks
functions and some of the advanced functionality. It also uses
JavaScript strings as backing, so does not have jq proper's Unicode
support.

- [x] Identity: `.`
- [x] Object Identifier-Index: `.foo`, `.foo.bar`
- [x] Generic Object Index: `.[<string>]`
- [x] Array Index: `.[2]`
- [x] Array/String Slice: `.[10:15]`
- [x] Array/Object Value Iterator: `.[]`
- [x] Comma: `,`
- [x] Pipe: `|`
- [x] Parentheses: `(...)`
- [x] Array Construction: `[...]`
- [x] Object Construction: `{ ... }` including shorthand `{ user, title }`
      and computed keys `{ (.x): true }`
- [x] Recursive Descent: `..`
- [x] Addition, subtraction, multiplication, division, and modulo,
  including the overloaded type operations.
- [x] Named functions
    - Built-in functions
        - [x] tostring, tonumber, path, length, keys, has, in, type, del
        - [x] empty, select, arrays, objects, booleans, numbers, strings, nulls
        - [x] map, map_values, add, sort, sort_by, explode, implode, split, join
        - [x] to_entries, from_entries, with_entries
        - [x] range/1, range/2, range/3
        - [x] any/0, any/1, any/2, all/0, all/1, all/2
        - [x] contains, inside
        - [ ] the others
    - [ ] User-defined functions
    - [ ] Mathematical functions
- [x] String interpolation: `\(foo)`
- [x] Format strings and escaping: `@text`, `@json`, `@html`, `@uri`, `@csv`,
  `@tsv`, `@sh`, `@base64`, `@base64d`
    - [x] Format interpolations: `@uri "https://google.com/search?q=\(.x)"`
- [x] Equality checks: `==`, `!=`
- [x] Comparisons: `<`, `>`, `<=`, `>=`
    - [x] Correct sorting order for unequal types (`null < 7`, `[] < {}`)
- [ ] Conditionals: `if A then B else C`
- [x] Alternative operator: `//`
- [ ] Try-catch: `try EXP catch EXP`
  - [x] Error Suppression operator `?`
- [ ] Regular expressions
- [x] Variable/Symbolic Binding Operator `... as $identifier | ...`
- [x] Reduce: `reduce .[] as $item (0; . + $item)`
- [ ] foreach: `foreach .[] as $item (...;...;...)`
- [ ] Recursion: `recurse(.children[])`
- [ ] I/O (unlikely to make sense here)
- [x] Update-assignment: `.posts[].comments |= . + ["Another"]`
- [x] Arithmetic update-assignment: `+=`, `-=`, `*=`, `/=`, `%=`, `//=`
- [ ] Plain assignment: `(.a,.b) = range(2)`
- [ ] Modules with `import` and `include`

Performance
-----------

Not great.

The intention is to be semantically correct first and to have clear code
second. Performance improvements sit after that, if at all. Executing a
program may reëvaluate parts of it or traverse the object multiple times
where that makes things simpler, and internally evaluation happens by
tree-walking the input syntax.

Demonstration
-------------

[demo.html] is a [live demo](https://mwh.github.io/jqjs/demo.html) of how
to use jqjs that lets you enter a jq program and an input JSON value and
see the output JSON values it produces.


[jq]: https://stedolan.github.io/jq/
[demo.html]: demo.html
