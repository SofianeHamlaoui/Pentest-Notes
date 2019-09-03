- Schtasks without Schtasks.exe via Reflective DLL: https://ijustwannared.team/2018/08/13/schtasks-without-schtasks-exe-via-reflective-dll/
- Windows 10 Persistence via PATH directories - CDPSvc: https://www.a12d404.net/windows/2019/01/13/persistance-via-path-directories.html
  - CDPSvc (Connected Devices Platform Service) searches the file cdpsgshims.dll inside PATH directories and loads it if found.
  - With a default Windows installation, there can’t be a non-admin directory in the PATH, so this can’t be exploited.
  - A sample DLL (source included) that executes calc.exe is available here: https://github.com/marpie/a12d404.net-files/tree/master/CDPSvcPersist

- Useful for vulnerability research workflows on Windows: https://gist.github.com/jthuraisamy/af862987fff437daec52ee3cc5894203
- Windows 10 Device Security: https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2IIVu

- Windows Command Line cheatsheet (part 1): some useful tips: https://www.andreafortuna.org/technology/windows/windows-command-line-cheatsheet-part-1-some-useful-tips/
- Windows Command Line cheatsheet (part 2): WMIC: https://www.andreafortuna.org/dfir/windows-command-line-cheatsheet-part-2-wmic/
