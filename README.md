# Starlink Statuspage

https://starlinkstatus.space/

Original project and conecept by [![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/C0C67UDEB)

### About

Starlinkstatus.space is a website that offers statistics from Starlink users worldwide. All data is collected by users that are interested in the performance of Starlink, and who run frequent speed tests as well as collect latency measurements with our script.

## How to Contribute Data

You can contribute data to https://starlinkstatus.space/ by registering for a free api key and running the script in this repository on a Linux device.
For best performance, ensure that the contributing device has an ethernet connection directly to the Dish's router. 


## WARNINGS AND CONSIDERATIONS 
This will use quite a lot of traffic! If you are NOT on a unlimited plan you should increase the interval of the speedtests.
Each test can use up to ~500Mb of Data, so a test every 15min could use up to 48gb/Day!


 
Windows Users should use the Automatic Installer by @tevslin:
https://github.com/Tysonpower/starlinkstatus/blob/main/windowsinstall/NativeWindowsREADME.md

If you want to use WSL2 you need to follow Microsofts WSL2 installation and continue on the WSL2 Ubuntu console afterwards.
https://docs.microsoft.com/en-us/windows/wsl/install

### Register a Account

Go to https://starlinkstatus.space and register an account by entering your email, username, and choosing a password. 
You'll recieve an email with instructions (you may need to check your Spam folder). After verifying, you'll get a second email with your API key.

### Install Prerequisite Software

#### Gnu Parallel

Since V 1.3 we use parallel to make latency collection faster, please make sure it is installed with `parallel --version`

If you need to install it use the following command: ``sudo apt install parallel``

#### Speedtest CLI

The Client script uses the Speedtest CLI by Ookla to run tests and optionally collect the data.
If you already have a Third-Party Speedtest CLI installed, remove it first.
See Ookla's tutorial for your platform: https://www.speedtest.net/de/apps/cli

For a Raspberry Pi, use the following:
```
wget https://install.speedtest.net/app/cli/ookla-speedtest-1.2.0-linux-armhf.tgz
tar zxvf ookla-speedtest-1.2.0-linux-armhf.tgz
sudo cp speedtest /usr/bin/speedtest
speedtest --accept-license --accept-gdpr
```
Run `speedtest -V` to check the version.

#### gRPCUrl

gRPCUrl is used to communicate with Dishy and optionally collect data.
Please install the GO SDK from Google first: https://golang.org/doc/install
```
go install github.com/fullstorydev/grpcurl/cmd/grpcurl@latest
sudo cp ./go/bin/grpcurl /usr/bin
```
Run `grpcurl version` to check the version.

### Install the Client

Download our Client Script (starlinkstatus_client.sh) which will collect latency measurements every 15 seconds and make a speedtest every 15 minutes.

The following flags are allowed:
```   
-k | --key        API Key for starlinkstatus.space
-s | --speedtest  Enable Speedtest Data, requires speedtest.net cli
-d | --dishy      Enable Dishy Data, requires gprcurl
-i | --interval   Interval for speedtest in seconds (default 15 min / 900s)
-o | --once       Run only once (simulates old version)
-h | --help       Show this Message
```

## Linux/Mac
The script is run by a cronjob on reboot; follow the commands below after the download to set it up.
Replace `~path/to/` with the script's location, and YOURAPIKEY with the key you recieved.
This example will run the script, including a Speedtest and data from your Dishy, every 15 minutes.
```
chmod +x starlinkstatus_client.sh
crontab -e
@reboot ~/path/to/starlinkstatus_client.sh -k 'YOURAPIKEY' -s -d
```
### Data Saver / speedtest interval
This example will run speedtests every 8 hours / 28800 seconds (3 times in total per day), the latency measurements will continue as usual.
```
chmod +x starlinkstatus_client.sh
crontab -e
@reboot ~/path/to/starlinkstatus_client.sh -k 'YOURAPIKEY' -s -d -i 28800
```
