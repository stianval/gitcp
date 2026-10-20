### Strategy
This file will be double moved

### How
```bash
mkdir -p copies
mkdir -p originals
cp to_double_move.md copies/
mv to_double_move.md originals/
git add copies/to_double_move.md originals/to_double_move.md to_double_move.md
git commit -m "Double moved to_double_move.md"
mv originals/to_double_move.md to_double_move.md
git add originals/to_double_move.md to_double_move.md
git commit -m "Moved back original copy of to_double_move.md"
```

### Hypothesis
`git blame` will work for both `to_double_move.md` and
`copies/to_double_move.md` without too much further ado.
