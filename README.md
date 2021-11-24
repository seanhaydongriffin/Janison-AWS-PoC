# Janison AWS PoC

## Windows EC2 VMs



## Mac EC2 VMs

### Remote Controlling the Mac with VNC

The following instructions were taken from -> https://aws.amazon.com/premiumsupport/knowledge-center/ec2-mac-instance-gui-access/

Note: The following steps are tested for macOS Mojave 10.14.6 and macOS Catalina 10.15.7.

1.    Connect to your EC2 Mac instance using SSH ...

__Linux__

Use the following command to SSH to your EC2 Mac instance as ec2-user. Replace keypair_file with your key pair and Instance-Public-IP with the public IP of your instance.

% ssh -i keypair_file ec2-user@Instance-Public-IP

__Windows__

Windows 10 and newer versions of Windows Server have an OpenSSH client installed by default. Or, you can enable the OpenSSH client by selecting Settings, Apps, Apps & features, Manage optional features, Add a feature, and then select OpenSSH Client. If you're using an older version of Windows, then use Git Bash to implement the preceding command.

Note: The instance can be in a public subnet and accessible through a public IP address or an Elastic IP address. You can use a bastion/jumphost to connect to the instance. Or, you can establish a connection using AWS VPN or AWS Direct Connect that allows you to access your instance through a private IP. For security reasons, traffic to the VNC server is tunneled using SSH. It's a best practice to avoid opening VNC ports in your security groups.

2.    Run the following command to install and start VNC (macOS screen sharing SSH) from the Mac instance ...

sudo defaults write /var/db/launchd.db/com.apple.launchd/overrides.plist com.apple.screensharing -dict Disabled -bool false
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.screensharing.plist

3.    Run the following command to set a password for ec2-user: ...

sudo /usr/bin/dscl . -passwd /Users/ec2-user

4.    Create an SSH tunnel to the VNC port. In the following command, replace keypair_file with your SSH key path and 192.0.2.0 with your instance's IP address or DNS name ...

ssh -i keypair_file -L 5900:localhost:5900 ec2-user@192.0.2.0

Note: The SSH session should be running while you're in the remote session.

5.    Using a VNC client, connect to localhost:5900 ...

Note: macOS has a built-in VNC client. For Windows, you can use RealVNC viewer for Windows. For Linux, you can use Remmina. Other clients, such as TightVNC running on Windows don't work with this resolution.

6.    The GUI of the macOS launches. Connect to the remote session of the Mac instance as ec2-user using the password that you set in step 3 ...

You are now logged into your macOS desktop.

7.    Use Remote Desktop Manager (


### Install Chrome for Mac

Download the correct version from ...

https://google-chrome.en.uptodown.com/mac/versions


### Install Java

Go to www.java.com and download and install the latest version of JRE 8.


### Install Selenium Node

From this repository (above) download the latest selenium server.

### Start Selenium Node

Open a Terminal and ...

java -Xmx128m -jar ./selenium-server-4.0.0.jar node --host <Mac Public IP address> --detect-drivers true --publish-events tcp://<Selenium Hub Private IP address>:4442 --subscribe-events tcp://<Selenium Hub Private IP address>:4443


java -Xmx128m -jar ./selenium-server-4.0.0.jar node --host 3.128.28.238 --detect-drivers true --publish-events tcp://172.31.27.218:4442 --subscribe-events tcp://172.31.27.218:4443

