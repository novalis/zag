
This is Zag, an implementation of glulx v2.0.0 for Java

© 2003-2004, Jon Zeppieri (see license/README-license.txt for details)
[jalfred97 @ yahoo . com]
-------------------------------------------------------

Contents:
1. Introduction
2. Requirements
3. Contents of package
4. Using Zag as a Java Web Start application
5. Using Zag as a normal Java application
6. Glk
7. Features missing and present
8. Bugs

-------------------------------------------------------


1. Introduction:

Zag implements v2.0.0 of the glulx standard[1].  It is (to my
knowledge) fully compliant with the specification, and, moreover, it
is a feature complete implementation.  Even SONG music is now supported.

Zag was developed under Mac OS 10.2.6 - 10.3.4 and has been tested
(though not enough) on that platform as well as on Linux (2.4.20
kernel) and Windows XP (very briefly).

[1] http://www.eblong.com/zarf/glulx/index.html


2. Requirements

Zag requires the JRE (Java Runtime Environment) version 1.4 or later,
or the JDK (Java Development Kit) version 1.4 or later.  To run it as
a Java Web Start application (it cannot be run as an applet), you must
have Java Web Start, which basically means having a recent version of
the the Java Plugin for your web browser.  (If you use Mac OS X, you
already have this.)


3. Contents

This package comes with a few things:

a. Complete source code, under the "org," "micromod", and "com"
directories.  Please note the licensing differences between the three,
dicussed in the file license/README-license.txt.

b. Complete binary code, under the "bin" directory.

c. "Complete" API documentation, under the "doc" directory.  There are
quotation marks around "complete," because my code is really
undocumented.  You can browse the APIs, but unfortunately they are not
explained.

d. A signed jar file, containing all the binary code.  The jar file
bears my digital signature, the authenticity of which is certified by
Thawte (an internationally trusted Certificate Authority).  I have
signed the jar so as to make it easier to use the program with Java
Web Start, but see section 4 for details.

e. License files for both my code and for others' (the mod player and
PNG decoder), in the directory "license."

f. A "build.xml" file, for building the program from source, using
ant.

e. A "zag.jnlp" file, which can be used as a prototype for making
.jnlp files, in order to use Zag as a Java Web Start application.
(But see the next section.)


4. Using Zag as a Java Web Start application

Zag cannot be run as an applet.  Applets can only access data by means
of streams; they have no method of randomly accessing data (except
for the contents of dynamic memory, of course).  This restriction
makes handling blorb files (which contain graphic and sound resources)
impracticable.

So, the solution is to use Zag as a Web Start application.  Now, since
Zag demands access to your file system, there are two possibilities.
Either Zag could use the special, restricted IO APIs designed for Web
Start applications, or it could use the normal APIs but require a
digital signature in order to run.

I chose the latter option.  Had I chosen the former, it would have
been difficult to run the program as a normal application, as well,
and it was always my intention to allow both modes of operation.

The jar file that comes with this package is signed by me, and the
signature's validity is asserted by Thawte, a trusted certificate
authority.  In other words, Thawte offers assurance that the code was
signed by the party who claims to have signed it and that the code has
not been in any way tampered with since it was signed.

To use a Java Web Start (JWS) application, the user must have JWS
installed.  Generally, this means having a recent version of the Java
Plugin for one's web browser.  If you use Windows, Solaris, or Linux,
the Java Plugin comes with the JDK and JRE, either of which you can
download from Sun [http://java.sun.com/].

A JWS application is launched when the user downloads a .jnlp file,
which is an XML file of a particular sort.  An example is provided in
this package; see the file "zag.jnlp."  When JWS runs, it will
download the jar file and note that it wants unresticted access to the
system and network.  It will ask the user if he wants to give the
program such access.  If the user grants access, Zag will run.


5. Using Zag as a normal Java application

Zag can be launched with the command:

   java -jar zag-[version].jar

For instance, if you are running the 1.06 version:

   java -jar zag-1.06.jar

You may optionally supply as an argument the path to a valid glulx or
blorb file, or you may supply a fully qualified URL (don't leave out
the protocol portion, e.g, http://, or ftp://, or file:///).  Zag will
attempt to execute the specified file when it launches.


6. Glk

Since Zag implements Glulx (and does not implement it trivially), it
supports the glk opcode, which, in turn, means that it comes with
something like an implementation of Glk.  This is Zing, which,
unfortunately, stands for "Zing is not Glk."  I detest recursive
acronyms, but this was the very first thing that came to mind, and it
stuck.  At any rate, the name is accurate, since, in a number of ways,
Zing really is *not* Glk.  For one thing, it makes no attempt
whatsoever to keep holy the full 32-bit address range of Glk.  Java
does not have unsigned integers, and it is fantastically annoying to
implement them usefully.  (You cannot, for instance, index an array
with a Java long integer.)  Additionally, Zing does not implement the
Glk dispatch system, since Java already supports interface discovery
through the reflection API.  (I did, however, need to support the
notion of "in parameters" and "out parameters," but I did this with
extra type information.)

The org.p2c2e.zing.Glk class provides a set of static methods that map
directly onto the set of glk_ functions defined in the Glk spec.
Originally, I had intended this to be nothing more than a
compatibility layer on top of a fully object-oriented library.  (The
idea being that Java programs would use zing directly, without going
through the Glk layer.)  This ended up being rather messier than
intended and could probably stand to be cleaned up quite a bit.

Note that Zing does not support the interfaces proposed by Matthew
Russotto; it is not quite so faithful to the C API.

Zing, of course, comes with Zag, and is under the exact same license
(BSD), so you can use it for whatever purposes you desire.


7. Features missing and present

a. Missing: cut, copy and paste.  

The most notable feature lacking in Zag (if you do not count speed) is
the ability to select, cut, and paste text.  Zag (or rather Zing, the
Glk-alike library used by Zag) does not use the standard Java text
widgets for its story windows, so it does not get this ability "for
free," as it were.  (I tried, long ago, to use the javax.swing.text
package for this purpose and failed miserably.  Actually, I concluded
that it was possible, but that I would end up writing even more code
than if I worked from scratch.)  For my own part, I almost never use
cut and paste while playing interactive fiction, so this doesn't
bother me, but I can imagine it being annoying to others.  I apologize
for this.  Maybe some day I'll implement it, though probably not.  If
someone else wants to, however, I will be happy to incorporate his or
her patches back into the main code tree.

b. Present:  command history

Athough it lacks cut & paste, Zag does have a command history buffer
for story windows (a separate history for each such window).  Use the
up and down arrow keys to scroll backwards and forwards through the
history.  Note that this feature is only present on story windows, not
on "status," or grid, windows.

c: Present:  ability to load a file from a URL

Under the File menu, there is an option, "Load URL..."  If you select
this, a dialog will pop up prompting you to enter a URL.  You may, for
instance, enter the URL of a game file stored on the IF archive.  The
file will be downloaded and then opened.  The downloaded file will be
placed in temporary storage (e.g., under UNIX, it will be in the /tmp
directory; perhaps this should be configurable).  Please note that you
must enter a fully qualified URL, including the protocol string (e.g.,
http://).

Many files on the archive, however, are not directly readable by Zag.
Zag understands glulx (.ulx) files and blorb (.blb) files, but it will
not delve into a .zip file to extract either of these.  This feature
may be added in the future, if there is sufficient demand for it.

d. Present:  emacs key bindings (not all of them, of course)

In buffer (story) windows, Ctrl-a will position the cursor at the
start of the input.  Ctrl-e will position it at the end.
Ctrl-right-arrow will move the cursor forward one word;
Ctrl-left-arrow will move it back one word.  Meta-backspace will erase
the previous word.  (On a Mac, "Meta" is the Command, or Apple, key.
On Windows, it is the Alt key.)


8. Bugs

All software has bugs, and Zag is no different.  Here I will describe
known issues with the program.  Surely there are other issues I do not
know about-- likely many of them.  *Please* report these.  Even
better, fix them and send me a patch.

a. WindowMask (from the WinGlulxe configration file specification) is
implemented using an opaque, black background where it ought to be
transparent.  Java isn't up to the task yet.
