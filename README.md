# cmder_DLL_Side-Loading
This repo details an issue in the cmder application (Full and Mini) in which a DLL is seached for and not found allowing an attacker the ability to get code execution

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
