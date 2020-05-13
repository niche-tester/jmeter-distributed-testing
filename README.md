# jmeter-distributed-testing
Some key notes when setting up distributed testing using Jmeter and Azure

## Assumptions:
* You have setup a at least 2 VMs
* The VMs should have Ubuntu 18 installed on them 

### Jmeter Master and Slave setup on Ubuntu
1. Run update first 
```sudo apt update```

2. Check if Java is installed
```java -version```

3. If not, install JDK and JRE
```sudo apt install openjdk-8-jdk```
```sudo apt install openjdk-8-jre```

5. Download Jmeter installation using curl 
```curl http://apache.is.co.za/jmeter/binaries/apache-jmeter-<version>.tgz -o /home/<user>/apache-jmeter-<version>.tgz```

6. Unzip jmeter installation 
```tar -xzf apache-jmeter-<version>.tgz apache-jmeter-<version>```

7. Add or Edit ```bash_profile```
```vim .bash_profile```

8. Add jmeter installation to PATH 
```export PATH=”/home/denishas/apache-jmeter-<version>/bin/:$PATH”```

9. Save and refresh bash_profile 
```source ~/.bash_profile```

10. Test to make sure the PATH is now set
```echo $PATH```

11. Increase Java heap size to avoid 'OutOfMemory' issues while running tests 
Go to ~/jmeter-apache/bin , then Open jmeter file and update the following line:
```"${HEAP:="-Xms2g -Xmx2g -XX:MaxMetaspaceSize=512m"}"```
*-Xms2g* is the minumum memory allocation and *-Xmx2g* is the maximum memory allocation

###  Additional configuration: Jmeter Master

1. For ease of testing, install remote desktop connection on the master instance:
https://docs.microsoft.com/en-us/azure/virtual-machines/linux/use-remote-desktop
2. Open the following ports (Inbound and Outbound)
- 4000 TCP
3. Install Taurus:  
https://gettaurus.org/install/Installation/#Installing-Taurus-Manually
If there is no JMeter installed at the configured path, Taurus will attempt to install the latest JMeter and associated plugins into this location (by default this is: ```~/.bzt/jmeter-taurus/bin/jmeter```). You can change this setting to your preferred JMeter location (consider putting it into the ```~/.bzt-rc``` file).
You can also configure report settings here as well.

### Additional configuration: Jmeter Slave

1. Disable RMI keystore, if you don't want to use SSL for RMI
Go to jmeter *bin* folder and find the ```jmeter.properties``` file
Edit this file, uncomment the following line
```server.rmi.ssl.disable=true```

2. Set jmeter server port to use port 4000 by uncommenting this line in the ```jmeter.properties``` file
```server.rmi.localport=4000```

3. Open the following ports (Inbound and Outbound)
- 4000 TCP


###  References
https://medium.com/@yash3x/how-to-setup-distributed-load-testing-with-jmeter-fba703f41a32
https://medium.com/@viniciuscorrei/have-you-ever-needed-to-simulate-a-higher-number-of-virtual-users-it-is-usually-done-in-stress-c8dc9d922d2f
