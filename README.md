# How to setup Termux for Java

## Why does this exist?
Because doing this is a complicated and confusing process with not a lot of resources.

# Steps:
1) Install termux (duh). It is only on android. [Apkpure](https://m.apkpure.com/termux/com.termux), [F-Droid](https://f-droid.org/en/packages/com.termux/) and [Google Play Store](https://play.google.com/store/apps/details?id=com.termux&hl=en_IN&gl=US). There are assistant apps for it to add functionality like [Styling](https://f-droid.org/en/packages/com.termux.styling) and [Tasker](https://f-droid.org/en/packages/com.termux.tasker). If you wish to install them, be sure to download them from the same source as you used for Termux (the hash keys need to match)

2) Now we need to download a JDK to compile our code. Unfortunately, open-jdk was discontinued because of excessive heating issues. So we will use ECJ (Eclipse compiler for Java) and Dalvik VM, a virtual machine to run code. Run `pkg install ecj dx` in your terminal. It will take a little bit of time and install itself.

3) Alright now we need a text editor to write code. If you are not familiar with any terminal text editors, use Nano. It will be already installed but you can update it by `pkg install nano`. To open a file you do `nano <filename.fileExtension>` and start writing. To save, you do `ctrl key + s` and to exit you do `ctrl key + x`. By now you can write a ceremonial hello world program in java. If syntax highlighting is not already enabled, you can go in nano's configuration file called nanorc by running `nano ~/.nanorc` and write the line `include /usr/share/nano/java.nanorc`. It's not to the level of IDE's but it's something. Otherwise if you're familiar, you can install vim by `pkg install vim`

4) Now we need to compile and execute our code. It is HIGHLY recommended to have java file same name as class. You will shortly see why. 

To compile, do `ecj <filename>.java`. Now a .class file will appear next to the .java file in the directory. Now we need to make a .dex file for it which is run by the virtual machine. You would have never done this before because JDK on PCs automatically does this. But here this is another step to do. Anyways, we have to do `dx --dex --output=<filename>.dex <filename>.class`. Now a .dex file will appear in the same directory. Now to run that file, we do `dalvikvm -cp <filename>.dex <filename>`

Now that's a lot to do. If you have different names for class and file, it's even harder to do. But don't loose hope. I made a bash script which makes all of this a lot simpler.

Run `cd /data/data/com.termux/files/usr/bin`. Then do `nano java` to make a script. Paste this: 
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
                rm $CFILE.class
                ;;
        *) echo "`basename${0}`:Usage: java -r Classname"
                ;;
esac
```
Leave that directory by doing `cd`. Now when you make .java file called hw.java, write code in it using nano/vim and save it. You do `java -r <filename>` and don't give any file extension. The script will compile it, make a dex file, run it, delete the dex file and other nonsense it makes to not clutter your directory and call it a day!

5) That's basically it. But I recommend you do some more things to make your life simpler, like using git. Using git allows you to get the project files you made on your pc and access them from your phone, edit them and sync them! This isn't a tutorial for git, however. [A good git tutorial](https://www.tutorialspoint.com/git/index.htm). You can install it on termux by `pkg install git`. 
