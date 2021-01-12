+++
title = "Introduction to DLL"
date = "2019-12-15"
author = "Freakston"
tags = ["Windows", "Development"]
keywords = ["Windows", "C"]
description = "A short introduction to windows DLL"
showFullContent = false
+++

## What is a DLL -

A DLL stands for "Dynamic Link Library".It acts as a shared library. The executables can use functions from the DLL libraries by importing/attaching the DLL. Dynamic Libraries are libraries that are loaded from another file (typically the DLL file). They are similar in structure to PE, the only difference they have is that they contain export tables. The list of functions that can be loaded is in the export table of the DLL. The functions which do not appear on the export table are private functions. These functions cannot be accessed by other executables.

## Basic DLL Template 
```c
BOOL APIENTRY DllMain( HMODULE hModule,
                       DWORD  reason_for_call,
                       LPVOID lpReserved
                     )
{
    switch (reason_for_call)
    {
    case DLL_PROCESS_ATTACH:
    case DLL_THREAD_ATTACH:
    case DLL_THREAD_DETACH:
    case DLL_PROCESS_DETACH:
        break;
    }
    return TRUE;
}

__declspec(dllexport) int func1(int x) {
    puts("Func1 was called");
    return x;
    }

__declspec(dllexport) int func2(int x) {
    puts("Func2 was called");
    return x * 2;
}
```

In the above template, we have two functions called **func1** and **func2**. The function func1 just prints out that func1 was called Similarily func2 prints that it was called. As mentioned above the main difference in the structure of PE and DLL is the exports section which lists the functions which can be called by another executable. Now let's have a look at the exports section of the above created DLL. The exports section can be viewed using dumpbin. The command to do so is **dumpbin dllname /EXPORTS**

```bash
>dumpbin Testdll.dll /EXPORTS

Microsoft (R) COFF/PE Dumper Version 14.24.28314.0
Copyright (C) Microsoft Corporation.  All rights reserved.

Dump of file Testdll.dll
    
    The section contains the following exports of Testdll.dll
    ordinal  hint    RVA       name
     1         0   00001020    func1
     2         1   00001040    func2
```
We see new terms such as ordinal, hint, and RVA. There are two ways of exporting a function from a DLL, one is by using the function name which we have done above and the other one is by using the ordinal number. When we use the ordinal number we cannot use the **__declspec(dllexport)** we will have to .def the function with the ordinal number. For those interested in knowing more about this method can refer to [MSDN official documentation](https://docs.microsoft.com/en-us/cpp/build/exporting-from-a-dll-using-def-files?view=vs-2019).

## An executable which loads the DLL

Now that we have the DLL ready let's write an executable that loads functions from the above-created DLL.

```c
#include<windows.h>

typedef int (*ptr_func1)(int x);

ptr_func1 fn1;
ptr_func1 fn2;

HMODULE Testdll;
int main()
{
    Testdll = LoadLibrary("Testdll.dll");
    fn1 = (ptr_func1)GetProcAddress(Testdll,"func1");
    fn2 = (ptr_func1)GetProcAddress(Testdll,"func2");

    int x = 20;
    fn1(x);
    fn2(x);

    FreeLibrary(Testdll);
}
```

- HMODULE is going to have the handle to the loaded DLL.
- LoadLibrary is used to load the DLL.
- FreeLibrary is used to free the library has been loaded after the use.

## References 
- https://docs.microsoft.com/en-us/cpp/build/exporting-from-a-dll-using-declspec-dllexport?view=vs-2019
- https://docs.microsoft.com/en-us/windows/win32/dlls/dllmain