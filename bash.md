
# TOC
[Clang-format on a folder](Clang-format-on-a-folder)

# Clang-format on a folder
use fd and clang-format to format all C/C++ files recursively in a folder structure
``` 
fdfind -e h -e hpp -e cpp -e c -x clang-format --style=file -i
```
