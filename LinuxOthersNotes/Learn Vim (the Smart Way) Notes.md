## [Learn Vim (the Smart Way)](https://github.com/iggredible/Learn-Vim)

### Buffers, windows, tabs
1. Buffers are in memory space where you write and edit some text.
   + ```:buffers``` or ```:ls``` to show buffer numbers
   + ```:buffer BUFFER#``` to show BUFFER# buffer in this window
2. Windows show buffers.
   + ```:split filename```
   + ```:vsplit filename```
   + ```ctrl + W + hjkl``` to navigate between windows in a tab
3. tabs show a collection of windows.
   + ```vim -p filename filename2*``` to open these files each in a seperate tab
   + ```:tabnext``` ```:tabprevious``` to navigate between tabs
   + command ```gt``` or ```gT``` or ```tab#gt``` to navigate between tabs
   
### Opening and searching files
1. ```:edit filename``` to open file
2. use file search tool ```fzf``` and ```ripgrep``` to search for files and search texts in files and replace texts for search results
   + command ```:Files``` to search for and list all files in the current project
   + command ```:Rg``` to search for text in files
3. search and replace in the whole project
4. search and replace in selected files
5. command ```:%bd | e# | bd#``` to clear the buffer