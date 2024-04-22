
### Bash::Sugar README

Bash::Sugar is a collection of many utilities for file and 
environment management. Many of these are generic sysadmin 
utilities, but some are specific to managing perl projects. 

Much of the impetus for these tools is the difficulty 
inherent to nesting data structures and escapes in Bash. 
By using the tools in Bash::Sugar you can quickly generate 
highly dynamic jobs in one command line statement. 

What is different about Bash::Sugar, is that the programs 
are implemented both as perl methods, and as commands via 
simple symbolic links. This allows identical functionality 
when writing tools in perl, or in bash. 

This is implemented by using symlinks to the included 
executable: 

"sugar" 

### EXAMPLE: 

Bash::Sugar has a few date formats based on ISO-8601
which are useful for backups because they are self 
sorting. 

These are: 
"isodate", "isotime" and "isothu" (time host user).  

These exist both as perl methods: 

#! /bin/perl
my $S = Bash::Sugar->new()  ; 
print $S->isodate() ; # prints foo.2022-12-31

And as shell commands: 

#!/bin/bash 
echo "foo\.`isodate`" # prints foo.2022-12-31

This works because the distro already did this: 

/bin/ln -s sugar isodate 

### HOW IT WORKS

In this fashion the program "sugar", knows the name of  
symlink and and uses that name to find an identical method 
in the class Bash::Sugar class to run. It then outputs 
that output to STDOUT.  

### HOW TO USE 

Bash::Sugar has stream editors, formats, file interrogators, 
and temp file manager with automatic expirer.  

A solid knowledge of the perl regexp bestiary (which is much 
better than the standard bestiaries in most programs) can 
allow you to achieve some interesting things. Included is 
the program 'pexp', which functions like sed -E. It just 
takes a regexp on the CLI and you can stack it as many 
times as you want: 

# example: 

ls | pexp 's/(^.*$)/cp $1 \.$1/' | pexp "s/$/\.`isodate`/" | doeach  

# translates to: 

(for all files) (copy <filename> to .<filename>.<datestring>) 
and run each copy in a spawned shell) 

# continuing: 

Because we also have interrogators we can easily list only bash 
or only perl scripts. We can check perl packages names against 
their directories to verify they are all straight. (perl can 
throw some wonky errors if this line is broken sometimes) 

# tmp files: 

Tmp files are still useful in the bash environment because we are 
often working with non-threading processes. Because of this 
we can use the program "rtfn", to generate a random temp file name.  
"rtfn" cleans up after itself autoexpiring old tmp files. This is 
handy for using with macros in external programs (see the .vimrc 
in FvwmSkel) as we can copy out and copy in data cleanly. 

### LIST OF METHODS (most are also programs)

sub new  # constructor
sub dobyexename  # map symlinks to methods 
sub sugar  # the sugar executable runs help if used directly 
sub isodate  # iso8601 date
sub isotime  # iso8601 date_time
sub isothu  # iso8601 date_time_host_user (when, where, who) 
sub pexp  # simple perl stream editor like sed -E
sub capstr  # capitalize the first character an argument, or all on STDIN 
sub shoutstr  # capitalize the the whole argument, or all on STDIN
sub whisperstr  # lowercase a whole argument, or all on STDIN
sub chstring  # match & replace in file: chstring <filename> <oldstring> <newstring>
sub chterp  # change the interpreter line: chterp <filename> <pathtoexecutable> (don't include shabang)
sub chpack  # change the "package" line: chpack <filename> <newpackname>
sub class2fn  # convert a perl class to a filename. class2fn <Foo::Bar>
sub class2bfn  # convert a perl class to hidden backup filename. class2bfn <Foo::Bar> 
sub pack2fn  # convert a package line to a filename. pack2fn <package line>
sub fn2pack  # convert a filename to a perl package line. fn2pack <filename>
sub fn2class  # convert a filename to a perl class. fn2class <filename>
sub seeclass  # display the perl class is a file. seeclass <filename>
sub showpack  # same as seeclass (both exist for compat) 
sub seepack  # show the package line a perl package
sub seeterp  # show the interpreter line of a file
sub showmethod  # see a particular perl method in file seemethod -f <filename> -m <method> -i [interior only]
sub crlf  # strip crlf from dos files. 
sub addcrlf  # append crlfs to a file (never used, does it work?)
sub asciify  # strip UTF-8 garbage from web pastes
sub gettime  # get the current time params properly formatted
sub getuser  #  the current working user
sub hostonly  # the current hostname  (corrects for often broken "hostname" commands
sub gethostname  # maybe everything in the iso proggies should be in a lib? 
sub bashexpand  # ? incomplete ?
sub starexpand  # expand trailing * into a fully qualified array of files
sub singlequote  # normalize quotes on specified fields of an Pdt::L Object to be used to write an rc file.
sub sourceenv  # same as bash "source" but for perl
sub sourceprefix  # same as above, but exclude all but a specified prefix
sub envbyprefix   # grep environment variables by leading string 
sub rtfn  # return a fully qualified unique time stamped temp file name. Expire it automatically. 
sub doeach  # take a stack of lines, run each in an indevidual shell. (avoids ENV breakage)
sub stackbash  # source one or multiple bash files and run the last one in the argument chain
sub versioninplace    # inserts a dated version line in a file (waiting refactor) 
sub copyrightinplace  # inserts a copyright line in a file (waiting refactor) 
sub dohelp  # prints this file

### CONFIGURABLES 

rtfn  # random temp file name (entropy: 1/1m per second) 

	rtfn prints fully qualified filenames which are 
	reliably unique for the purposes of generating 
	tmp files. rtfn also expires (deletes) old temp 
	files automatically. 

	there are two environment variables that can be 
	defined to change rtfn behavior: 

	XTMPATH 
	
	defines the location of your temp file 
	directory. ($HOME/.tmp is the default) If for 
	example you want your files to be place 
	in ~/Project/.tmp you can add the following to 
	your .bashrc or .xinitrc file: 

	export XTMPPATH=$HOME/Project/.tmp

	XTMPEXPIRE

	defines the length of time (in seconds) to 
	wait before expiring old temp files. This 
	value is set to one hour (3600) by default. 
	So for example if you wanted to make the 
	expire interval two hours you can add the 
	following to your .bashrc file: 

	export XTMPEXPIRE=7200



### Bash::Sugar README

Bash::Sugar is a collection of many utilities for file and 
environment management. Many of these are generic sysadmin 
utilities, but some are specific to managing perl projects. 

Much of the impetus for these tools is the difficulty 
inherent to nesting data structures and escapes in Bash. 
By using the tools in Bash::Sugar you can quickly generate 
highly dynamic jobs in one command line statement. 

What is different about Bash::Sugar, is that the programs 
are implemented both as perl methods, and as commands via 
simple symbolic links. This allows identical functionality 
when writing tools in perl, or in bash. 

This is implemented by using symlinks to the included 
executable: 

"sugar" 

### EXAMPLE: 

Bash::Sugar has a few date formats based on ISO-8601
which are useful for backups because they are self 
sorting. 

These are: 
"isodate", "isotime" and "isothu" (time host user).  

These exist both as perl methods: 

#! /bin/perl
my $S = Bash::Sugar->new()  ; 
print $S->isodate() ; # prints foo.2022-12-31

And as shell commands: 

#!/bin/bash 
echo "foo\.`isodate`" # prints foo.2022-12-31

This works because the distro already did this: 

/bin/ln -s sugar isodate 

### HOW IT WORKS

In this fashion the program "sugar", knows the name of  
symlink and and uses that name to find an identical method 
in the class Bash::Sugar class to run. It then outputs 
that output to STDOUT.  

### HOW TO USE 

Bash::Sugar has stream editors, formats, file interrogators, 
and temp file manager with automatic expirer.  

A solid knowledge of the perl regexp bestiary (which is much 
better than the standard bestiaries in most programs) can 
allow you to achieve some interesting things. Included is 
the program 'pexp', which functions like sed -E. It just 
takes a regexp on the CLI and you can stack it as many 
times as you want: 

# example: 

ls | pexp 's/(^.*$)/cp $1 \.$1/' | pexp "s/$/\.`isodate`/" | doeach  

# translates to: 

(for all files) (copy <filename> to .<filename>.<datestring>) 
and run each copy in a spawned shell) 

# continuing: 

Because we also have interrogators we can easily list only bash 
or only perl scripts. We can check perl packages names against 
their directories to verify they are all straight. (perl can 
throw some wonky errors if this line is broken sometimes) 

# tmp files: 

Tmp files are still useful in the bash environment because we are 
often working with non-threading processes. Because of this 
we can use the program "rtfn", to generate a random temp file name.  
"rtfn" cleans up after itself autoexpiring old tmp files. This is 
handy for using with macros in external programs (see the .vimrc 
in FvwmSkel) as we can copy out and copy in data cleanly. 

### LIST OF METHODS (most are also programs)

sub new  # constructor
sub dobyexename  # map symlinks to methods 
sub sugar  # the sugar executable runs help if used directly 
sub isodate  # iso8601 date
sub isotime  # iso8601 date_time
sub isothu  # iso8601 date_time_host_user (when, where, who) 
sub pexp  # simple perl stream editor like sed -E
sub capstr  # capitalize the first character an argument, or all on STDIN 
sub shoutstr  # capitalize the the whole argument, or all on STDIN
sub whisperstr  # lowercase a whole argument, or all on STDIN
sub chstring  # match & replace in file: chstring <filename> <oldstring> <newstring>
sub chterp  # change the interpreter line: chterp <filename> <pathtoexecutable> (don't include shabang)
sub chpack  # change the "package" line: chpack <filename> <newpackname>
sub class2fn  # convert a perl class to a filename. class2fn <Foo::Bar>
sub class2bfn  # convert a perl class to hidden backup filename. class2bfn <Foo::Bar> 
sub pack2fn  # convert a package line to a filename. pack2fn <package line>
sub fn2pack  # convert a filename to a perl package line. fn2pack <filename>
sub fn2class  # convert a filename to a perl class. fn2class <filename>
sub seeclass  # display the perl class is a file. seeclass <filename>
sub showpack  # same as seeclass (both exist for compat) 
sub seepack  # show the package line a perl package
sub seeterp  # show the interpreter line of a file
sub showmethod  # see a particular perl method in file seemethod -f <filename> -m <method> -i [interior only]
sub crlf  # strip crlf from dos files. 
sub addcrlf  # append crlfs to a file (never used, does it work?)
sub asciify  # strip UTF-8 garbage from web pastes
sub gettime  # get the current time params properly formatted
sub getuser  #  the current working user
sub hostonly  # the current hostname  (corrects for often broken "hostname" commands
sub gethostname  # maybe everything in the iso proggies should be in a lib? 
sub bashexpand  # ? incomplete ?
sub starexpand  # expand trailing * into a fully qualified array of files
sub singlequote  # normalize quotes on specified fields of an Pdt::L Object to be used to write an rc file.
sub sourceenv  # same as bash "source" but for perl
sub sourceprefix  # same as above, but exclude all but a specified prefix
sub envbyprefix   # grep environment variables by leading string 
sub rtfn  # return a fully qualified unique time stamped temp file name. Expire it automatically. 
sub doeach  # take a stack of lines, run each in an indevidual shell. (avoids ENV breakage)
sub stackbash  # source one or multiple bash files and run the last one in the argument chain
sub versioninplace    # inserts a dated version line in a file (waiting refactor) 
sub copyrightinplace  # inserts a copyright line in a file (waiting refactor) 
sub dohelp  # prints this file

### CONFIGURABLES 

rtfn  # random temp file name (entropy: 1/1m per second) 

	rtfn prints fully qualified filenames which are 
	reliably unique for the purposes of generating 
	tmp files. rtfn also expires (deletes) old temp 
	files automatically. 

	there are two environment variables that can be 
	defined to change rtfn behavior: 

	XTMPATH 
	
	defines the location of your temp file 
	directory. ($HOME/.tmp is the default) If for 
	example you want your files to be place 
	in ~/Project/.tmp you can add the following to 
	your .bashrc or .xinitrc file: 

	export XTMPPATH=$HOME/Project/.tmp

	XTMPEXPIRE

	defines the length of time (in seconds) to 
	wait before expiring old temp files. This 
	value is set to one hour (3600) by default. 
	So for example if you wanted to make the 
	expire interval two hours you can add the 
	following to your .bashrc file: 

	export XTMPEXPIRE=7200


