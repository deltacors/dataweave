# Dataweave
List of basic transformations using DataWeave

## Part 1

### payload  
This payload is used for the following examples
```
{
    "firstName": "Max",
    "lastName": "Mule",
    "company": "Mulesoft",
    "exampleKey": "Test string"
}
```

### define global vars
```
%dw 2.0
output application/json
var test = "Hello world"
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
  "index": index, // "index": $$ can be used too
  "name": item.firstName ++ " " ++ item.lastName,
  "company": item.company
}
```

### payload  
This payload is used for the following examples
```
{
  "accountType": [
    {
      "users": [
        {
          "name": "Jordan",
          "company": "MuleSoft"
        },
        {
          "name": "Bob",
          "company": "Salesforce"
        }
      ],
      "admins": [
        {
          "name": "Max",
          "company": "MuleSoft"
        }
      ]
    }
  ]
}
```

### mapping using selectors 
```
%dw 2.0
output application/json
---
payload.accountType.*users map -> {
    userInfo: $
}
```

```
%dw 2.0
output application/json
---
payload.accountType.*admins map -> {
    index: $$,
    adminInfo: $.name ++ $.company // perchè produce un array e non una stringa?
}
```
