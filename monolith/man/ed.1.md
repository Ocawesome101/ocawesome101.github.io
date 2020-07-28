*NAME*
        ed - the standard Unix text editor

*SYNOPSIS
        ed* [*file*]

*DESCRIPTION
        ed* is a Lua implementation of the standard Unix text editor.  It does not feature all of the commands, but is similar enough to be recognizable.  It supports the *a*, *c*, *d*, *i*, *l*, *p*, *P*, *q*, *s*, and *w* commands.

*EXAMPLES*
        Command parameters should be given without spaces - for example, *w example* becomes *wexample*.

        *1,5s/*old*/*new*/*
            Substitute *old* for *new* on lines 1 through 5.

        *,p*
            Print the whole buffer.

*COPYRIGHT
        Original design and implementation* (c) 1969 Ken Thompson/Bell Labs. *Lua implementation* (c) 2020 Ocawesome101 under the GNU GPLv3.

*SEE ALSO
        led*(*1*), *fled*(*1*), *vled*(*1*), *editor*(*3*)
