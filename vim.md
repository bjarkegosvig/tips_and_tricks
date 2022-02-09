# CTags
- Install universal-ctags
- Generate ctags for only c/c++ by running  
    - ```ctags.exe -R --languages=C,C++ --kinds-C++=+pLf --kinds-C=+pLf --fields=+iaS --extras=+q+f -f full_path_to_tags_storage_folder\tags --exclude=bin --exclude=Simulator --exclude=Debug --exclude=Release --sort=yes --tag-relative=never full_path_to_folder_with_code\* ```
- In vimrc file set   
    - ```set full_path_to_tags_storage_folder/tags ```
- Tag cheatsheet

| **Command** | **Description**         |
|-------------|-------------------------|
| CTRL+]      | Go-to definition of tag |
| CTRL+o      | Go back to before jump  |
|             |                         |
|             |                         |

# FZF 
- Install FZF, ripgrep and ag
- If on windows in order to have proper previews install Windows subsystem by running
    - ```wsl.exe --install```
