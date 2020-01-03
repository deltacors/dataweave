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

### mapObject
```
%dw 2.0
output application/json
---
payload.accountType[0] mapObject (value, key) -> {
 index: key,
 accountInfo: value
}
```

### pluck
```
%dw 2.0
output application/json
---
payload.accountType[0] pluck ((value, key, index) -> {
  (index): (key),
  "accountInfo" : (value)
})
```

### payload  
This payload is used for the following examples
```
[
  1,
  2,
  3,
  4,
  5
]
```

### filter
```
%dw 2.0
output application/json
---
payload filter ((item, index) -> (item >= 2))
```
or
```
%dw 2.0
output application/json
---
payload filter ($$ >= 2)
```

### payload  
This payload is used for the following examples
```
{
  "count1": 1,
  "count2": 2,
  "count3": 3,
  "count4": 4,
  "count5": 5
}
```

### filterObject
```
%dw 2.0
output application/json
---
payload filterObject (value, key, index) -> (value > 2)
```

### payload  
This payload is used for the following examples
```
[
  5,
  5,
  5,
  5
]
```

### reduce
```
%dw 2.0
output application/json
---
payload reduce ($ + $$)
```


## Part 1

### payload  
This payload is used for the following examples
```
[
  2,
  2,
  2
]
```

### math functions
```
%dw 2.0
output application/json
---
random()
```

```
%dw 2.0
output application/json
---
sum(payload)
```

```
%dw 2.0
import * from dw::core::Numbers
output application/json
---
{
   a: toHex(123456789),
   b: toBinary(123456789),
   c: toRadixNumber(2,2),
   d: fromHex("75bcd15"),
   e: fromBinary(111010110111100110100010101),
   f: fromRadixNumber("10",10)
}
```


### splitBy
```
%dw 2.0
output application/json
---
"192.168.0.1" splitBy "."
```

### generate UUID
```
%dw 2.0
output application/json
---
uuid()
```

### replace
```
%dw 2.0
output application/json
---
"TEST1234" replace "T" with "7"
```

### payload  
This payload is used for the following examples
```
[ 
  [0.0, 0],
  [1,1], 
  [2,3],
  [5,8] 
]
```

### flatten
```
%dw 2.0
output application/json
---
flatten(payload) // [ 0.0, 0 1, 1, 2, 3, 5, 8 ]
```
