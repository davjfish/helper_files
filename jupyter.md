# Jupyter Commands

## Notebook
### From Command Mode (Blue)
- enter command mode: Esc
- new block above: a
- new block below: b
- delete block: d,d
- cut cell: x
- paste cell below: v
- change cell to markdown cell: m
- change cell to code cell: y

### From Edit Mode (Green)
- enter edit mode: Enter
- run block: Shift + Enter

### Markdown
- header: #
- bullet: *
- bold: ** text **
- split cell: Ctrl + Shift + -

### Debug Options
1. Use %debug right after error cell (doesn't allow easy stepping to next section etc.)
2. Use %%debug at top of cell to be debugged
    1. Can input variables to look at 
    2. Use 'n' to step to next section
    3. Use 'c' to step to continue
3. Import pdb; set breakpoint with pdb.set_trace() (plain Python debugging)
4. Use pixiedust
    1. pip install pixiedust (will also require matplotlib and pandas)
    2. import pixiedust
    3. at top of cell of interest %%pixie_debugger (opens interactice debugger)



## Terminal
Convert and add converted file to Jupyter, from command prompt:
```
jupyter nbconvert <file> -- to HTML (or PDF, etc..)
```

## Server notebook on LAN

- Create a notebook config file `jupyter notebook --generate-config`
- add the following lines at the end of the config file:
 ```
c.NotebookApp.ip = '0.0.0.0' # listen on all IPs
c.NotebookApp.token = ''     # disable authentication
c.NotebookApp.allow_origin = '*' # allow access from anywhere
c.NotebookApp.disable_check_xsrf = True # allow cross-site requests
```
- run the notebook: `jupyter notebook`