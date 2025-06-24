# File Format
- `.text` - Import Table, Import Address Table and .NET Section
- `.reloc` - To relocate the address which the EntryPoint instruction jumps to (it's the only address contained the IAT). The IT counts just one imported module (mscoree.dll) and one imported function (\_CorExeMain for executables and \_CorDllMain for dynamic load libraries).
- `.rsrc` - Main icon for an executable, since all others resources are in the .NET Section.

> References:
> - https://www.ntcore.com/files/dotnetformat.htm
> - https://www.red-gate.com/simple-talk/blogs/anatomy-of-a-net-assembly-pe-headers/

# Dynamically load memory-only modules

- [Assembly.Load(byte[])](https://msdn.microsoft.com/en-us/library/system.reflection.assembly.load)

> References:
> - https://www.endgame.com/blog/technical-blog/hunting-memory-net-attacks

# .NET framework included in OS version

> https://blogs.msdn.microsoft.com/astebner/2007/03/14/mailbag-what-version-of-the-net-framework-is-included-in-what-version-of-the-os/

# Tutorials

- Reverse Engineering .NET Applications For Beginners: https://www.youtube.com/watch?v=KOVXWRrd_qg

# Tools

- CFF Explorer: https://ntcore.com/?page_id=388
- PE inspection library allowing .NET programmers to read, modify and write executable files: https://github.com/Washi1337/AsmResolver
- Parser for Windows Portable Executable headers: https://github.com/secana/PeNet
