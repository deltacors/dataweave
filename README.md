# Dataweave
List of basic transformations using DataWeave

## Part 1

### payload  
This payload is used for the following examples
```
{
    "firstName": "Max",
    "lasttName": "Mule",
    "company": "Microsoft"
}
```

### define global vars
```
%dw 2.0
output application/json
var test = "hello world"
---
```

### define local vars
```
%dw 2.0
output application/json
---
using(test = "hello world") payload.exampleKey ++ test
```

### define "inline" function
```
%dw 2.0
output application/json
fun exampleFunction(args) = if(args.exampleKey == "Mulesoft") "A" else "B"
---
exampleFunction(payload)
```

### define "do" functions
```
%dw 2.0
output application/json
fun exampleFunction(args) = do {
    if(args.exampleKey == "Mulesoft") "A" else "B"
}
---
exampleFunction(payload)
```

### match function
```
%dw 2.0
output application/json
fun exampleMatcher(args) = args.exampleKey match {
    case literalMatch: "String to check" -> "A"
    else -> "B" ++ $
}
```


## Part 2

### payload  
This payload is used for the following examples
```
{
  "entries": [
    {
      "firstName": "Max",
      "lastName": "Mule",
      "company": "MuleSoft",
      "email": "maxthemule@mulesoft.com"
    },
    {
      "firstName": "Astro",
      "lastName": "Nomical",
      "company": "Salesforce",
      "email": "astro@salesforce.com"
    },
    {
      "firstName": "Cloudy",
      "lastName": "the Goat",
      "company": "Salesforce",
      "email": "cloudy@salesforce.com"
    }
  ]
}
```

### dynamic elements
```
%dw 2.0
output application/json
---
payload.entries[?(lower($.company) == "salesforce")]
```

### map
```
%dw 2.0
output application/json
---
payload map (item, index) -> {
  "index": index,
  "name": item.firstName ++ " " ++ item.lastName,
  "company": item.company
}
```
