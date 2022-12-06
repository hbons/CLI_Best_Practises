# CLI Best Practises

It may be that your product provides (or requires) some interaction on the command line interface (CLI) level. This document aims to provide guidelines and best practises to create a usable CLI application. These best practises were set up to work in a world with lots of different kinds of CLI tools adhering to many different conventions.

## Commands

A command is a reference to a program, that can be run from the CLI.

Commands are usually structured like this:

    command [OPTIONS] [ARGUMENTS]

Don't make command names too long. Try to make them should be at least two characters, but no more than nine characters long. Command names can be either nouns or verbs, but try to be as descriptive and self-documenting as possible. Abbreviations are allowed.

## Arguments

Arguments are values that can be passed to a command as parameters. Usually arguments are treated as subjects. For example: you may pass a string of text or a file path to as an argument to a command to perform operations on.

Always require an argument if your command can do something descructive. You don't want people to run your command without arguments in an attempt to get help and end up with unintended consequences.

Don't just continue on invalid arguments. always mention what the error is and stop the process.

Be forgiving about synonyms. Allow for quit but also exit for example, to prevent annoyance.

## Options

Unlike regular arguments, options are prefixed by a - or \--. Commands have a predefined set of options that can be used to alter the default behaiviour of a program. Multiple options may be applied at the same time.

All options may precede arguments or may be put after them.

People shouldn't have to move the cursor back when they forgot to type options before the arguments. Allow options at any position in the command syntax:

    command --option arguments # Valid
    command arguments --option # Also valid

Provide both long (\--long) and short (-s) notations for every option. Shorter options are faster to type in day to day use, whilst longer option make commands easier to understand when they're being used in automation processes or scripts.

Make long option names as descriptive as possible, without using too many characters. Try keeping the length to nine characters or fewer.

If possible, use the first character of the long option name as the character for the short option. If that character is already taken by a different short option, use a character that's as distinctive as possible for that word.

Unix is case sensitive generally, so don't be permissive with case changes. -v and -V may have different meanings and shouldn't be interchangable.

Options without option arguments should be accepted when grouped behind one - indicator. For this reason, don't use the short option notation for long option names, as the whole could be confused as a series of options. For example: -abc is the same as specifying the -a, -b, and -c options at once, whilst \--abc triggers the "abc" long option.

Error out when two mutually exclusive options are given. Never assume one over the other and continue. Tell the user what went wrong.

### Option Arguments

Some options require their own arguments. Option arguments are handy when your command has many options that are variations of the same thing. Grouping them together can make your program easier to understand and documentation easier to read. For example:

    eggs [--boil] [--pouch] [--fry]
    eggs --method=[boil|pouch|fry]

Always use the = (equals) sign when specifying option arguments like so:

    command --option=argument

This is to prevent confusion about what the argument applies to. By using a space instead of the = sign it won't always be clear whether the following argument applies to the option or the command itself. Option arguments should not be optional:

    eggs --method=boil
    eggs --method # Doesn’t make sense

## Manuals

Having good documention available is important. There's a lot of information available on commands on the web, but your users may find themselves in a situation where they can't access the internet. If you can, provide manual pages with all of your commands. These should be available by calling the man command followed by your command name:

    man command

Also create a reference to the fact that this manual is available through your command's help screen. See the [Help Screen](#HelpScreen) section for an example on how to do this.

["What you need to know to write man pages"](http://archive09.linux.com/feature/34212) has more information on how to write a manual page.

## Help Screen

### Activation

Feel free to show the help screen when the users has entered an invalid command, but always mention what has gone wrong:

    Invalid argument: -g
    # Show the help screen here

Be permissive when people ask for help. Since most Linux distributions ship a mixed set of commands, it won't be immediately clear which argument to provide to aqcuire help. Next to \--help, also allow for help, -help, -h, -?, and ?.

### Anatomy

Keep the help screen as brief as possible. Move more in-depth information to the command's manual.

Keep the user's terminal application's width in mind. Large displays are common today, but there are situations where people have to work in more restricted environments or with older hardware. Try to keep every line within 80 characters, wrap lines after that number.

    Usage: mkdir [OPTION]... DIRECTORY...

    Create the DIRECTORY(ies), if they do not already exist.

    Options:
      -m=MODE, --mode=MODE    set file mode (as in chmod), not a=rwx - umask
      -p,      --parents      create parent directories as needed
      -v,      --verbose      print a message for each created directory
      -Z=CTX,  --context=CTX  set the SELinux security context of each created
                              directory to CTX
                              
      -h, --help              display this help and exit
      -V, --version           output version information and exit

    For complete documentation, run: 'man mkdir'
    Report problems to: youremail@domain.com

Sort the options by alphabet, using the letters of the short options. exceptions are \--version and \--help.

Provide both the \--help or (-h) and \--version (or -v) options at all times. They can be grouped together at the end of the list of options, seperated by a blank line. This makes the list of uncommon options somewhat easier to scan.

Supply a \--version option with all of your commands. The output must be easy for a program to parse. The version number starts after the last space. In addition, it contains the canonical name for the program, like this:

    Application Name 1.0

Add an email address or link to the issue tracker at the end of the screen if desired.

### Style

Indent text using two spaces. Also use two spaces to separate the descriptions from the options. Wrap long descriptions and keep the right indentation.

Optimise for scanning: align all the options vertically into columns.

## Exit codes

An exit code is a number that a command reports after when having finished execution. Exit codes can have a value from 0 to 255. Make sure the exit code of the help screen is zero, unless the help screen was invoked by providing an invalid argument or option.

The most common convention is to use an exit code of 0 for a successful execution, and 1 for failure. A higher number up to 255 may be used to define different types of errors. Knowing the reason why a command failed can be useful information in scripts, but only use higher exit codes if you document them well in the man page, otherwise just use 1.

## Command Chains

**TODO:** pipes, input files, etc.

## Tools and Sources

- [getopts](http://en.wikipedia.org/wiki/Getopt) is a library that helps you parse command line arguments and options.
- [The Open Group Base Specifications, Chapter 12](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html)\
- [GNU Coding Standards, Chapter 4](http://www.gnu.org/prep/standards/standards.html)

# About

© 2013 Hylke Bons
