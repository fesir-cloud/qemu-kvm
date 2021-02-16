
Überprüfen ob Prozessor Die richtigen Flags aktiviert hat:
cat /proc/cpuinfo|egrep -c '(vmx|svm)' 




# Pakete für QEMU/KVM-Hardwarevirtuaisierung:
-----------------------------------------------

(ohne KVM)
(# qemu qemu-system-x86_64)

mit qemu-KVM-Prozessoremulation und virtio-Hardwareemulation(VT-x und VT-d oder AMD analogon)(Pakete für Mint 19.3)

# qemu-kvm libvirt-bin libvirt-deamon-system libvirt-clients 


(Für Mint 20: https://www.itzgeek.com/post/how-to-install-kvm-on-ubuntu-20-04-linux-mint-20/)  
# qemu-kvm libvirt-daemon-system libvirt-clients


(libvirt oder libvirt-bin ist das Paket welches libvirt(einen im Kernel verwaltet
# qemu-utils
(für image verwaltung und ...)


UEFI
# ovmf 

für GUI
# virt-manager
zum Verbinden einmal als root starten und Nutzer der libvirtd Gruppe hnzufügen

Nach der Installation:
# systemctl start libvirtd

# systemctl enable libvirtd


# SPICE:

gir1.2-spiceclientgtk-3.0



-----------------------------------------------

für qemu-img muss qemu-utils mitinstalliert werden

Ein Disk-Image erstellen:
# qemu-img create -f qcow2 /var/lib/libvirt/images/Disc1-VM.qcow2 60G


# virsh list --all
 
kopieren eines bereits erstellten Images:
 
# qemu-img create -b path_to_SOURCE.qcow2 -f qcow2 -F qcow2 path_to_DEST.qcow2


-----------------------------------------------


# virsh define <IMAGE_PATH>/<XML-Datei>.xml


-----------------------------------------------
oder parameter per virsh install festlegen:

    virt-install --connect=qemu:///system \
    --name=Windowws-X-y \
    --ram=6144 \
    --vcpus=4 \
    --arch=x86_64 \
    --os-type=windows \
    --os-variant=windows \
    --hvm \
    --virt-type kvm \
    --cdrom=DVD.iso \
    --disk path=****.qcow2,format=qcow2,bus=virtio \
    --network bridge=vbr0,model=virtio \
    --accelerate \
    --vnc

---------------------------------------------------------------------------

# bridge-utils bridge:
Mit bridge-utils

This section describes the management of a network bridge using the legacy brctl tool from the bridge-utils package, which is available in the official repositories. See brctl(8) for full listing of options.

Erstelle eine neue Netwerkbrücke:
# brctl addbr bridge_name

Ordne Netzwerkinterface der Brücke zu, z.B. eth0:
Note: Adding an interface to a bridge will cause the interface to lose its existing IP address. If you are connected remotely via the interface you intend to add to the bridge, you will lose your connection. This problem can be worked around by scripting the bridge to be created at system startup.
# brctl addif bridge_name eth0

Zeige alle Brücken und die zugeordneten Interfaces:
# brctl show


Setze das Brückendevice auf "up"(online):
# ip link set dev bridge_name up


    Um eine Brückez zu löschen muss sie erst "down"(offline) gesetzt werden: 
    # ip link set dev bridge_name down
    # brctl delbr bridge_name


Wenndie Brücke vollständig eingerichtet ist kann ihr eine IP(v4) vergeben werden:
# ip addr add dev bridge_name 192.168.66.66/24



--------------------------------------------------------------------------



# IOMMU-Passthrough

0. setzten der VT-x und VT-d parameter im BIOS

1. In /etc/default/grub folgendes in 

       GRUB_CMDLINE_LINUX_DEFAULT="quiet"
-->      
       
       GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on" 
   hinzufügen und grub updaten ("$update-grub")
   (Wenn ein UEFI-Boot benutzt wird muss statdessen /etc/kernel/cmdline verändert werden)
   
2. In /etc/modules folgendes 

       vfio
       vfio_iommu_type1
       vfio_pci
       vfio_virqfd
hinzufügen und initramfs updaten ("$update-initramfs -u -k all")
3.






.






.






.





.




# Device-passthrough per I/O-module:


Folgender Workthrough ist ein Komplement aus diesen 3 Tutorials bezogen auf eine "Debian 9"/"Ubuntu18.04"/"Mint 19.X" analoge Distribution :)

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

 

# ACS-Override patchen:(notwendig, wenn PCI-e Device nicht alleinstehend in einer IOMMU-Gruppe ist)

https://queuecumber.gitlab.io/linux-acs-override/

#überprüfen mit:

# uname -a

#dann installiere folgende Pakete:

# sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virt-manager ovmf

#GRUB-Konfigurationsdatei:

# sudo nano /etc/default/grub

#GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on vfio-pci.ids=8086:9d71,8086:5926 pcie_acs_override=downstream"
# lspci -nnk

# sudo update-grub

# reboot

 

sudo printf "vfio\nvfio_iommu_type 1\nvfio_pci\nvfio_virqfd">>/etc/modules

#lädt die vfio-module zuerst, bevor der kernel die devices lädt, um sie am libvirt zu übergeben, sbald dieses geladen wird

# sudo update-initramfs

# reboot

#überprüfe mit "$lspci -k" ob das richtige kernel-module für die vfio devices geladen wurde (vfio-XXXXX)

# sudo virt-manager

 

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

 

Firmware: UEFI

Chipsatz: Q35

 

Datenträger: VirtIO

Speicherformat: qcow2

 

2 SATA CDROM-Laufwerke //2. Laufwerk für "virtio-win ios CD" während der  Installation->wichtig für Erkennung der "VirtIO-Datenträger"

 

NIC-Modul.....testen: Brücke!!! 

Netzerkquelle: z.B. vbr0

Gerätemodell: virtio

Anzeige: "Video VNC"

 

----------------------------------------------------------------------------------------------------------------

 https://docs.fedoraproject.org/en-US/quick-docs/creating-windows-virtual-machines-using-virtio-drivers/

 

 https://r.search.yahoo.com/_ylt=AwrIAX.jcbBe.lwA8w5fCwx.;_ylu=X3oDMTByMWk2OWNtBGNvbG8DaXIyBHBvcwMyBHZ0aWQDBHNlYwNzcg--/RV=2/RE=1588650531/RO=10/RU=https%3a%2f%2fwww.ibm.com%2fdeveloperworks%2fcommunity%2fwikis%2fform%2fanonymous%2fapi%2fwiki%2ffad12df3-37d4-420d-83fe-e71b158ef55e%2fpage%2f8846d450-8965-4a11-bd79-cd648ed0ea0a%2fattachment%2f9024a5e2-d339-489d-906e-e534292cee80%2fmedia%2fcreate_kvm_vm_cli.pdf/RK=2/RS=LGuuYKjhpguodSukQhZOGclaOGw-
