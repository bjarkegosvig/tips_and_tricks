# Search replace in folder
1) Open vim and cd to wanted folder
2) Populate the args list with wanted file types e.g. .py ```:args **/*.py```
3) Do search replace with :argdo e.g.  
    ```:argdo %s/\<np.float\>/e | update```  
    ```\<specific_word\>```  ensure we match the specific_word and don't replace a matching pattern  
    ``` | update ``` writes only the files where something is changed
For more info see this stack-overflow post https://vi.stackexchange.com/questions/2776/vim-search-replace-all-files-in-current-project-folder

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

# Telescope-project
## Add windows project with space in folder path
- In %localappdata%/nvim-data/telescope-project.txt add a line like this 
    ```scratch_code=c:\Users\bjkn\Onedrive - Demant\scratch_code=w0=1=1```

# LSP
## setup a .clangd configuration file
Clangd is used as LSP server when developing c/c++ code. In the project root you can put a .clangd configuration file, which tells clangd how to behave, add extra compiler arguments and system includes.
It can also be used to specify which compile_commands.json file to use, depending on which folder you have opened a file from. Below is an example on a small .clangd configuration file.
For more https://clangd.llvm.org/config
```
# Set compile_commands.json and additional compiler flags / compiler standard include path for the dsp
If:
  PathMatch: [sdk/dsp/.*, dsp_lib/.*, dsp_middleware/.*, epos_dsp_app/.*] 
CompileFlags:
# set the folder where the compile_commands.json file are located
  CompilationDatabase: ./sdk/dsp/out/
  Add: [-I/home/bjkn/xtensa/XtDevTools/install/builds/RI-2021.8-linux/AIR_PREMIUM_G3_HIFI5/xtensa-elf/arch/include,
        -I/home/bjkn/xtensa/XtDevTools/install/builds/RI-2021.8-linux/AIR_PREMIUM_G3_HIFI5/xtensa-elf/include,
        -I/home/bjkn/xtensa/XtDevTools/install/tools/RI-2021.8-linux/XtensaTools/llvm/lib/clang/10.0.1/include,
        -I/home/bjkn/xtensa//XtDevTools/install/builds/RI-2021.8-linux/AIR_PREMIUM_G3_HIFI5/xtensa-elf/arch/include/xtensa/config,
        -I/home/bjkn/xtensa//XtDevTools/install/builds/RI-2021.8-linux/AIR_PREMIUM_G3_HIFI5/xtensa-elf/arch/include/xtensa/tie,
        -D_ILP32 1,
        -D__ATOMIC_ACQUIRE 2,
        -D__ATOMIC_ACQ_REL 4,
        -D__ATOMIC_CONSUME 1,
        -D__ATOMIC_RELAXED 0,
        -D__ATOMIC_RELEASE 3,
]

# Must have --- to separate If sections, otherwise clangd will see the entries as duplicates and skip them.
---

# Setup separate compile_commands.json file and compiler flags for MCU code
If:
  PathMatch: [sdk/mcu/.*, epos_middleware/.*, epos_app/.*, epos_apps/.*] 
CompileFlags:
# set the folder where the compile_commands.json file are located
  CompilationDatabase: ./sdk/mcu/out/
  Add: [  -I/home/bjkn/repos/headset_ref/sdk/mcu/tools/gcc9.2.1/linux/gcc-arm-none-eabi/bin/../lib/gcc/arm-none-eabi/9.2.1/include,
          -I/home/bjkn/repos/headset_ref/sdk/mcu/tools/gcc9.2.1/linux/gcc-arm-none-eabi/bin/../lib/gcc/arm-none-eabi/9.2.1/include-fixed,
```

# Close buffers
## Close buffers from a certain folder
Use \<C-a\> to complete all matches.
type
```
:bd path/to/folder/*
```
or
```
:bd *.filetype
```
Then press 
```
Ctrl + a
```
to compete all matches. Finally press \<Enter\> to close all the matches.

## Close all buffers except current
```
:%bd|e#
```

