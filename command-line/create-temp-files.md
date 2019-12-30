# Create Temp Files

```bash
#!/bin/bash

trap 'rm -f "$TMPFILE"' EXIT

TMPFILE=$(mktemp) || exit 1
echo "Our temp file is $TMPFILE"
```

Ref: [http://www.ruanyifeng.com/blog/2019/12/mktemp.html](http://www.ruanyifeng.com/blog/2019/12/mktemp.html)

