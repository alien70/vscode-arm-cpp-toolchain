# Set Up C++ ARM Development Toolchain with Visual Studio Code

**`<TLDR></TLDR>`** TThe final goal of this series of notes is to simulate remote debugging of **`Microsoft Visual Studio 2017`** &copy; on a Linux target computer (ie a tiny ARM computer, such as a **`Raspberry Pi`** &copy; or a **`FriendlyARM NanoPi M1 Plus`** &copy;), but using **`Microsoft Visual Studio Code`** &copy;. First of all, let's see what we need to achieve the desired goal. 

## Index

- [How Visual Studio 2017 does](#how-visual-studio-2017-does)
    - [Debug mode](#debug-mode)
    - [Release mode](#release-mode)
    - [Let's sum up](#let's-sum-up)
- [Source copy](#sources-copy)

## How Visual Studio 2017 does

Following the great **`Scott Hanselman`**'s article [Writing and debugging Linux C++ applications from Visual Studio using the "Windows Subsystem for Linux"](https://www.hanselman.com/blog/WritingAndDebuggingLinuxCApplicationsFromVisualStudioUsingTheWindowsSubsystemForLinux.aspx), let's try to create a *`Cross Platform Linux Console Application`*  project with Visual Studio 2017.

![Figure 1 - New Project](./images/vs-2017-new-prj.png)

The next step is to configure the IDE to connect *via ssh*, to the remote Linux client. From the *`Tools->Options...`* menu, search for the *`Cross Platform->Connection Manager`* item,

![Figure 2 - Connection Manager](./images/vs-2017-conn-mgr.png)

and click on the **`Add`** button

![Figure 3 - Connect to Remote](./images/vs-2017-conn-mgr-conn.png)

and fill it with the ssh coordinates of the target client. Now we're ready, let's build our simple project and see what appens.

![Figure 4 - Build Output](/images/vs-2017-build-output.png)

### Debug mode

The first time we'll compile our project in *`Debug Mode`*, set a breakpoint in the source file and start a debug session. As shown in the following images the debugger stops correctly at the breakpoint, 

![Figure 5 - Breakpoint](./images/vs-2017-main-cpp.png)

and the output is

![Figure 6 - Linux Console](./images/vs-2017-linux-console-window.png)

### Release mode

For sake of completness, let's compile our project also in *`Release Mode`*

### Let's sum up

Reading the build output shown in Figure 4, the steps taken by the *`full fledged IDE`* are:

1. *Validating sources*;
2. *Copying sources remotely to '192.168.0.101'*;
3. *Validating architecture*;
4. *Starting remote build*;
5. *Compiling sources: main.cpp*;

Omitting the first step, the following are the ones that we will try to reproduce with **`Visual Studio Code`** &copy;

## Cross Compile

First thing first we have to compile our project for the target architecure, in our case ARM, on our development pc.

## Sources copy

So the thing VS2017 does is copying all the necessary files to the remote target. Taking a close look to what has been copied, we have:

```
└── projects
    └── ConsoleApplication1
        ├── bin
        │   └── ARM
        │       ├── Debug
        │       │   └── ConsoleApplication1.out
        │       └── Release
        │           └── ConsoleApplication1.out
        ├── main.cpp
        └── obj
            └── ARM
                ├── Debug
                │   └── main.o
                └── Release
                    └── main.o

10 directories, 5 files
pi@NanoPi-M1-Plus:~$
```

