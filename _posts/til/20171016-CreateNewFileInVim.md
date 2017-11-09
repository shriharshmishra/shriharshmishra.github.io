#VIM Trick 1 - Create new file using NERDTree plugin 

Many VIM users depend on NERDTree plugin to browse directory but rare use it to create a new file on without leaving Vim (at least I was one such Vim user). 

I found nice blog entry which told me how to do so - [Link here](https://sookocheff.com/post/vim/creating-a-new-file-or-directoryin-vim-using-nerdtree/)

To summarize - 

- Open NERDTree panel
- Navigate to the directory where you want to create new file or directory. 
- Press `m` - which opens up NERDTree Filesystem Menu. 
- Type `a` to create the child node and type the name of file or directory. For directory append `/` at the end. 
- That's it. 



#VIM Trick 2 - Create new file using VIM built-in feature. 

While I was experimenting on the steps above I made a silly mistake. I didn't open the newly created file before typing my experiment with NERDTree. I decided to rely on VIM help. This is what I did - 

- Type `:h save` - this opened up the help window with VIM
- The `saveas` command in VIM helps us same the current buffer into a file. 
- Use `!` option to save to an existing file. 
- I will let the reader experiment with this with their own hands-on exercise. 
