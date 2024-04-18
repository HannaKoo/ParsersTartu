# Installing ConlluEditor on Windows

Tested on PowerShell, should work on cmd.com or Windows Terminal.

## Get a Java JRE

(Avoid Oracle, get another OpenJDK based version instead  
https://whichjdk.com/ )

(JRE == *Java Runtime Environment*)  
(A JDK (*Java Development Kit*) also includes a JRE, but is bigger and not needed for running ConlluEditor.)

ConlluEditor requirements says:  

> Java jre >= 11.0 (including 17)  

https://github.com/Orange-OpenSource/conllueditor

It lists also other requirements, but they are included in the installation zip.

I'd guess newer versions (eg. 21) also work, but have not tried.

### Install the Java JRE

### Check if you have Java already, if you do, then you don't have to do anything
### Are you unsure if you have it or not?

Open powershell and write `java -version`

**Either** download the installation package from:  
https://adoptium.net/temurin/releases/?os=windows&package=jre&version=17&arch=x64  
(The .msi is the install package with a wizard.)

**Or** alternatively use the command line:  
`winget install EclipseAdoptium.Temurin.17.JRE`

## Check that your Java works by typing in PowerShell

If you had PowerShell open, close and restart it

`java -version`

## Install ConlluEditor

Download the latest version (`conllueditor-2.25.3.zip` today) from: https://github.com/Orange-OpenSource/conllueditor/releases

To install, simply unzip to some path, eg. `C:\Program Files`. If you have a space in the path, you have to be careful to start it without one particular backslash '`\`' in the command. If you don't want to worry about that, use a directory without a space, eg. `C:\ProgramData` (Is this a hidden system folder?), or `C:\`.

Also, if you want to make a desktop shortcut (see the end of this file), use a path without spaces. (But it's easy to move it afterwards, it's only one directory, so no stress.)

## Use ConlluEditor

1. Using Windows Explorer navigate to the location with the conllu file.
1. Open a shell in the folder where the conllu file is:
   - Windows 10: Click: *File-menu* -> *Open Windows PowerShell*
   - Windows 11: Right-click on the folder, or an empty background in the folder, and select (*Open in Terminal*?)
1. Paste this command to the command line, and **replace the conllu file name** in it (tab completion works!).
   - Also replace the installation directory (2 places!) if different.

**If** installed to `C:\Program Files`:  
```PowerShell
java -jar "C:\Program Files\conllueditor-2.25.3\target\ConlluEditor-2.25.3-jar-with-dependencies.jar" --rootdir "C:\Program Files\conllueditor-2.25.3\gui" output.conllu 8888
```
(If you use tab completion for the `--rootdir` directory name, it adds a `\` after the `....\gui`, and then it does not work. https://github.com/Orange-OpenSource/conllueditor/issues/39 )  

**or**, if installed to `C:\ProgramData`:  
```PowerShell
java -jar C:\ProgramData\conllueditor-2.25.3\target\ConlluEditor-2.25.3-jar-with-dependencies.jar --rootdir C:\ProgramData\conllueditor-2.25.3\gui\ output.conllu 8888
```  
**or**, if installed to `C:\`:  
```PowerShell
java -jar C:\conllueditor-2.25.3\target\ConlluEditor-2.25.3-jar-with-dependencies.jar --rootdir C:\conllueditor-2.25.3\gui\ output.conllu 8888
```  
(With these others it works with or without the `\` after `....\gui`)  

You might have to allow it through the firewall.

Point your web browser to the address it tells you to. (It's not connecting to web, but actually your own machine.)

Click eg. the arrow buttons to load a sentence.

Press Ctrl+C in the PowerShell/terminal to quit the server? If you only close the browser tab, the server is left running.

## Update Java and Conllueditor

### Update Java

Download a new install package(?). 

Or run  
`winget upgrade EclipseAdoptium.Temurin.17.JRE`  
(?)

( `winget list` shows all installed packages on the system, `winget upgrade --all` updates everything it can (also some programs installed from other places), to the latest version )

At some point Java 17 will be end-of-life. Then see what Conllueditor requirements say, or just try a newer Java. A new main version (eg. 21) will probably install alongside the old 17. Remember to uninstall the old version.

https://adoptium.net/support/ :
> Java 17 (LTS) | ... | At least Oct 2027

### Update Conllueditor

Download the .zip for the new version, and unzip it like before. Remember to replace version number to the startup command line (two or three places in the line).  
```PowerShell
java -jar 'C:\ProgramData\conllueditor-2.25.3\target\ConlluEditor-2.25.3-jar-with-dependencies.jar' --rootdir 'C:\ProgramData\conllueditor-2.25.3\gui\' output.conllu 8888
```  
When you have verified that the new version works, you can delete the old installation directory (or folder in Windows speak).


### Making a shortcut for graphical start

TODO, probably needs a small .bat script or something.

https://superuser.com/questions/106313/pass-dragged-filename-to-windows-shortcut

To use: Drag and drop the conllu file on the shortcut.

If you always want to edit the **same conllu file**, and your installation directory and conllu-file name have **no spaces**, it's easy to make a desktop shortcut:
1. Right click on Desktop
1. *New* -> *Shortcut*
1. Target: `C:\Windows\System32\cmd.exe /k "java -jar C:\ProgramData\conllueditor-2.25.3\target\ConlluEditor-2.25.3-jar-with-dependencies.jar --rootdir C:\ProgramData\conllueditor-2.25.3\gui\ output.conllu 8888"` (fix the installation directory (2 places), version (3 places) and filename to read, here `output.conllu`.)
1. Working Folder: The location of the file, eg. `C:\Users\jere\Downloads`

