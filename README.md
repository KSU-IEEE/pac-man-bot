# pac-man-bot
An integration repo of everything needed for the 2021 SouthEastcon hardware competition.  
This is meant to be able to be used to test, compile, and run the code, both on the robot for deployment, as well as on test computers.  

## Using this on a Windows Machine  
This code is meant to be ran and compiled on a linux machine (Ubuntu 18.04 to be specific). Because of that, it is highly recommended to not set this up on a windows machine.  

Rather, setup the Windows subsystem for Linux, by following [these instructions](https://www.windowscentral.com/install-windows-subsystem-linux-windows-10). WSL acts as a light weight VM, and is able to to use applications you have already installed onto windows (such as using the correct test editor: VSCode). 

If you already have an ubuntu VM setup, you can use that, but keep in mind that WSL takes a lot less memory to run.

## Installation process  
Do the following, to install this repo, it's submodules, and have a full development environment **NOTE**: you will need an internet connection for the steps in this section:  

1. Setup onetime sign in for git https pushing:  
By default, https forces users to type in their username & password each time they ammend something on the server. I hate this. So, to get around it, do the following:  
```BASH
$ git config credential.helper store 
```
The first time you push, you will have to enter your username and password, but everytime after that you won't have too.

**NOTE:** this puts your git information visibily in your home directory (`~/.git-credential` becomes initialized after you first enter your credentials). So be sure that you don't put this on a computer you don't trust.

2. Clone repos:  
```BASH  
$ git clone https://github.com/KSU-IEEE/pac-man-bot.git
$ cd /path/to/pac-man-bot  
$ git pull
$ git submodule update --init --recursive  
```  

3. Install the dependencies:  
```BASH  
$ cd /path/to/pac-man-bot 
$ cd ./accessibility/python  
$ python3 dependencies.py -a  
``` 
**NOTE**: this step is only meant to be ran on the first time using this pacakge. After the first install, you will have these dependencies on your machine. So if you delete the repo, and reclone (steps 1 and 2) then you will not need to rerun this again.  

## Compiling 
To compile this code, do the following:  
```BASH  
$ cd /path/to/pac-man-bot  
$ mkdir build && cd build  
$ cmake .. -GNinja  
$ ninja  
```  

## Running test code  
There are test nodelets written to test your environment and ensure you can run a ros launch file. To test this, do the following:  

If you are running in the same terminal you compiled in for the first time, run `source ~/.bashrc`, if not continue onto the next step (~/.bashrc gets source automatically when opening a new terminal)  

Then do the following:
```BASH  
$ srcros # this is an alias added during compile time  
$ roslaunch behaviors test_behaviors.launch 
```  
This can be ran from any directory, upon using the following command roscore will launch and there will be a series of outputs showing 2 nodes running.  
**NOTE:** The last command starts roscore. This process will keeping going until you cancel it with `ctrl-C`.  