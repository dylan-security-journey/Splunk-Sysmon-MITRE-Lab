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

<img width="752" height="467" alt="image" src="https://github.com/user-attachments/assets/2cf918b2-a339-48fa-b812-e6540d3b45dc" />

from the image above running the sysmon with the config file and also checking in event viewer to see sysmon status

<img width="1093" height="465" alt="image" src="https://github.com/user-attachments/assets/56ecdde9-f0c2-4545-a897-c589d89254ff" />

proceed to download splunk universal forwarder on windows VM, and run as admin

<img width="499" height="393" alt="image" src="https://github.com/user-attachments/assets/55d8c741-ec67-4dcf-847e-ca6c95b0a313" />

Making sure I customize options for later I need to input splunk server IP from Ubuntu VM

<img width="497" height="383" alt="image" src="https://github.com/user-attachments/assets/05d50e77-e955-4fc5-9ac1-e63959d657cd" />


Now create a sysmon index on Ubuntu VM because this is where the logs are getting forward to and stored

<img width="1105" height="454" alt="image" src="https://github.com/user-attachments/assets/70c6a16b-1bb4-4a55-9e6b-b8456181fbf4" />

Now on Windows VM, go to Splunk Universal Forwarder install path and go to the local directory inside where you'll create a inputs.conf file

Make sure to run the notepad as admin and paste code below

<img width="481" height="169" alt="image" src="https://github.com/user-attachments/assets/1c8b2a89-f82e-42c3-8760-2fbdb2d9bbb0" />

[WinEventLog://...] → tells the forwarder to monitor the Sysmon Operational Event Log.

disabled = 0 → ensures it’s active.

renderXml = true → sends full structured XML logs (better for Splunk parsing).

index = sysmon → ships the logs into the sysmon index you created on your Ubuntu Splunk.

<img width="648" height="117" alt="image" src="https://github.com/user-attachments/assets/4f5341d5-9602-4a30-b637-715f88194e2c" />

Move back to powershell to restart the Splunk Forwarde service so it updates with the new info that was created with input.conf

I ran into a troubleshooting issue where sysmons tried to read logs but got accessed denied so in order to fix go to services.msc from (win > R) and then click local system priviliges and restart once changes applied

<img width="2522" height="267" alt="image" src="https://github.com/user-attachments/assets/eaed0f51-33ab-4744-86f9-245c0790db78" />

<img width="1766" height="933" alt="image" src="https://github.com/user-attachments/assets/1411cc28-5122-41b5-8514-df4cab110127" />













