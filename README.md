# Inline hooking bypass

Todos los loaders realizan la misma operación: reservar memoria, escribir shellcode y ejecutarlo en un hilo.

- [1_Win32_API](./1_Win32_API)  
  Ejemplos usando la API de Windows estándar:  
  `loader.exe -> kernel32.dll -> kernelbase.dll -> ntdll.dll -> syscalls`  
  Problemas: Sensible a hooks en user mode en toda la cadena.

- [2_NTAPI](./2_NTAPI)  
  Ejemplos usando funciones nativas (`ntdll.dll -> syscalls`)  
  Problemas: Sensible a hooks en `ntdll.dll`.

- [3_DirectSyscalls](./3_DirectSyscalls)  
  Syscalls directas en 64 bits con stubs propios.  
  Problemas: Detectables si el EDR usa **ETW** o **EtwTi** para verificar el área de memoria de la syscall o la dirección de retorno.

- [4_IndirectSyscalls](./4_IndirectSyscalls)  
  Syscalls indirectas ejecutándose desde direcciones válidas de `ntdll.dll`.  
  Problemas: si el EDR inspecciona toda la pila de llamadas, la ejecución puede fallar.

---

## Uso

Se requiere: **x64 Native Tools Command Prompt for Visual Studio 2022**

```bat
ml64 /c /Fo syscalls.obj syscalls.asm
cl /c loader.c
link loader.obj syscalls.obj /SUBSYSTEM:CONSOLE
```