# Win10 kleinkriegen

1.  Schreibe folgende Eigenschaften in die Registry-Datenbank:
(https://www.askvg.com/collection-of-registry-tweaks-for-windows-7/)

        Windows Registry Editor Version 5.00

        [HKEY_CLASSES_ROOT\AllFilesystemObjects\shellex\ContextMenuHandlers\Copy To]
        @="{C2FBB630-2971-11D1-A18C-00C04FD75D13}"

        [HKEY_CLASSES_ROOT\AllFilesystemObjects\shellex\ContextMenuHandlers\Move To]
        @="{C2FBB631-2971-11D1-A18C-00C04FD75D13}"

        [HKEY_CURRENT_USER\Control Panel\Desktop]
        "AutoEndTasks"="1"
        "HungAppTimeout"="1000"
        "MenuShowDelay"="8"
        "WaitToKillAppTimeout"="2000"
        "LowLevelHooksTimeout"="1000"

        [HKEY_CURRENT_USER\Control Panel\Mouse]
        "MouseHoverTime"="8"

        [HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer]
        "NoLowDiskSpaceChecks"=dword:00000001
        "LinkResolveIgnoreLinkInfo"=dword:00000001
        "NoResolveSearch"=dword:00000001
        "NoResolveTrack"=dword:00000001
        "NoInternetOpenWith"=dword:00000001

        [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control]
        "WaitToKillServiceTimeout"="2000"
    
    
        [HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\WindowsUpdate\AU]
        "NoAutoUpdated"=dword:00000001
    
2.  sysdm.cpl  >>  Erweitert   >>   Leistung  >>  Einstellungen  -> (x) Für optimale Leistung anpassen

3.  Windows + I  >>  Erleichterte Bedienung  >>  Anzeige  ->  Transparenz in Windows anzeigen (aus)

4.  Windows + I  >>  Personalisierung  >>  Farbenn  ->  Tranzparenzeffekte (aus) + Farben auf Dunkel stellen

    Windows + E  >>  Ansicht  >> Optionen  ->  /austoben (

5.  shell:ChangeRemoveProgramsFolder  >>  Windows-Features aktivieren oder Deaktivieren  ->  Internetexplorer 11 & Media Player deaktivieren

6.  services.msc  //Hier könne "überflüssige services auf MANUAL gestellt werden"/"niemals deaktivieren, fals doch wichtiger Systemprozess"  Folgende Liste kann bedenkenlos manualisiert werden:
    
    Connected User Experiences and Telemetry 
    Diagnostic Policy Service
    Diagnostic Tracking Service
    dmwappushsvc 
    Downloaded Maps Manager 
    IP Helper 
    Program Compatibility Assistant Service
    Remote Registry
    Security Center
    TCP/IP NetBIOS Helper
    Touch Keyboard and Handwriting Panel Service
    Windows Defender Service
    Windows Error Reporting Service
    Windows Search


7. Autostart aufräumen 
    regedit  >>  Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
    regedit  >>  Computer\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
 
 
8. Vorinstallierten Müll entfernen:

Power-Shell:

    
    Alle Installieretn Packages anzeigen
    get-AppxPackage| Select Name, PackageFullName
    
    Mit folgendem Power-SHell Befehelf findet man z.B. die PackageFullName von Edge
    get-AppxPackage| Select Name, PackageFullName|select-string "edge"
    
    Entfernen:
    
    Get-AppxPackage <PackageFullName> | Remove-AppxPackage
    
    bzw.
    
    get-AppxPackage|select-string Weather|remove-appxpackage
 
 
 
 
 
 
 
 # Nützliche shellcommandos:
    
    shell:common administrative tools
    shell:printersfolder
    shell:printhood
    shell:ChangeRemoveProgramsFolder
    shell:CryptoKeys
    shell:DpapiKeys
    shell:ConflictFolder
    shell:Device Metadata Store
    shell:Fonts
   
   
   
   
   
   
   
   
   
#  Zum Schluss noch ein cheatsheet 

    Nützliche .msc & .cpl & .exe
    
    sysdm.cpl
    printmanagement.msc
    mstsc.msc
    gpedit.msc
    regedit
    sfc /Scannow
    slui
    eventvwr
    service
    netplwiz
    compmgmt.msc
    shutdown /i
