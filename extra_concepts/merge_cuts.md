## Merge Cuts

Merge cuts are used in `dark record` to create a cut object by successively overwriting cut files _left to right_ staring with the reel cut:

Given these three files:

```jsonc
// the post reel cut file
{
  "ADDRESS": "https://httpbin.org",
  "BODY": "the exact body that is sent"
}
```

```jsonc
// override1.json
{
  "ADDRESS": "http://localhost:8080",
  "BODY": "override1 body"
}
```

```jsonc
// override2.json
{
  "BODY": "override 2 body"
}
```



```sh
dark -v record ./test_data post --cut ./test_data/post.cut.json override1.json override2.json
```

The command above should produce the cut register below once recording starts:

```json
{
  "ADDRESS": "http://localhost:8080",
  "BODY": "override 2 body"
}
```
