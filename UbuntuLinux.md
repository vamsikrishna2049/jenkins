## STEP 1: Updating the local packages
```xml
sudo apt update -y
```

## STEP 2: Installing Java Run time Environment
```xml
sudo apt install fontconfig openjdk-17-jre
```

## STEP 3: Adding Jenkins Key
```xml
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

## STEP 4: Adding a repository for Jenkins to your APT package manager configuration
```xml
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

## STEP 5: Refresh the package cache after adding the repository.
```xml
sudo apt update -y
```

## STEP 6: Installing Jenkins
```xml
sudo apt-get install jenkins -y
```

## STEP 7: Upgrading the local Repository Packages
```xml
sudo apt upgrade -y
```

## STEP 8: Checking the Jenkins service
```xml
sudo systemctl status jenkins.service
```

## STEP 9: Start the Jenkins service
```xml
sudo systemctl start jenkins.service
```

## STEP 10: Enable the Jenkins service
```xml
sudo systemctl enable jenkins.service
```

## STEP 11: To display a snapshot of currently running Jenkins processes 
```xml
ps -aux | grep jenkins 
```

## STEP 12: Package for networking tasks on Unix-like operating systems. It allow users to monitor and troubleshoot network connections, interfaces, and routing tables
```xml
sudo apt install net-tools
```

## STEP 13: To display information about network connections and listening ports on a Unix-like operating system. 
```xml
netstat -nltp
```

netstat
displaying network connections, routing tables, interface statistics, masquerade connections, and multicast memberships
- n  netstat not to resolve IP addresses to hostnames, showing numerical addresses instead.
- l  display listening server sockets
- t  only TCP sockets should be displayed
- p  adds the process identifier (PID) and the name of the program associated with each socket

## Congragulations Jenkins installation successfully created at Ubuntu Linux
