## Clang format (llvm)

- You can you create .clang-format in you root project to customize your format
- For more details you can search `clang format style option` for documentation.

```c++
  # Example config
  BasedOnStyle: LLVM
  IndentWidth: 4
  ColumnLimit: 100
  AllowShortIfStatementsOnASingleLine: AllIfsAndElse
  BreakBeforeBraces: Attach #Allman|Attach
  AlignConsecutiveAssignments: false
  AlignConsecutiveDeclarations: false
  AlignConsecutiveMacros: true
  AllowShortEnumsOnASingleLine: false
```
