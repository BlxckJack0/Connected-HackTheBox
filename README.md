# Connected - This is a HackTheBox machine, We exploit a vulnerbility in a FREEPBX website Vulnerbility was an unauthenticated sqli RCE which is marked as CVE-2025-57819. Then after getting a foothold we escalate our privlege by log poisoning.


## 1- Reconnaissance
## We start with a An nmap Scan, Then we do a version,OS detection and basic script
<img width="1630" height="801" alt="Screenshot 2026-11-11 122927" src="https://github.com/user-attachments/assets/6e3f536b-12ff-43ee-93a5-9418076d4201" />
<img width="1632" height="782" alt="Screenshot 2026-11-11 123133" src="https://github.com/user-attachments/assets/78d5d1d7-4dd8-42b4-84ee-15bbf6f1bfc6" />



##


## 2 - Visiting the website it reveals it is using FREEPBX, its using the version 16.0.40
FreePBX is a free, open-source web-based graphical user interface (GUI) and management platform used to control and configure the Asterisk VoIP (Voice over Internet Protocol) communication server
<img width="1917" height="877" alt="Screenshot 2026-11-11 123011" src="https://github.com/user-attachments/assets/6e70007a-90ff-46d8-9ac1-b896e8d582ca" />

##

## 3- Started Searching for Famous cves then came across this one 
<img width="947" height="277" alt="Screenshot 2026-11-11 123027" src="https://github.com/user-attachments/assets/f7b65425-d5b8-49c2-89cf-2b378314629c" />

##

# 4-  Then i Started searching for the exploit cod, and found one on github by watchtowr
```
https://github.com/watchtowrlabs/watchTowr-vs-FreePBX-CVE-2025-57819/blob/main/watchTowr-vs-FreePBX-CVE-2025-57819.py
```

# 5 - I used the code and ran it on the website 
<img width="1645" height="792" alt="Screenshot 2026-11-11 123200" src="https://github.com/user-attachments/assets/a9310f0a-54fd-43ae-9a2d-a7f91303834c" />

<img width="1647" height="805" alt="Screenshot 2026-11-11 123250" src="https://github.com/user-attachments/assets/cc0562ef-fba6-411e-a749-066b30053366" />

##

## 6-  NICE, we have a shell
<img width="1917" height="987" alt="Screenshot 2026-11-11 123307" src="https://github.com/user-attachments/assets/141b7793-2eaa-4481-b6a8-b339db926334" />
##

# 7 - I tried injecting a reverse shell payload, and there is a filter blocking us, so then i URL encode it and it worked 
<img width="1917" height="871" alt="Screenshot 2026-11-11 123349" src="https://github.com/user-attachments/assets/5a5e4766-1428-44fc-8bc7-b538c1a504cf" />
<img width="1917" height="885" alt="Screenshot 2026-11-11 123425" src="https://github.com/user-attachments/assets/93f47bc5-81cb-479c-b45b-70a77a94694f" />
<img width="1857" height="650" alt="Screenshot 2026-11-11 123456" src="https://github.com/user-attachments/assets/ece4705a-5ead-4e07-945b-0fd3bfeb9317" />



##
# 8 -Its time to transfer linpeas to the comprimised machine.
<img width="1632" height="353" alt="Screenshot 2026-11-11 123731" src="https://github.com/user-attachments/assets/7ec45e12-b360-42fc-960b-5a814602375b" />
<img width="1862" height="652" alt="Screenshot 2026-11-11 123816" src="https://github.com/user-attachments/assets/81995970-4f58-4367-acb2-49b1447deb0f" />


# 9 - Linpeas Found a Service that is Ran by root, and has weak Write permission.
<img width="1247" height="251" alt="Screenshot 2026-11-11 124030" src="https://github.com/user-attachments/assets/70ee9406-b7c2-4e19-8002-291b85fc25c9" />
##

## 10 - There is a service in this machine called "dahdi" which its purpose is to Just watch files if they got modified or changed or anything and it runs as root, I checked the init.conf and as we can see its owned by us and we can write on it.
<img width="1247" height="251" alt="Screenshot 2026-11-11 124030" src="https://github.com/user-attachments/assets/c4c689b9-6572-4cec-a86a-dd2243e20aa0" />

##
## 12 - We inject a Reverse shell Payload
<img width="1866" height="647" alt="Screenshot 2026-11-11 125143" src="https://github.com/user-attachments/assets/2cff2a1a-ceb7-4215-929e-2b4786928708" />
##


## 13-  we need to Trigger the service we can trigger it by creating a file using touch command, or using echo "restart"> /path/to/file.
```
touch /var/spool/asterisk/sysadmin/dahdi_restart
or
echo "restart" > /var/spool/asterisk/sysadmin/dahdi_restart
```
Like that we are Triggering the script on run the malicious config file we replaced.
<img width="1882" height="646" alt="Screenshot 2026-11-11 125255" src="https://github.com/user-attachments/assets/313ef4ee-cee1-49ef-9fa4-35b0f9971fd8" />
##

## 14 - Then we get a shell and we are root 
<img width="1883" height="646" alt="Screenshot 2026-11-11 125356" src="https://github.com/user-attachments/assets/fb3a77a3-3e03-4154-8a85-eae41e539b71" />



## Make Sure to Star the repo if u understood the Writeup.









