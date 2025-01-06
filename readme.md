# About scripts

## listener
listener is a bash script used for running a web server on a specified ports. The use of a web server eliminates the false connection refuse error caused by net cat utility, and its automated


### Requirements

This script is dependent on the followings:

    * Linux OS
    * Docker
    * net-tools

### Usage

Here are the steps needed to use the script
  1. Run the command **listener [options] setup-server** to setup server (one time setup, to setup the Nginx server)
  2. Specify all target ports in the **./comp/ports.txt** file, each port on a new line
  3. To start port listening, run the command  **listener [options] start**
  4. To stop port listening, run the command **listener [options] stop**
  5. Where the **listener [options] stop** command fails to stop the active runing instance of the listener, run the command **listener [options] stop-all**
  6. To check the status of the listener, run the command **listener [options] status**
  7. To view the applications making use of the none free ports, run the command **listener [options] show-used-ports**


**WHERE**

_options_: An optional flag to pass to the listener to change the way it behaves. Here are the available options:
 - **-a** : Tells the listener to attempt opening all ports, without checking if a ports is free or not
 - **-s** : Tells the listener to skip dependencies check.


**Note**

The script supports global access, so you may register the path to the script and call the listener script from any directory

## simpleListener
This script is used to create a light weight service that listens on the ports specified in a text file. This script is needed in an environment where there is no docker

### Usage
The syntax for the script usage is shown below

**./simpleListener -p portsFile**

**WHERE**

- **-p**  : A reqired parameter, which specifies the file (new line sperated ports) that has the list of the ports the light weight service should listen on