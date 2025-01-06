#!/bin/bash

#Call using the format scriptName -h IP -p portFile [-t telnet | nc]

timeout=7

# _________________Functions definitions starts_____________
function fileExist (){

    file=$1

    if [ ! -f $file ]; then
        echo -e "The file '$file' does not exist, please confirm and try again\n"
        exit 2
    fi

}
# _________________Functions definitions ends_______________

# Check if needed utilities are installed
which telnet > /dev/null 2> /dev/null
ec1=$?

which nc > /dev/null 2> /dev/null
ec2=$?

if [[ $ec1 -eq 1 && $ec2 -eq 1 ]]; then

    echo -e "\nNo Telnet and Netcat utilities installed, try installing one\n"
    exit

elif [[ $ec1 -eq 0 && $ec2 -eq 1 ]]; then

    if [ $tool == "nc" ]; then
        echo -e "\nNo Netcat utility installed, Telnet utility has been set as default\n"
        tool="telnet"
    fi

elif [[ $ec1 -eq 1 && $ec2 -eq 0 ]]; then

    if [ $tool == "telnet" ]; then
        echo -e "\nNo Telnet utility installed, Netcat utility has been set as default\n"
        tool="nc"
    fi

fi

host=""
portFile=""
tool="telnet"
while getopts "h:p:t:" variable; do
case "$variable" in
    h)
        host=$OPTARG
    ;;
    p)
        portFile=$OPTARG
        fileExist $portFile
    ;;
    t)
        tool=$OPTARG
        if [[ $tool != "nc" && $tool != "telnet" ]]; then
            echo -e "Only telnet and nc utility is supported, '$tool' is not supported, and has been defaulted to telnet utility\n"
            tool="telnet"
        fi
    ;;
esac
done

# Check if the host and port file has been specified
if [[ -z $host && -z $portFile ]]; then
    echo -e "Please specify the host and the portFile, using '-h' and '-p' option respectively, and try again\n"
    exit 2
elif [[ ! -z $host && -z $portFile ]]; then
    echo -e "Please specify the portFile, using '-p' option and try again\n"
    exit 2
elif [[ -z $host && ! -z $portFile ]]; then
    echo -e "Please specify the host, using '-h' option and try again\n"
    exit 2
fi

readarray -t ports < $portFile

echo "The ports are being tested with $tool utility"

output="${host}_Ports_test_result.txt"
echo "PORTS CONNECTIVITY TEST  WITH ($host) RESULT" > $output
echo -e "\n" >> $output
echo -e "PORTS\t----------\tSTATE" >> $output

for port in ${ports[@]}; do

    # Parse Data
    trimmed_port=$(echo $port | sed 's/[^0-9]//g')

    echo "Testing connectivity with  $host on port $trimmed_port"

    if [ $tool == "telnet" ]; then
        timeout --foreground $timeout telnet $host $trimmed_port > temp_output
        cat temp_output | grep -i connected > /dev/null
        ec=$?
    elif [ $tool == "nc" ]; then
        nc -w $timeout $host $trimmed_port
        ec=$?
    fi

    if [ $ec -eq 0 ]; then
        echo -e "Exit code: $ec, Connected succesfully\n"
        echo -e "$trimmed_port\t----------\tConnected successfully" >> $output
    elif [ $ec -ne 0 ]; then
        echo -e "Exit code: $ec, Could not connect to the server $host on port $trimmed_port\n"
        echo -e "$trimmed_port\t----------\tCould not connect" >> $output
    fi
done

echo -e "\nPorts testing completed successfully, you can view the test result in the file $output"