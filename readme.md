# About script

Portlistener is a bash script used for running a web server on a specified ports. The use of a web server eliminates the false connection refuse error caused by net cat utility, and its automated


# Requirements

This script is dependent on the followings:

    * Linux OS
    * Docker
    * net-tools

# Usage

Here are the steps needed to use the script

    1 Run the command **listener server-setup** to setup server (one time setup, to setup the Nginx server)
    2 Specify all target ports in the **./comp/ports.txt** file, each port on a new line
    3 To start port listening, run the command  **listener start**
    4 To stop port listening, run the command **listener stop**

# Note

The script supports global access, so you may register the path to the script and call the listener script from any directory