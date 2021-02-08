# Patching SmallTree to support NUC8i7HNK controller

So this little project came about when Vit9696 asked to update this kext to support SmallTreeIntel82576 version `1.2.5` (And now `1.3.0`), so here it is! Most users are likely running the `1.0.6` version Ydeng originally made then got rehosted both on [AMD OSX](https://drive.google.com/file/d/0B5Txx3pb7pgcOG5lSEF2VzFySWM/view) and on the OpenCore Guide.

**Sources**:

* Original kext: [SmallTree](https://small-tree.com/support/downloads/gigabit-ethernet-driver-download-page/)
* Original patch: [Ydeng's patch](https://www.insanelymac.com/forum/topic/324392-ryzen-clover-installation-guide-macos-sierra/?page=18&tab=comments#comment-2460269)
* The probe function checks 0xa.  If this patched to nop, it might work as well.

  0000000000001b40        movzwl  0xd74(%rbx), %ecx
  0000000000001b47        cmpl    $0xa, %ecx
  0000000000001b4a        movw    0xd76(%rbx), %ax
  0000000000001b51        jne     0x1cff   <-------                 patch to nop
 
  0000000000001b57        cmpw    $0x8086, %ax
 
  Replace "cmpl $0xa, %ecx" with
 
  "cmpl %ecx, %ecx
  nop"
  
* F: 0fb7 8b74 0d00 0083 f90a -> R: 0fb7 8b74 0d00 0039 c990
