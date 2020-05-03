Folgender Workthrough ist ein Komplement aus diesen 3 Tutorials bezogen auf eine "Debian 9"/"Ubuntu18.04"/"Mint 19.X" analoge Distribution - und hat jetzt schon >2 Wochen(enden) viel Spaß bereitet :)

(
Cli-Workthrough
https://www.youtube.com/watch?v=HXgQVAl4JB4

https://www.youtube.com/watch?v=HXgQVAl4JB4

Video 2


https://www.youtube.com/watch?v=HXgQVAl4JB4

#PCI-Durchtreichung über ovmf mittels vfio:

#Arch-Wiki-VFIO-pci via OVMF


https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#With_vfio-pci_loaded_as_a_module

 

 

 

#CHEATSHEET:

# dmesg | grep -E "DMAR|IOMMU"

//dmesg gibt den "kernel-ring-buffer" aus, und grep filtert die Zeilen mit "DMAR" und "IOMMU" heraus: wenn Output-> werden solche module im kernel geladen(wichtig)


# dmesg | grep -i vfio

//Überprüfe ob vfio-module geladen sind

pci-device-checks:

# lspci -nn

//Listet alle PCI-Geräte und ihre Hardware-Adressen(xxxx:xxxxx), sowie ihre CPU-Zuordnungen(xx:xx.x) auf


# lspci -k

//Listet alle PCI-Geräte und ihre kernel-Modul zugehörigkeit auf


# find  /sys/kernel/iommu_groups/ -type l 

//zeigt IOMMU-Groups der PCI-Geräte

# sudo nano /etc/initramfs-tools/modules 

//config datei von initram-filesystem

)

 

 ----------------------------------------------------------------------------------------------------------------

 

#auf  zuerst ACS patchen, wirklich nicht so schwer :) -> quecumber


https://queuecumber.gitlab.io/linux-acs-override/

#überprüfen mit:

uname -a

#dann installiere folgende Pakete:

sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virt-manager ovmf

#GRUB-Konfigurationsdatei:

sudo nano /etc/default/grub

#GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on vfio-pci.ids=8086:9d71,8086:5926 pcie_acs_override=downstream"
#lspci -nnk

sudo update-grub

 reboot

 

sudo echo vfio\nvfio_iommu_type 1\nvfio_pci\nvfio_virqfd >>/etc/modules

#lädt die vfio-module zuerst, bevor der kernel die devices lädt, um sie am libvirt zu übergeben, sbald dieses geladen wird

#wenn "bash: /etc/modules: Keine Berechtigung oder su:legitierungsfehler" dann_>!!!die 4 zeilen per hand in /etc/modules schreiben

sudo update-initramfs

reboot

#überprüfe mit "$lspci -k" ob das richtige kernel-module für die vfio devices geladen wurde (vfio-XXXXX)

#

 

sudo virt-manager

 

----------------------------------------------------------------------------------------------------------------

 

#Konfiguration der neuen Maschiene:

 

#virtio-win drivers iso

#latest virtio-win ios: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso


 

----------------------------------------------------------------------------------------------------------------

1.: Lokales Installationsmedium(Iso Abbild oder CDROM):

(hier könnten uach die Architekturoptionen geändert werden, aber es wird der default: x86_64, also moderne 64-bit Prozessorarchitektur genutzt)

2.: ISO-Abbild benutzen: Windows10 Installations-iso auswählen

 

 

----------------------------------------------------------------------------------------------------------------

 

----------------------------------------------------------------------------------------------------------------

 

Firmware: BIOS

Chipsatz:i440FX

 

Datenträger: VirtIO

Speicherformat: qcow2

 

2 SATA CDROM-Laufwerke //2. Laufwerk für "virtio-win ios CD" während der  Installation->wichtig für Erkennung der "VirtIO-Datenträger"

 

NIC-Modul.....testen:

Netzerkquelle: eno 1: mactap | Brücke


Gerätemodell: e1000

 

Anzeige: "Video Spice"

 

----------------------------------------------------------------------------------------------------------------

 

 

 
