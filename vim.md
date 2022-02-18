# CTags
- Install universal-ctags
- Generate ctags for only c/c++ by running  
    - ```ctags.exe -R --languages=C,C++ --kinds-C++=+pLf --kinds-C=+pLf --fields=+iaS --extras=+q+f -f full_path_to_tags_storage_folder\tags --exclude=bin --exclude=Simulator --exclude=Debug --exclude=Release --sort=yes --tag-relative=always full_path_to_folder_with_code\* ```
- In vimrc file set   
    - ```set full_path_to_tags_storage_folder/tags ```

```--tag-relative``` must be set to always to work with the FZF command Tags. ```--tag-relative=always``` generate a relative path based on the tag file and FZF opens the file based on the path of the tag file. Therefore the tag paths in the tag file must be relative to the tag file. This also works with the other tag commands. See [ctags(1)](https://docs.ctags.io/en/latest/man/ctags.1.html) for more.  
This is at least requeired for windows
    
- Tag cheatsheet

| **Command** | **Description**         |
|-------------|-------------------------|
| CTRL+]      | Go-to definition of tag |
| CTRL+o      | Go back to before jump  |
| CRTL+t      | Go on up in tag stack   |
| :Tags       | Open FZF and search     |

# FZF 
- Install FZF, ripgrep and ag
- If on windows in order to have proper previews install Windows subsystem by running
    - ```wsl.exe --install```
