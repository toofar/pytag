Requires python3 and eyed3. Modeled off of the tag editor in Predixis
Music Magic Mixer. That being the source to destination pattern matching
thing.


    usage: pytag [-h] [-d] [-f] [-r] [-o] [-l] [-e [myeditor]]
                 [source] [dest] [file [file ...]]
    
    Usage:
        pytag file...
        pytag source dest file...
        pytag -f dest file...
        pytag dest file...
    
        In the first usage just list the tags of file(s). Equivalent to -l.
    
        In the second and third usage set the tags specified in dest from
        the string specified in source. Passing -f is equivalent to passing
        source as "{filename}".
    
        In the fourth usage print the string specified in dest with
        substitutions applied.
    
        The source argument is used to provide a string that dest matches
        against to extract tags from. It can use the python curly-brace
        format string syntax to substitute values into the string.
        Eg "{p1}::{album}::{filename}".
        Valid keys are lower case ID3 tags, when present in the file, along
        with a few extra ones:
            filename  The basename of the file.
            ext       The file extension.
            pdir      The dirname of the absolute path to the file.
            p1        The parent directory of the file.
            p2..N     The Nth parent directory of the file.
        Use the -l argument to see all available ones for a file (except for
        the pN key which are dynamiccaly generated).
    
        The dest argument is used to set tags from match expressions or to
        rename the file to the specified string (with the same keys available
        as specified for source). Additionally to what was described above
        for source you can suffix a keyname with an asterix to specify a
        greedy match (eg "{track*}"). You can also, with the -r flag, pass a
        valid python regex. It can still have curly-brace delimited keys in
        it and name match groups are matched to tags to set.
    
        There are is is one other transform available in dest right now eg
        "{track^}" (or {track*^} if you want it greedy) will turn all
        underscores to spaces and sentence case tag. Eg.
    
            like_a_stone_(live)_(bonus_track) => Like a Stone (Live) (Bonus Track)
    
    Examples:
    
        # Set tags from filename
        pytag -f "{track}. {artist} - {title}.mp3" file
    
        # Set tags from parent dir, fixed strings and filename
        pytag "{p1}::Big Joe::2013::{filename}" "{album}::{artist}::{year}::{track} {title}_junk.mp3" file
    
        # Rename file(s) from tags.
        pytag "{track:02} - {artist} - {title}.{ext}" "{filename}" file
    
    positional arguments:
      source
      dest
      file                  The file(s) to operate on.
    
    optional arguments:
      -h, --help            show this help message and exit
      -d, --dry-run         Print what would be done, but don't do it.
      -f                    Use filename as source. Equivalent to passing
                            "{filename}" as the first positional argument.
      -r, --regex           The dest string is a (python) regex. Don't escape it.
      -o, --stdout          Print the transformed source string to stdout. Don't
                            perform the operation. When renaming a file just print
                            out the new filename. When setting tags print out the
                            changed tags in rfc822(-ish) format.
      -l, --list            Just print the tags from the file (plus a couple
                            extra) in rfc822 format.
      -e [myeditor], --editor [myeditor]
                            Open (expanded) dest string(s) an editor before
                            applying to dest. Default if no source is specified is
                            dest. Default for source and dest if neither are
                            specified is
                            {filename}::{album}::{track}::{artist}::{title}. You
                            can specify arguments to the external program (like
                            sed) with shell quoting. Prefixing the command name
                            with '-' will use stdin and stdout otherwise a file to
                            operate on will be passed as the final argument.
