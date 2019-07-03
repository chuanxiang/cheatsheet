
### Search history
C-r

### Clear Terminal Screen
C-I

### Paste
`%paste` - Paste code and execute
`%cpaste` - Paste multi code blocks

### Run External Code
`%run` xxx.py

### Timing Code Execution
`%timeit statement`
`%%timeit` For multi-lines code
`%time` Time the execution of a single statement
`%timeit` Time repeated execution of a single statement for more accuracy
`%prun` Run code with the profiler
`%lprun` Run code with the line-by-line profiler
`%memit` Measure the memory use of a single statement
`%mprun` Run code with the line-by-line memory profiler
```
pip install line_profiler
%load_ext line_profiler
pip install memory_profiler
%load_ext memory_profiler
```

### Help for Magic commands
%magic
%lsmagic

### Input/Output of IPython
```
In - List Object
Out - dictionary object
_ - Last Output
__ - 2nd last Output
___ - 3rd last Output
_n - nth last output, eg: _2: 2nd last output
```

### Control exception trace output
%xmode

### Debug
%debug - use print/up/down check context of last error
%pdb on - turn on debug automatically after exception

### Write cell to file
`%%file [-a] filename`
