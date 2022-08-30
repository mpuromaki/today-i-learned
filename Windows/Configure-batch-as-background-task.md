# Running Batch file as background task

In new Windows versions there is issue with DCOM permissions when trying to run .bat file as background task,
ie. Task Scheduler task set with "Run whether user is logged in or not".

Below are the required steps to fixing this issue.

## Regedit modification

When trying to edit properties for "runtimebroker" under "component services", the settings can not be modified.
This seems to be due to issues with ownership and permissions in regedit.

In regedit go to ```HKEY_LOCAL_MACHINE\SOFTWARE\Classes\AppID{9CA88EE3-ACB7-47c8-AFC4-AB702511C276}```.

(Where that ID is the ID of RuntimeBroker under "component services". I think it's same for every Windows install?)

```
Right Click -> Permissions -> Advanced -> Set Owner to Administrator account on your domain.
Set "Full Control" rights for "Administrators"
```

## Restart COM+ Service

1. Open "services".
2. Restart "COM+ System Application".

## Configure Component Services

```
Component Services -> Computers -> My Computer -> DCOM Config -> RuntimeBroker
Right Click -> Properties
Edit "Launch and Activation Permissions".
Add Administrators Group with "local launch" and "local activation".
```

---

#### Tags and words to help search engines.

Below are list of things I searched for when trying to get this to work. Hopefully it helps search engines to find their way here.
- Windows scheduled batch
- Local activation permission for COM
- Microsoft Windows task scheduler local activation permission
- Task scheduler user account does not have permission to run this task
- Windows task scheduler runs manually fails in scheduler
- Windows DCOM can not edit runtimebroker
- Windows error 10016 runtime broker
- DCOM can't set launch and activation permissions
