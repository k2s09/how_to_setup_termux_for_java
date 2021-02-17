# How to Setup Termux for Java

## Why does this exist?
This exists because doing the aforementioned task is very confusing and does not have a lot of resources on it. 

## Steps:
* Step 1: Download Termux. Links from [Apkpure](https://m.apkpure.com/termux/com.termux), [F-Droid](https://f-droid.org/en/packages/com.termux/) and [Google Play Store](https://play.google.com/store/apps/details?id=com.termux&hl=en_IN&gl=US). There are assistant apps for it like [Termux:Styles](https://f-droid.org/en/packages/com.termux.styling) and [Tasker](https://f-droid.org/en/packages/com.termux.tasker), etc. If you want to use them, just remember to download all of them from the same source.

* Step 2: Download the compiler ECJ (Eclipse compiler for java) by doing `pkg install ecj`. Other JDK's are discontinued.

* Step 3: Download the virtual machine called Dalvikvm by doing `pkg install dx`.

* Step 4: Get a text editor. The easiest one to use is Nano. It's already installed. You can use it by doing `nano <filename.fileextension>`. A new window appears. You can type there, save by doing `ctrkey + s` and exit by `ctrlkey + x`. If you are familiar with vim, you can install it by `pkg install vim`

* Step 5: To compile, do `ecj <filename>.java`

* Step 6: To execute our program, we need to make a dex file for the compiled class file by doing `dx --dex --output=<filename>.dex <filename>.class`. (This is automatically done by the JDK's on PC)

* Step 7: To run that dex file, do `dalvikvm -cp <filename>.dex <filename>`.

Now you might see that these are a lot of commands to run. That is why I made a script to make this process a lot simpler. Keep in mind it only works if the name of your .java file is the same as your class. 

### Script
Goto your bin folder by doing `cd /data/data/com.termux/filer/usr/bin`. There make a script called java by doing `nano java`. In that file, paste the following: 

```
#!/data/data/com.termux/files/usr/bin/bash

option="${1}"

case ${option} in
        -r) CFILE="${2}"
                ecj $CFILE.java
                dx --dex --output=hello.dex $CFILE.class
                dalvikvm -cp hello.dex $CFILE
                rm hello.dex
                rm -r oat
                ;;
        *) echo "`basename${0}`:Usage: java -r Classname"
                ;;
esac
```

Save and close the file, and do `chmod +x java` and come back to your root directory by doing `cd`. Now let's say you had a file `firstProgram.java` which had the class `firstProgram` **(the names need to be the same)**. Do `java -r <filename>` **(without .java)**, and the file should execute. 

* Step 8: This is an optional step. You can use git in your workflow (super useful) by doing `pkg install git` and then using all the git commands. I had cloned and synced my java project files on termux and my pc. [A good git tutorial](https://www.tutorialspoint.com/git/index.htm). If you decide on using vim, [here is my ~/.vimrc](https://github.com/k2s09/dotfiles/blob/main/.vimrc).

## Troubleshooting
Q. Syntax Highlighting in Nano for java files is not working  
A. Nano has a configuration file which you can open by doing `nano ~/.nanorc`. In that file, paste the following line: `include /usr/share/nano/java.nanorc`. Save and close, and the highlighting should work.  
  
  
Q. Running script does not give output  
A. It could be because of one of three things:   
* The java class and file have different names. Sorry, they must be the same.  
* The class didn't compile because of errors in your code. The errors should show up when you run the command. Go back in the file and fix them.  
* You used the script in the wrong way. It is supposed to be `java -r <filename>`. You don't have to include .java or .class in the filename. Just  the literal name.
