# Test cases

1. UEFI boot works - TrenchBoot boots into OS
2. Legacy boot works - TrenchBoot boots into OS
3. Normal boot (`boot` menuentry) works - TrenchBoot boots into OS
4. SKL boot (`skl-boot` menuentry) works - TrenchBoot boots into OS
5. `/boot/grub/grub.cfg` exists
6. `/boot/EFI/BOOT/grub.cfg` exist
7. `/boot/grub/grub.cfg` and `/boot/EFI/BOOT/grub.cfg` are the same
8. `grub.cfg` contains slaunch
9. Intel ACMs are available at `/boot/acm` and they are used in `grub.cfg` -
`grub.cfg` contains line with `/acm/<acm_filename>`
    - ADL_SINIT*.bin
    - BDW_SINIT*.bin
    - CFL_SINIT*.bin
    - CMLSTGP_SINIT*.bin
    - CML_RKL_S_SINIT*.bin
    - CML_S_SINIT*.bin
    - RKLS_SINIT*.bin
    - SKL_KBL_AML_SINIT*.bin
    - SNB_IVB_SINIT*.bin
    - TGL_SINIT*.bin
10. `skl.bin` exists at `/boot/skl.bin`
11. `grub.cfg` contains `/skl.bin` line
12. `tpm2_tools` is installed - check if `tpm2_tools` package is installed
13. `rsync` is installed - check if `rsync` is installed
14. There is at least one active PCR bank - check if there is at least one
directory: `/sys/class/tpm/tpm0/pcr-sha*`
15. PCRs 0 to 23 can be read from `/sys/class/tpm/tpm0/pcr-sha*`
16. When SKL booting - PCR 17 & 18 shouldn't contain only '0's or only `F`s
17. Kernel boots without critical errors
18. Kernel boots without errors
