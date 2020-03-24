py, py3

py3do
	:pydo return "%s\t%d" % (line[::-1], len(line))
	:pydo if line: return "%4d: %s" % (linenr, line)

	:py3 << EOF
	needle = vim.eval('@a')
	replacement = vim.eval('@b')

	def py_vim_string_replace(str):
		return str.replace(needle, replacement)
	EOF
	:'<,'>py3do return py_vim_string_replace(line)

py3f[ile]
	:pyfile myscript.py

#Wrong
   if has('python')
     python << EOF
       this will NOT work!
   EOF
   endif

# Correct
Instead, put the Python command in a function and call that function:
>
    if has('python')
      function DefPython()
        python << EOF
          this works
    EOF
      endfunction
      call DefPython()
    endif


# Overview
	:python import vim
	:py print "Hello"		# displays a message
	:py vim.command(cmd)		# execute an Ex command
	:py w = vim.windows[n]		# gets window "n"
	:py cw = vim.current.window	# gets the current window
	:py b = vim.buffers[n]		# gets buffer "n"
	:py cb = vim.current.buffer	# gets the current buffer
	:py w.height = lines		# sets the window height
	:py w.cursor = (row, col)	# sets the window cursor position
	:py pos = w.cursor		# gets a tuple (row, col)
	:py name = b.name		# gets the buffer file name
	:py line = b[n]			# gets a line from the buffer
	:py lines = b[n:m]		# gets a list of lines
	:py num = len(b)		# gets the number of lines
	:py b[n] = str			# sets a line in the buffer
	:py b[n:m] = [str1, str2, str3]	# sets a number of lines at once
	:py del b[n]			# deletes a line
	:py del b[n:m]			# deletes a number of lines

# vim.command(str)					*python-command*
	Executes the vim (ex-mode) command str.  Returns None.
	Examples: >
	    :py vim.command("set tw=72")
	    :py vim.command("%s/aaa/bbb/g")

# vim.eval(str)						*python-eval*
	Evaluates the expression str using the vim internal expression
	evaluator (see |expression|).  Returns the expression result as:
	- a string if the Vim expression evaluates to a string or number
	- a list if the Vim expression evaluates to a Vim list
	- a dictionary if the Vim expression evaluates to a Vim dictionary
	Dictionaries and lists are recursively expanded.
	Examples: >
	    :py text_width = vim.eval("&tw")
	    :py str = vim.eval("12+12")		# NB result is a string! Use

# vim.buffers						*python-buffers*
	A mapping object providing access to the list of vim buffers.  The
	object supports the following operations: >
	    :py b = vim.buffers[i]	# Indexing (read-only)
	    :py b in vim.buffers	# Membership test
	    :py n = len(vim.buffers)	# Number of elements
	    :py for b in vim.buffers:	# Iterating over buffer list
<

# vim.windows						*python-windows*
	A sequence object providing access to the list of vim windows.  The
	object supports the following operations: >
	    :py w = vim.windows[i]	# Indexing (read-only)
	    :py w in vim.windows	# Membership test
	    :py n = len(vim.windows)	# Number of elements
	    :py for w in vim.windows:	# Sequential access
<	Note: vim.windows object always accesses current tab page. 
	|python-tabpage|.windows objects are bound to parent |python-tabpage| 
	object and always use windows from that tab page (or throw vim.error 
	in case tab page was deleted). You can keep a reference to both 
	without keeping a reference to vim module object or |python-tabpage|, 
	they will not lose their properties in this case.

# vim.current						*python-current*
	An object providing access (via specific attributes) to various
	"current" objects available in vim:
		vim.current.line	The current line (RW)		String
		vim.current.buffer	The current buffer (RW)		Buffer
		vim.current.window	The current window (RW)		Window
		vim.current.tabpage	The current tab page (RW)	TabPage
		vim.current.range	The current line range (RO)	Range

# vim.vars						*python-vars*
vim.vvars						*python-vvars*
	Dictionary-like objects holding dictionaries with global (|g:|) and 
	vim (|v:|) variables respectively. Identical to `vim.bindeval("g:")`, 
	but faster.

# The buffer object attributes are:
	b.vars		Dictionary-like object used to access 
			|buffer-variable|s.
	b.options	Mapping object (supports item getting, setting and 
			deleting) that provides access to buffer-local options 
			and buffer-local values of |global-local| options. Use 
			|python-window|.options if option is window-local, 
			this object will raise KeyError. If option is 
			|global-local| and local value is missing getting it 
			will return None.
	b.name		String, RW. Contains buffer name (full path).
			Note: when assigning to b.name |BufFilePre| and 
			|BufFilePost| autocommands are launched.
	b.number	Buffer number. Can be used as |python-buffers| key.
			Read-only.
	b.valid		True or False. Buffer object becomes invalid when 
			corresponding buffer is wiped out.

# The buffer object methods are:
	b.append(str)	Append a line to the buffer
	b.append(str, nr)  Idem, below line "nr"
	b.append(list)	Append a list of lines to the buffer
			Note that the option of supplying a list of strings to
			the append method differs from the equivalent method
			for Python's built-in list objects.
	b.append(list, nr)  Idem, below line "nr"
	b.mark(name)	Return a tuple (row,col) representing the position
			of the named mark (can also get the []"<> marks)
	b.range(s,e)	Return a range object (see |python-range|) which
			represents the part of the given buffer between line
			numbers s and e |inclusive|.

## Examples (assume b is the current buffer) >
	:py print b.name		# write the buffer file name
	:py b[0] = "hello!!!"		# replace the top line
	:py b[:] = None			# delete the whole buffer
	:py del b[:]			# delete the whole buffer
	:py b[0:0] = [ "a line" ]	# add a line at the top
	:py del b[2]			# delete a line (the third)
	:py b.append("bottom")		# add a line at the bottom
	:py n = len(b)			# number of lines
	:py (row,col) = b.mark('a')	# named mark
	:py r = b.range(1,5)		# a sub-range of the buffer
	:py b.vars["foo"] = "bar"	# assign b:foo variable
	:py b.options["ff"] = "dos"	# set fileformat
	:py del b.options["ar"]		# same as :set autoread<

# 4. Range objects					*python-range*
Range objects represent a part of a vim buffer.  You can obtain them in a
number of ways:
	- via vim.current.range (|python-current|)
	- from a buffer's range() method (|python-buffer|)
A range object is almost identical in operation to a buffer object.  However,
all operations are restricted to the lines within the range (this line range
can, of course, change as a result of slice assignments, line deletions, or
the range.append() method).

## The range object attributes are:
	r.start		Index of first line into the buffer
	r.end		Index of last line into the buffer

## The range object methods are:
	r.append(str)	Append a line to the range
	r.append(str, nr)  Idem, after line "nr"
	r.append(list)	Append a list of lines to the range
			Note that the option of supplying a list of strings to
			the append method differs from the equivalent method
			for Python's built-in list objects.
	r.append(list, nr)  Idem, after line "nr"

# 5. Window objects					*python-window*

Window objects represent vim windows.  You can obtain them in a number of ways:
	- via vim.current.window (|python-current|)
	- from indexing vim.windows (|python-windows|)
	- from indexing "windows" attribute of a tab page (|python-tabpage|)
	- from the "window" attribute of a tab page (|python-tabpage|)

You can manipulate window objects only through their attributes.  They have no
methods, and no sequence or other interface.

## Window attributes are:
	buffer (read-only)	The buffer displayed in this window
	cursor (read-write)	The current cursor position in the window
				This is a tuple, (row,col).
	height (read-write)	The window height, in rows
	width (read-write)	The window width, in columns
	vars (read-only)	The window |w:| variables. Attribute is 
				unassignable, but you can change window 
				variables this way
	options (read-only)	The window-local options. Attribute is 
				unassignable, but you can change window 
				options this way. Provides access only to 
				window-local options, for buffer-local use 
				|python-buffer| and for global ones use 
				|python-options|. If option is |global-local| 
				and local value is missing getting it will 
				return None.
	number (read-only)	Window number.  The first window has number 1.
				This is zero in case it cannot be determined
				(e.g. when the window object belongs to other
				tab page).
	row, col (read-only)	On-screen window position in display cells.
				First position is zero.
	tabpage (read-only)	Window tab page.
	valid (read-write)	True or False. Window object becomes invalid 
				when corresponding window is closed.
