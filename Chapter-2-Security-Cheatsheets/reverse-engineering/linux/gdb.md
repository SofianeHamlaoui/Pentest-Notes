# Profiles

- PEDA - http://ropshell.com/peda/Linux_Interactive_Exploit_Development_with_GDB_and_PEDA_Slides.pdf
-  

# Display Information
```
info registers
info all-registers
```
# Display memory map
```
vmmap
```

# Display Registers / Memory
```
display /x $eax
```
```
x/50c $eax
x/s $eax
```
# Disassemble

```
set disassembly-flavor intel
```

```
disassemble $eip
```

# Print Type Information
```
ptype Student
```

# Check security information
```
checksec
```
