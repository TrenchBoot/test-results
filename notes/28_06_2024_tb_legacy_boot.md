# Intro

In the pursue of running new TB (Linux boot path) code on AMD, it may be useful
to have a booting reference that used to work some years ago.

Resources used:
* Blog posts
  - https://blog.3mdeb.com/2020/2020-08-13-trenchboot-event-log/
  - https://blog.3mdeb.com/2020/2020-07-03-trenchboot-grub-cbfs/
* old [meta-trenchboot repository](https://gitlab.com/trenchboot1/3mdeb/meta-trenchboot)
* old [testing-trenchboot repository](https://gitlab.com/trenchboot1/3mdeb/testing-trenchboot)

## Locating set of TB components

1. [This job](https://gitlab.com/trenchboot1/3mdeb/meta-trenchboot/-/jobs/660143007)
   claims to pass TB boot flow on a few platforms (including PC Engines APU2)
2. This is the meta-trenchboot
  [revision](https://gitlab.com/trenchboot1/3mdeb/meta-trenchboot/-/tree/49183efa4818987c7067ea4b35c3c25a9309b486)
3. It translate to following components:
   - Landing Zone (LZ) - today known as SKL
     - [recipe](https://gitlab.com/trenchboot1/3mdeb/meta-trenchboot/-/blob/49183efa4818987c7067ea4b35c3c25a9309b486/dynamic-layers/openembedded-layer/recipes-support/landing-zone/landing-zone_0.3.0.bb)
     - [code revision](https://github.com/TrenchBoot/landing-zone/commit/4dc904bf70d6913a5706bdfc6f7781a9ff039d38)
  - GRUB
    - [recipe](https://gitlab.com/trenchboot1/3mdeb/meta-trenchboot/-/blob/49183efa4818987c7067ea4b35c3c25a9309b486/dynamic-layers/openembedded-layer/recipes-bsp/grub/grub-tb_git.bb)
    - [code revision](https://github.com/3mdeb/grub2/tree/d6fa0d1f92446243e34e009361470f5730869ee9)
  - Linux
    - [recipe](https://gitlab.com/trenchboot1/3mdeb/meta-trenchboot/-/blob/49183efa4818987c7067ea4b35c3c25a9309b486/dynamic-layers/openembedded-layer/recipes-kernel/linux/linux-tb_5.5.bb)
    - [code revision](https://github.com/TrenchBoot/linux/tree/eed5cdf480ee3761d18294d64ac7e2184229b51c)

## Building image

1. Notes from building, although no need to do it - prebuild version
  [available here](https://cloud.3mdeb.com/index.php/s/P8sxp9LQjFZNNDk). 

```
wget https://raw.githubusercontent.com/siemens/kas/2.1.1/kas-docker
chmod +x ./kas-docker
git clone https://gitlab.com/trenchboot1/3mdeb/meta-trenchboot.git
cd meta-trenchboot
git checkout 49183efa4818987c7067ea4b35c3c25a9309b486
cd ..
SHELL=bash ./kas-docker build meta-trenchboot/kas-pcetb-base.yml
```

## Running image

Image has been started on the PC Engines APU2 platform with
[Dasharo (coreboot + SeaBIOS) v24.05.00.01](https://docs.dasharo.com/variants/pc_engines/releases_seabios/#v24050001-2024-06-28).

## Test results

### Booting

Boot log can be available [here](../logs/28_06_2024_tb_legacy_boot.log).

### PCRs

1. Executed on the host:

```
git clone git@github.com:TrenchBoot/landing-zone.git
cd landing-zone
git checkout 4dc904bf70d6913a5706bdfc6f7781a9ff039d38
scp -O root@$APU2_IP:/boot/bzImage .
scp -O root@$APU2_IP:/boot/lz_header.bin .
./extend_all.sh bzImage
```

Result:

```
7251a94abb2ff0de0c2c6b89e16559e0e11605f4  SHA1
a67681a54c9e46ae39316d7cb2c983d052f8190478b916224420dd5a719d358f  SHA256
```

2. Exeucted on the APU2:

```
root@tb:~# tpm2_pcrlist
sha1 :
  0  : 3a3f780f11a4b49969fcaa80cd6e3957c33b2275
  1  : 639d6beca9cf408d4266548378f74c01e0835681
  2  : e6e6645fb9f15ee84e8d87144ffb6184604437f9
  3  : 3a3f780f11a4b49969fcaa80cd6e3957c33b2275
  4  : 29e503140459f9ae7b6fb502ab0486d941de948c
  5  : e39b032eaa782999471de86e00fcb3c7c254eab6
  6  : 3a3f780f11a4b49969fcaa80cd6e3957c33b2275
  7  : 3a3f780f11a4b49969fcaa80cd6e3957c33b2275
  8  : 0000000000000000000000000000000000000000
  9  : 0000000000000000000000000000000000000000
  10 : 0000000000000000000000000000000000000000
  11 : 0000000000000000000000000000000000000000
  12 : 0000000000000000000000000000000000000000
  13 : 0000000000000000000000000000000000000000
  14 : 0000000000000000000000000000000000000000
  15 : 0000000000000000000000000000000000000000
  16 : 0000000000000000000000000000000000000000
  17 : b8e108dfa488195c2bd86a2cba16e983e98e37ed
  18 : 0000000000000000000000000000000000000000
  19 : 0000000000000000000000000000000000000000
  20 : 0000000000000000000000000000000000000000
  21 : 0000000000000000000000000000000000000000
  22 : 0000000000000000000000000000000000000000
  23 : 0000000000000000000000000000000000000000
sha256 :
  0  : e21b703ee69c77476bccb43ec0336a9a1b2914b378944f7b00a10214ca8fea93
  1  : 37a60d915ea369df3b4c24ce34c8ee115d2a87f8362b64d982941ba6ca5d3f9b
  2  : 8c92c8075ba25c46cb3424502a41184b2cf8994aa6f32b24c266cf45bfb36735
  3  : e21b703ee69c77476bccb43ec0336a9a1b2914b378944f7b00a10214ca8fea93
  4  : 40f1272fd32ae0ba5c2000bc600199b0a4422a359b100033140120359690d717
  5  : e778a0f4176fdba0964f1b709aa41fed0926accce6be1684fcf83e7c04dbb823
  6  : e21b703ee69c77476bccb43ec0336a9a1b2914b378944f7b00a10214ca8fea93
  7  : e21b703ee69c77476bccb43ec0336a9a1b2914b378944f7b00a10214ca8fea93
  8  : 0000000000000000000000000000000000000000000000000000000000000000
  9  : 0000000000000000000000000000000000000000000000000000000000000000
  10 : 0000000000000000000000000000000000000000000000000000000000000000
  11 : 0000000000000000000000000000000000000000000000000000000000000000
  12 : 0000000000000000000000000000000000000000000000000000000000000000
  13 : 0000000000000000000000000000000000000000000000000000000000000000
  14 : 0000000000000000000000000000000000000000000000000000000000000000
  15 : 0000000000000000000000000000000000000000000000000000000000000000
  16 : 0000000000000000000000000000000000000000000000000000000000000000
  17 : a67681a54c9e46ae39316d7cb2c983d052f8190478b916224420dd5a719d358f
  18 : 7fd9068bb68527a096b44bf6831b8ad0cbb8b53e045bfd3ecc427bd68ec27de3
  19 : 0000000000000000000000000000000000000000000000000000000000000000
  20 : 0000000000000000000000000000000000000000000000000000000000000000
  21 : 0000000000000000000000000000000000000000000000000000000000000000
  22 : 0000000000000000000000000000000000000000000000000000000000000000
  23 : 0000000000000000000000000000000000000000000000000000000000000000
```

SHA256 hash of PCR17 mathes to the pre-calculated one.
SHA1 does not.
In the [blog post](https://blog.3mdeb.com/2020/2020-08-13-trenchboot-event-log/)
both of them used to match.
