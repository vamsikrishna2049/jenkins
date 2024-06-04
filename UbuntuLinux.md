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

## STEP 7: Upgrading the Local Repository Packages
```xml
sudo apt upgrade -y
