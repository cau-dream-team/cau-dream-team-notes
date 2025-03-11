# 02. Lexical Analyzer 1

````mermaid
flowchart LR
    cs(Character Stream)
    le(Lexical Analyzer)
    to(Tokens)
    
    cs --> le --> to
````

```
a = b * 2

<a> <=> <b> <*> <2>

print "a=b*2"

<print> <"a=b*2">
```

## Concepts: tokens, patterns, lexemes
