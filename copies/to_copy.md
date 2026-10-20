### Strategy
This file will be be copied

### How
```bash
mkdir -p copies
cp to_copy.md copies/
git add copies/to_copy.md
git commit -m "Copied to_copy.md"
```

### Hypothesis
`git blame` will work well for `to_copy.md`, but will need more flags to view
proper history for `copies/to_copy.md`.
