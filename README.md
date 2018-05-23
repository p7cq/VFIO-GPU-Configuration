VFIO GPU Configuration

*System*:

* **CPU**: [Intel® Core™ i7-6700 Processor](https://ark.intel.com/products/88196/Intel-Core-i7-6700-Processor-8M-Cache-up-to-4_00-GHz)
* **Motherboard**: [Asus Z170 Deluxe](https://www.asus.com/Motherboards/Z170-DELUXE/)
* **GPU**: Asus STRIX GTX 970
* **iGPU**: Intel HD Graphics 530
* **OS**: Fedora 28 Workstation  Edition
* **Firmware**: UEFI


*Enable IOMMU*

* BIOS:

  * VT-d Enabled
  * VT-x Enabled
  * Graphics IGFX
* Kernel

    * In `/etc/sysconfig/grub` append  `rd.driver.pre=vfio-pci intel_iommu=on iommu=pt` to `GRUB_CMDLINE_LINUX` ; do not copy the entire line from the file in this repo, as it contains `nopti` and `spectre_v2=off`, thus ignoring [this](https://meltdownattack.com/)
    * Run `# grub2-mkconfig -o /etc/grub2-efi.cfg` (check with:  `# cat /proc/cmdline`)
    * Create `/etc/modprobe.d/vfio.conf` and add `options vfio-pci ids=10de:13c2,10de:0fbb`
    * Create `/etc/dracut.conf.d/vfio.conf` containing `add_drivers+="vfio vfio_iommu_type1 vfio_pci vfio_virqfd"`
    * Regenerate initramfs ``# dracut -f --kver  `uname -r` `` and reboot


*KVM/QEMU*

* Install Windows guest OS

* Install and setup TightVNC server

* Shutdown guest and edit domain XML file using `# virsh edit windows` and add


```
  <kvm>
    <hidden state='on'/>
  </kvm>
```
inside `<features/>` tag

* Install Nvidia driver in guest OS and reboot


References:

 * [PCI Passthrough](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF)


 * [VFIO GPU How to series](http://vfio.blogspot.ro/2015/05/vfio-gpu-how-to-series-part-1-hardware.html)








  ```

  ```
