
### Bash::Sugar README

Bash::Sugar is a collection of many utilities for file and 
environment management. Many of these are generic sysadmin 
utilities, but some are specific to managing perl projects. 
They can be devided up into formats, stream editors, file 
interrogators and file management tools. 

What is different about Bash::Sugar, is that the programs 
are implemented both as perl methods, and as commands via 
simple symbolic links. This allows identical functionality 
when writing tools in perl, or in bash. 

Much of the impetus for these tools is the difficulty 
inherent to nesting data structures and escapes in Bash. 
By using the tools in Bash::Sugar you can quickly generate 
highly dynamic jobs in one command line statement. 

Implementation of dual language functionality is by using 
symlinks to the included program "sugar", which acts as 
a method accessor. A symlink to the program "sugar" that 
is named the same as a method, will function by running 
the appropriate method. This is similar to how "busybox" 
works. 

### EXAMPLE: 

Bash::Sugar has a few date formats based on ISO-8601
which are particularly useful for backups because 
they are self sorting. 

These are: 
"isodate", "isotime" and "isothu" (time host user).  

These exist both as perl methods: 

#! /bin/perl
my $S = Bash::Sugar->new()  ; 
print ("foo.", $S->isodate()) ; # prints foo.2022-12-31

And as shell commands: 

#!/bin/bash 
echo "foo\.`isodate`" # prints foo.2022-12-31

This works because the installer did this:  

/bin/ln -s sugar isodate 

### HOW TO USE 

A solid knowledge of the perl regexp bestiary (which is much 
better than the standard bestiaries in most programs) can 
allow you to achieve some interesting things. Included is 
the program 'pexp', which functions like sed -E. It just 
takes a regexp on the CLI and you can stack it as many 
times as you want: 

# example: 

ls | pexp 's/^/seeterp /' | doeach | grep perl | chterp '/usr/local/perl'

translation: 
for every file check the interpreter line, if it is perl change the 
intepreter line to the locally installed perl 

Many such small tools like this exist for moving files around different 
distros. This is useful for (at the moment) perl script, perl packages, 
bash and generic string match/replace replacement (chstring) 
  
# tmp files: 

Tmp files are still useful in the bash environment because we are 
often working with non-threading processes. Because of this 
we can use the program "rtfn", to generate a random temp file name.  
"rtfn" cleans up after itself autoexpiring old tmp files. This is 
handy for using with macros in external programs (see the .vimrc 
in FvwmSkel) as we can copy out and copy in data cleanly. 

### LIST OF METHODS (most are also programs)

new  # constructor
dobyexename  # map symlinks to methods 
sugar  # the sugar executable runs help if used directly 
isodate  # iso8601 date
isotime  # iso8601 date_time
isothu  # iso8601 date_time_host_user (when, where, who) 
pexp  # simple perl stream editor like sed -E
capstr  # capitalize the first character an argument, or all on STDIN 
shoutstr  # capitalize the the whole argument, or all on STDIN
whisperstr  # lowercase a whole argument, or all on STDIN
chstring  # match & replace in file: chstring <filename> <oldstring> <newstring>
chterp  # change the interpreter line: chterp <filename> <pathtoexecutable> (don't include shabang)
chpack  # change the "package" line: chpack <filename> <newpackname>
class2fn  # convert a perl class to a filename. class2fn <Foo::Bar>
class2bfn  # convert a perl class to hidden backup filename. class2bfn <Foo::Bar> 
pack2fn  # convert a package line to a filename. pack2fn <package line>
fn2pack  # convert a filename to a perl package line. fn2pack <filename>
fn2class  # convert a filename to a perl class. fn2class <filename>
seeclass  # display the perl class is a file. seeclass <filename>
showpack  # same as seeclass (both exist for compat) 
seepack  # show the package line a perl package
seeterp  # show the interpreter line of a file
showmethod  # see a particular perl method in file seemethod -f <filename> -m <method> -i [interior only]
crlf  # strip crlf from dos files. 
addcrlf  # append crlfs to a file (never used, does it work?)
asciify  # strip UTF-8 garbage from web pastes
gettime  # get the current time params properly formatted
getuser  #  the current working user
hostonly  # the current hostname  (corrects for often broken "hostname" commands)
gethostname  # maybe everything in the iso proggies should be in a lib? 
bashexpand  # ? incomplete ?
starexpand  # expand trailing * into a fully qualified array of files
singlequote  # normalize quotes on specified fields of an Pdt::L Object to be used to write an rc file.
sourceenv  # same as bash "source" but for perl
sourceprefix  # same as above, but exclude all but a specified prefix
envbyprefix   # grep environment variables by leading string 
rtfn  # return a fully qualified unique time stamped temp file name. Expire it automatically. 
doeach  # take a stack of lines, run each in an indevidual shell. (avoids ENV breakage)
stackbash  # source one or multiple bash files and run the last one in the argument chain
versioninplace    # inserts a dated version line in a file (waiting refactor) 
copyrightinplace  # inserts a copyright line in a file (waiting refactor) 
dohelp  # prints this file

### CONFIGURABLES 

The random temp file name tool is quite helpful in macros where 
files need to be edited before importation, such as in templating 
systems like Perl Development Templates. 

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


