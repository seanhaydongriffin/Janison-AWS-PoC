# Janison AWS PoC


## AMIs

Import the following OS as AMIs ...

Windows 7
Windows 8.1
Windows 10

Additional AMIs can be manually created later from existing Instances (below).

## Instances

Create the resources contained in "resources.csv" in the repository above.  
Instances are created from AMIs.
All the Window 7, 8.1 and 10 resources will be created from the 3 AMIs above.
The Windows Server (2016) will be created from the AMI provided by AWS in the marketplace.

## Volumes

There should be an Elastic Block Store Volume generated for each Instance above.
Default is:
Type = gp2
Size = 40Gb

## Snapshots

There will be a single snapshot for each AMI (see "AMIs" above).

## Security Groups

Install the security groups and rules from the latest files from the repository as above ...

*_exportRulesToCsv.csv
*_exportSecurityGroupsToCsv.csv

Also note the following page for "Web Server Rules" as they would apply to Selenium Hub (a web server), and "Rules for Ping" ...

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules-reference.html

## Windows EC2 Instances

### Install Chrome for Windows (if not already)

### Start Selenium Node

C:\Selenium\selenium grid control.exe

## Mac EC2 Instances

### Remote Controlling the Mac with VNC

See https://www.lets-talk-about.tech/2020/12/aws-create-macos-desktop.html

...and ...

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

java -Xmx128m -jar ./selenium-server-4.0.0.jar node --host < Mac Public IP address > --detect-drivers true --publish-events tcp://< Selenium Hub Private IP address >:4442 --subscribe-events tcp://< Selenium Hub Private IP address >:4443 --config < toml config file >


java -Xmx128m -jar ./selenium-server-4.0.0.jar node --host 3.128.28.238 --detect-drivers true --publish-events tcp://172.31.27.218:4442 --subscribe-events tcp://172.31.27.218:4443 --config nodeConfig5555.toml
  
### Stopping Mac instances
  
When you stop a Mac instance, the instance remains in the stopping state for about 15 minutes before it enters the stopped state.

When you stop or terminate a Mac instance, Amazon EC2 performs a scrubbing workflow on the underlying Dedicated Host to erase the internal SSD, to clear the persistent NVRAM variables, and if needed, to update the bridgeOS software on the underlying Mac mini. This ensures that Mac instances provide the same security and data privacy as other EC2 Nitro instances. It also enables you to run the latest macOS AMIs without manually updating the bridgeOS software. During the scrubbing workflow, the Dedicated Host temporarily enters the pending state. If the bridgeOS software does not need to be updated, the scrubbing workflow takes up to 50 minutes to complete. If the bridgeOS software needs to be updated, the scrubbing workflow can take up to 3 hours to complete.

You can't start the stopped Mac instance or launch a new Mac instance until after the scrubbing workflow completes, at which point the Dedicated Host enters the available state.

Metering and billing is paused when the Dedicated Host enters the pending state. You are not charged for the duration of the scrubbing workflow. 
  
  
  
  

