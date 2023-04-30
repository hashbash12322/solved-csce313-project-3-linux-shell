Download Link: https://assignmentchef.com/product/solved-csce313-project-3-linux-shell
<br>
Most useful interaction with a UNIX/Linux-based system occurs through the shell. Using a series of easy to remember and simple commands, one can navigate the UNIX file system and issue commands to perform a wide variety of tasks. Even though it may appear simple, the shell encapsulates many significant components of the operating system.

<h1>Basic Shell Features</h1>

<h2>Environment</h2>

The shell maintains many variables which allow the user to maintain settings and easily navigate the filesystem. Two of these that are particularly important are the current working directory and the PATH. As its name implies, the current working directory variable keeps track of the user’s current directory. The PATH variable consists of string of colon separated pathnames. Whenever you type a name of a command, the kernel searches in the directories specified by the PATH variable starting with the leftmost directory first. If the executable is not found in any of the specified directories, then the shell returns with an error. One may modify the PATH at any time to add and remove directories to search for executables.

<table width="624">

 <tbody>

  <tr>

   <td width="624">shell:~$ pwd /home/tanzir shell:~$ echo $PATH/usr/local/bin:/usr/bin:/usr/local/games:..</td>

  </tr>

 </tbody>

</table>

Figure 1: Current directory and the PATH enviromental variable.

<h2>Pipelining</h2>

UNIX provides a variety of useful programs for you to use (grep, ls, echo, to name a few). Like instructions in C++, these programs tend to be quite effective at doing one specific thing (Such as grep searching text, ls printing directories, and echo printing text to the console). However, programmers/OS users would like to accomplish large tasks consisting of many individual operations. Doing such requires using results from previous steps in order to complete a larger problem. The UNIX shell supports this through the pipe operation (represented by the character |). A pipe in between two commands causes the standard output of one to be redirected into the standard input of another. An example of this is provided below, using the pipe operation to search for all processes with the name ”bash”.

shell:~$ ps -elf | grep bash | awk ’{print $10}’ | sort

3701

4197

Figure 2: Pipelining multiple commands.

<h2>Input/Output Redirection</h2>

Many times, the output of a program is not intended for immediate human consumption

(if at all). Even if someone isn’t intending to look at the output of your program, it is still immensely helpful to have it print out status/logging messages during execution. If something goes wrong, those messages can be reviewed to help pinpoint bugs. Since it is impractical to have all messages from all system programs print out to a screen to be reviewed at a later date, sending that data to a file as it is printed is desired.

Other times, a program might require an extensive list of input commands. It would be an unnecessary waste of programmer time to have to sit and type them out individually. Instead, pre-written text in a file can be redirected to serve as the input of the program as if it were entered in the terminal window.

In short, the shell implements input redirection by redirecting the standard input of a program to an file opened for reading. Similarly, output redirection is implemented by changing the standard output (and sometimes also standard error) to point to a file opened for writing.

<table width="624">

 <tbody>

  <tr>

   <td width="624">shell:~$ echo ‘‘This text will go to a file’’ &gt; temp.txt shell:~$ cat temp.textThis text will go to a file shell:~$ cat &lt; temp1.txt This text came from a file</td>

  </tr>

 </tbody>

</table>

Figure 3: I/O redirection.

<h1>Assignment</h1>

For this assignment, you are to design a simple shell which implements a subset of the functionality of the Bourne Again Shell (Bash).        The requirements for your shell are as follows:

<ul>

 <li>Continually prompt for textual user input on a command line.</li>

 <li>Parse user input according the provided grammar (see below)</li>

 <li>When a user enters a well formed command, execute it in the same way as a shell. You must use the commands fork and exec to accomplish this. You may NOT use the C++ system() command.</li>

 <li>Allow users to pipe the standard output from one command to the input of another an arbitrary number of times.</li>

 <li>Support input redirection from a file and output redirection to a file.</li>

 <li>Allow users to specify whether the process will run in the background or foreground using an ’&amp;’. Backgrounding processes should not result in the creation of zombie processes. (Commands to run in the foreground do not have an ’&amp;’, and commands that run in the background do)</li>

 <li>Print a custom prompt which supports printing the current directory, user name, current date, and current time.</li>

 <li>(Bonus) Allowing $-sign expansion. See the last sample command in the list of commands for example.</li>

</ul>

Write a report describing your design for piping, redirection and other special techniques that you are unique to your implementation. No need to restate the obvious.

Figure #4: Simple Shell Grammar

valid string = unix command || unix command AMP || special command

unix command = command name ARGS       || unix command REDIRECTION filename ||

unix command PIPE unix command

special command = cd DIRECTORY || exit command name = any valid executable/interpreted file name

AMP = &amp;

ARG = string

ARGS = ARG ARGS || ARG

DIRECTORY = absolute path || relative path

PIPE = |

REDIRECTION = <em>&lt; </em>|| <em>&gt;</em>

Figure #4: Some Sample Commands

<table width="633">

 <tbody>

  <tr>

   <td width="633">echo Here starts our shell …echo −e ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>This message contains a line feed <em>&gt;&gt;&gt;&gt;&gt;</em>
’’ echo ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>Start to exercise pipe <em>&gt;&gt;&gt;&gt;&gt;</em>’’echo ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>IO redirection <em>&gt;&gt;&gt;&gt;&gt;</em>’’ ps <em>&gt; </em>test.txt</td>

  </tr>

 </tbody>

</table>




<table width="633">

 <tbody>

  <tr>

   <td width="633">grep pts <em>&lt; </em>test.txt pwd <em>&gt; </em>pwd.txt mv pwd.txt newpwd.txt cat newpwd.txtecho ‘‘ 1 pipe <em>&gt;&gt;&gt;&gt;&gt;</em>’’ psecho ‘‘                   2 pipes <em>&gt;&gt;&gt;&gt;&gt;</em>’’psps −−aa || awk ’/pts/[0awk ’/pts/[0−−9]9]//{{print $1print $2}}’’ || tailtailecho ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>3 pipes <em>&gt;&gt;&gt;&gt;&gt;</em>’’ls<sub>ls</sub>ls −−−l /proc/sysl /proc/sysl /proc/sys ||| awk ’awk ’awk ’{<sub>{</sub>{print $9print $8$9print $8}<sub>}</sub>’<sub>’ </sub>}’|<sub>|</sub>|headsorthead−<sub>−</sub>−r<sub>3</sub>10|<sub>|</sub>|head<sub>sort</sub>sort −−<sub>r</sub>5echo ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>4 pipes with I/O redirection <em>&gt;&gt;&gt;&gt;&gt;</em>’’awk ’<sub>cat output.txt</sub>ls −l /proc/sys{print $8$9<em>&gt;</em>}’ test.txt<em>&lt; </em>test.txt | head −10 | head −8 | head −7 | sort <em>&gt; </em>output.txtecho ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>Background process (ps after some time to make sure the bg’s are not defunct) <em>&gt;&gt;&gt;&gt;&gt;</em>’’dd if=/dev/zero of=/dev/null bs=1024 count=10485760 &amp;sleep 10 &amp; psecho ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>Directory Operations <em>&gt;&gt;&gt;&gt;&gt;</em>’’ cd /home/ugrads/ pwd cd − pwdecho ‘‘<em>&lt;&lt;&lt;&lt;&lt; </em>Miscellenous <em>&gt;&gt;&gt;&gt;&gt;</em>’’ rm newpwd.txt jobssleep 10echo ‘‘cat /proc/$(ps<em>&lt;&lt;&lt;&lt;&lt;</em>|grep bashBonus ($−|headsign expansion)−1|awk ’{print $1<em>&gt;&gt;&gt;&gt;&gt;</em>}’)/status’’</td>

  </tr>

 </tbody>

</table>