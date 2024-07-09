# Radare2 Cheatsheet

```
r2 ipa://myipa.ipa : load the ipa
eco : (skin)
aar : analyze opcode and relative references
aae : emulate code to identify new pointer references
aao : Analyze all objc references
e emu.str-true : enable string emulation
i : bin info
il : linked libraries
ic > classes.txt
icc > classes-dump.txt
izz : strings
izzq > strings.txt
izz~string : search string
axt : Find xrefs to an address
s : seek to address
V : go to Graph view mode
p : change the view mode
n/N : jump next/previous flag
df : define function
pdsf : print disassemble summary for function
_ : search using HUD
x : find xrefs to/from data
u/U : Undo/redo seek
```

