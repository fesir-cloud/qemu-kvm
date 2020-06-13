# Grafikkartendurchreichung zur Performanceverbesserung:

Um die Grafikleistung zu optimierenkönnen mehrer Methoden gewählt werden:

- Emulierung einer Grafikkarte
- Durchreichung einer nicht nativ eingebundenen Frafikkarte
  - Entweder direkt per Kernel-module ( "iommu-passthrough" )
    Hierfür wird eine Nicht vom Host genutze Grafikarte so konfiguriert dass diese nicht 
  - oder mit Sofware wie z.B. "looking Glas"
        https://looking-glass.hostfission.com/
  






----------------------------------------------------------------------------------------------
1. https://wiki.archlinux.org/index.php/QEMU/Guest_graphics_acceleration
2. https://bbs.archlinux.org/viewtopic.php?id=162768&p=1
3. https://looking-glass.hostfission.com/
