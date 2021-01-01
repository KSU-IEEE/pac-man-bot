# pac-man-bot
An integration repo of everything needed for the 2021 SouthEastcon hardware competition.  
This is meant to be able to be used to test, compile, and run the code, both on the robot for deployment, as well as on test computers.  

## Installation process  
Do the following, to install this repo, it's submodules, and have a full development environment **NOTE**: you will need an internet connection for the steps in this section:  

1. Setup Keys:    
If you do not have ssh key setup, follow [these instruction](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh)  

2. Clone repos:  
```BASH  
git clone git@github.com:KSU-IEEE/pac-man-bot.git  
cd /path/to/pac-man-bot  
git pull
git submodule update --init --recursive  
```  

3. Install the dependencies:  
```BASH  
cd /path/to/pac-man-bot 
cd ./accessibility/python  
python3 dependencies.py -a  
``` 
**NOTE**: this step is only meant to be ran on the first time using this pacakge. After the first install, you will have these dependencies on your machine. So if you delete the repo, and reclone (steps 1 and 2) then you will not need to rerun this again.  

## Compiling 
To compile this code, do the following:  
```BASH  
cd /path/to/pac-man-bot  
mkdir build && cd build  
cmake .. -GNinja  
ninja  
```  

## Running test code  
There are test nodelet written to test your environment and ensure you can run a ros launch file. To test this, do the following:  
```BASH  
srcros # this is an alias added during compile time  
roslaunch behaviors test_behaviors.launch 
```  
This can be ran from any directory, upon using the following command roscore will launch and there will be a series of outputs showing 2 nodes running.  
