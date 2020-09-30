# Extra concepts

This page is decicated to additional concepts that extend the functionality of the filmReel format. 


## Full JSON object storage 

A *Cut Variable* can store any valid JSON object:

- This frame:
```json
{
  "protocol": "HTTP",
  "cut": {
    "to": {
      "COMPLEX_OBJECT": "'response'.'body'.'complex_object'"
    }
  },
  "request": {
    "body": {},
    "uri": "GET /complex_object"
  },
  "response": {
    "body": {
      "complex_object": "${COMPLEX_OBJECT}"
    },
    "status": 200
  }
}
```

- With this response:
```json
{
  "complex_object": {
    "a_lot_of_stuff": {
      "things": [
        1,
        2,
        3,
        4
      ]
    }
  }
}
```

- Gives us this *Cut File*:
```json
{
  "COMPLEX_OBJECT": {
    "a_lot_of_stuff": {
      "things": [
        1,
        2,
        3,
        4
      ]
    }
  }
}
```

## Retry attempts:

* retry `attempts` are held in the `request` object: `{"request":{"attempts": {"times": 5, "ms": 500}}}`

This *Frame* will try up to 5 times to get a correct response match before terminating with an error, waiting 500 miliseconds or half a second between requests:

```json
{
  "protocol": "HTTP",
  "cut": {
    "to": {
      "OBJECT": "'response'.'body'.'object'"
    }
  },
  "request": {
    "body": {},
    "uri": "GET /object",
    "attempts": {
      "times": 5,
      "ms": 500
    }
  },
  "response": {
    "body": {
      "complex_object": "${OBJECT}"
    },
    "status": 200
  }
}
```

## Hidden variables
Any variable with a leading underscore should not show up in any stdout or file output: `${_VARIABLE_NAME}`

## Ignoring variables
Variables containing only lowercase letters will be discarded\ from the *Cut Register* upon a successful *Frame* take: `${lowercase}` and not carry over into following frames. This is meant to decrease noise in the *Cut Register* and signal the relevance or lack of for a particular response.

* Lowercase variables can be combined with a leading underscore to ignore a variable and hide it from any output: `${_hidden_and_ignored}` 
