> Hey there, app enthusiasts! 👋 Ready to dive into the world of game-changing apps? At [jterrazz.com](https://jterrazz.com), I'm all about crafting useful applications and sharing the journey! Discover coding insights, self-improvement hacks, and sneak peeks of my latest projects (psst... my next app is all about leveling up in life! 🚀). Come along for the ride - you might just find the spark for your next big idea! 💡💻

# 42-override
> Follow-up to [rainfall](https://github.com/jterrazz/42-rainfall). Security challenges focused on binaries: buffer overflows, printf insertions, shellcode injections and binary analysis, this time also featuring 64bit binaries. The project ressources (ISO and subject) are avaible in the 42 school intranet.

## Common walkthrough

We use `radare2` to analyse the binary:
- We disassemble using the visual mode `r2 ./level00` then `v`.

We use `gdb` to debug the runtime:
  - breakpoints: `b *addr` and `b function`
  - move: `nexti`, `stepi`
  - run: `r`, `r $(CMD)`, `r < file` 
  - set follow-fork-mode child, to follow child process 

## Resources

### Programs
- `r2`: disassembly ([doc](https://radare.gitbooks.io/radare2book/visual_mode/intro.html))
- `gdb`: runtime debugging and disassembly
- `readelf`
- `ltrace`
- `python` scripting

### Helpers
- [Shellcode list](http://shell-storm.org/shellcode/)
- [Overflow pattern generator](https://wiremask.eu/tools/buffer-overflow-pattern-generator/)
- [Int/Uint converter](https://www.binaryconvert.com/convert_unsigned_int.html)
- [X86 Cheatsheet](https://trailofbits.github.io/ctf/vulnerabilities/references/X86_Win32_Reverse_Engineering_Cheat_Sheet.pdf)
- [SIG list](https://unix.stackexchange.com/questions/317492/list-of-kill-signals)

### Learning

- [Introduction to 64bit assembly](https://software.intel.com/en-us/articles/introduction-to-x64-assembly)
- [Understand overflow attacks, with stack, registers, etc ❤](https://itandsecuritystuffs.wordpress.com/2014/03/18/understanding-buffer-overflows-attacks-part-1/)
- [Buffer overflow with an environment variable](http://seanmurphree.com/blog/?p=54)

## Presentation
The project is contained in a virtual machine ISO that has 9 users. We know the password for the first one. To access the next user, we must exploit the given binary to extract the next user password.

### Security features
#### GCC stack support protector: Enabled

> If you have a bug and make a buffer change based on something an attacker can control, that attacker can overwrite the return address or similar portions of the stack to cause it to execute their code instead of your code. Stack protection will abort your program if it detects this happening.

#### Strict user copy check: disabled
Disabled Enforce read-only kernel data. It means we will be able to scp the binary.

#### Restrict /dev/mem access: enabled
It provides access to the system's physical memory.

#### Restrict /dev/kmem access: enabled
Same as above

#### grsecurity / PaX: No GRKERNSEC
> PaX is a patch to the Linux kernel that provides hardening in three ways:
Judicious enforcement of non-executable memory
Address Space Layout Randomization (ASLR)
Miscellaneous hardening on stack- and memory handling

#### Kernel jeap hardening

#### System wide ASLR: Off
> Address space layout randomization (ASLR) is a computer security technique involved in preventing exploitation of memory corruption vulnerabilities. In order to prevent an attacker from reliably jumping to, for example, a particular exploited function in memory, ASLR randomly arranges the address space positions of key data areas of a process, including the base of the executable and the positions of the stack, heap and libraries.

#### No RELRO
Relocation Read-Only (or RELRO) is a security measure which makes some binary sections read-only.

#### No stack canary
> Stack canaries, named for their analogy to a canary in a coal mine, are used to detect a stack buffer overflow before execution of malicious code can occur. This method works by placing a small integer, the value of which is randomly chosen at program start, in memory just before the stack return pointer. Most buffer overflows overwrite memory from lower to higher memory addresses, so in order to overwrite the return pointer (and thus take control of the process) the canary value must also be overwritten. This value is checked to make sure it has not changed before a routine uses the return pointer on the stack

#### NX Enabled
> The NX bit (no-execute) is a technology used in CPUs to segregate areas of memory for use by either storage of processor instructions (code) or for storage of data.

This means we can't rewrite the program code in ram.

#### no PIE
> In computing, position-independent code (PIC) or position-independent executable (PIE) is a body of machine code that, being placed somewhere in the primary memory, executes properly regardless of its absolute address.

#### No RPATH / No RUNPATH
> The rpath and runpath of an executable or shared library are optional entries in the .dynamic section. They are both a list of directories to search for.
> 
> rpath designates the run-time search path hard-coded in an executable file or library. Dynamic linking loaders use the rpath to find required libraries.
Advanced binary analysis project. Primarily x86-64 bytecode
