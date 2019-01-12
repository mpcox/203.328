# Compiling Velvet

[Return to the main index](../index.html)
<br><br>

The following text describes how to compile the [Velvet](https://www.ebi.ac.uk/~zerbino/velvet/) assember for your own computer.  That is, how you turn the human readable source code into a program file that your computer can run.

These instructions assume that you have set up a working UNIX system.

> Ubuntu: The commands below should work 'out of the box'<br>
> macOS: You may need to install [Xcode](https://developer.apple.com/xcode/), which is free from the App Store.<br>
> Windows: You may need to install interpreter software like [Cygwin](http://www.cygwin.com), which is also free.

To compile Velvet:

1. Download the Velvet source files, [*velvet_1.2.10.tgz*](../code/velvet_1.2.10.tgz)

2. Decompress the file:
```
tar -xvzf velvet_1.2.10.tgz
```

3. Move into the *velvet_1.2.10* directory:
```
cd velvet_1.2.10
```

4. Compile the program:
```
make 'MAXKMERLENGTH=256'
```

Once these commands complete, you should have two program files called ```velvetg``` and ```velveth``` in your directory.  These will run natively on your computer and you can move them wherever you want to.

<br>
[Return to the main index](../index.html)

