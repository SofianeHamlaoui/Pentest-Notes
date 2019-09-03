# Windows
## `$MFT` Master File Table

- Kind of index of all files on the hard drive
- https://jmharkness.wordpress.com/2011/01/27/mft-file-reference-number/
- All the entries of the MFT have a reference number, composed of the number of the MFT entry (6 bytes) and the sequence number (2 bytes) in hexadecimal.
  - Ref: 0x002E00000000F1AB -> MFT entry: 00000000F1AB, Sequence number: 0x002E
- Can be dumped with: http://malware-hunters.net/all-downloads/

## `MRU` Most Recently Used

- NTUSER.dat
- Can be read with `RegRipper` with the plugin `runmru`.
  - Pulling the UserAssist, which stores the latest applications, shortcuts and documents opened by the user
    - rip.exe -p userassist -r ../NTUSER.DAT

## `USN` Journal (Update Sequence Number Journal)

- Keeps a log of the changes that are made in an NTFS volume
- http://forensicinsight.org/wp-content/uploads/2013/07/F-INSIGHT-Advanced-UsnJrnl-Forensics-English.pdf
- Extract the log with FTK Imager
- parse this journal with the NTFS Log Tracker: https://sites.google.com/site/forensicnote/ntfs-log-tracker

`C: \ $ Extend \ $ UsrJrnl,`

## Logs

`C: \ Windows \ system32 \ winevt \ logs`

## User logins

Each time a session is started the user profile is loaded. This action leaves a record in the Microsoft-Windows-User Profile Service log/Operational.evtx

## Registry

`C: \ Windows \ system32 \ config`

- Timezone: `HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation`
- Computer Name: `HKLM\SYSTEM\CurrentControlSet\Control\ComputerName`
- Last Shutdown: `HKLM\SYSTEM\CurrentControlSet\Control\Windows` -> `ShutdownTime`
- Build Number:  `HKLM\SOFTWARE\Microsoft\Windows\NT\CurrentVersion\CurrentBuildNumber`

- Important Registry Locations Collection: https://www.dfir.training/resources/downloads/windows-registry

## Prefetch

## File extensions of interest

http://www.hexacorn.com/blog/2019/02/11/file-extensions-of-interest/
