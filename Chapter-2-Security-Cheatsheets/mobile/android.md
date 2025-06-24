# Take screenshots from ADB:
```
adb shell screencap -p | perl -pe 's/\x0D\x0A/\x0A/g' > screen.png
```

# `seccomp` `ptrace`

- Issue 1718: Android: ptrace hole makes seccomp filter useless on devices with kernel <4.8:
https://bugs.chromium.org/p/project-zero/issues/detail?id=1718
- Chainspotting: Building Exploit Chains with Logic Bugs: https://labs.mwrinfosecurity.com/publications/chainspotting-building-exploit-chains-with-logic-bugs/
