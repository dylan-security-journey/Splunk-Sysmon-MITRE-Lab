<img width="771" height="386" alt="image" src="https://github.com/user-attachments/assets/d98a0208-8448-4105-a37e-8676ef3c5a33" />

<img width="762" height="475" alt="image" src="https://github.com/user-attachments/assets/23d9f523-3984-4eb9-bc56-97fe39881f72" />

First download splunk Enterprise on my Ubuntu VM through official website then with this code below extract and Run as admin, take this Splunk archive I already downloaded, and unpack it into /opt so it installs there.

sudo tar -xvzf splunk-10.0.0-e8eb0c4654f8-linux-amd64.tgz -C /opt

Then continute to run splunk for the first time below with this code

sudo ./splunk enable boot-start

then it will prompt to make a username and password

after Splunk is up and running on your Ubuntu VM, move over to Windows VM and type the enp0s8 IP into window's VM since they're already linked from earlier setup with adapter 2

<img width="1550" height="555" alt="image" src="https://github.com/user-attachments/assets/a8f68c29-b2d7-4f02-becf-5b8051aa751a" />

manage to setting where you forward and receive data and configure receive data with 9997, and then download sysmons off microsoft sysinternals

<img width="823" height="991" alt="image" src="https://github.com/user-attachments/assets/81a3a842-f9ca-4be0-9ffd-55311cc901b9" />

Then find a community sysmon config file because of the vast amount of logs will overpower my VM and finding a nice config will help with visuals

Proceed with unzipping the file and with newly downloaded config file move it to the same folder for easer powershell command later



