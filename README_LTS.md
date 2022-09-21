# Additional info on building your own version of the rescue images

## Useful information
- It turns out Buildroot2 also has LTS versions, specifically the 20xx.02
  releases are the LTS ones.
- As of 2018.11-rc1 SEDutil became an "official" part of Buildroot2 -
  which means that when building newer Buildroot2 versions/targets, it
  becomes necessary to override the Buildroot2 "official" source for
  SEDutil with our own updated/customized tarball.  (Otherwise we end
  up with DTA SEDutil plus Buildroot2 patches but no other local
  patches that might be intentional/needed...)

## Updating Buildroot2  

 - Buildroot2 target version is determined by images/conf
 - Any changes to the Buildroot2 target version may require a new buildroot
    .config file to be created.  The actual files from the source tarball are
    kept in images/buildroot/{32,64}bit/.config but making changes to them
    should properly be done by:
    
    - execute "./buildpbaroot" the normal way from the images directory
    - _cd scratch/buildroot/32bit_
    - _make menuconfig_
    - Make any changes as needed
    - _cd ../64bit_
    - _make menuconfig_
    - Again, make any changes as needed
    - _cd .._
    - _make O=32bit_
    - _make O=64bit_
    - Now do whatever testing you believe is needed for the resulting output
    - Once you're happy, make sure you save those new 32bit/.config
      and 64bit/.config files somewhere, so you can use them again
      in the future.

## Updating Linux kernel version
    
 - Linux kernel version is determined by a config line in the buildroot
    .config files above; however updating to a newer Linux kernel may
    require new kernel.config files.  Changes of that sort can be 
    accomplished similar to what's shown above:
    
    - execute "./buildpbaroot" the normal way from the images directory
    - _cd scratch/buildroot/32bit/build/linux-[version]_
    - _make menuconfig_
    - Make any changes as needed
    - _cd ../../../64bit/build/linux-[version]_
    - _make menuconfig_
    - Again, make any changes as needed
    - _cd ../../.._
    - _make O=32bit_
    - _make O=64bit_
    - Now do whatever testing you believe is needed for the resulting
      output kernels
    - Once you're happy, make sure you save those new .config files
      for 32bit and 64bit kernels somewhere as 32bit/kernel.config
      and 64bit/kernel.config, so you can use them again
      in the future.]

## Speeding up image generation after kernel .config changes

Unfortunately, the "buildpbaroot" script deletes everything we compiled
previously, so each time we execute it, everything gets rebuilt. This is
exteremly time consuming if we only want to make some changes to the linux
kernel config. To speed up the process, the following apprach can be employed
once everything has been compiled at least once:
 - Modify the kernel config in the _images/scratch/buildroot/64bit/build/linux-[version]_ folder (make menuconfig)
 - _make clean_
 - _make -j4_
 - once the kernel has been compiled without errors, copy the generated _bzImage_
   from _linux-[version]/arch/x86/boot_ to _images/scratch/buildroot/64bit/images/_
 - Repeat previous steps for 32 bit kernel (with appropriate paths)
 - cd to _images/scratch/buildroot_ and rebuild the rootfs.cpio with the follwoing commands
 - _make O=32bit_
 - _make O=64bit_
 - cd to _/images_
 - run _buildbios_, _buildUEFI64_ and _buildrescue_ scripts to generate the rescue image
 - Repeat above steps to play around with the kernel config
 - Once happy with the results, save the _.config_ files to
   _images/buildroot/{32,64}bit/kernel.config_ for being able to do full builds.
