#!/bin/bash


path=/var/lib/jenkins/workspace/Raghu

    cd $path
    mkdir bin
    PATH=$path/bin:$PATH
    curl https://storage.googleapis.com/git-repo-downloads/repo > $path/bin/repo
    chmod a+x $path/bin/repo
    mkdir twrp
    cd twrp
    echo -ne '\n' | repo init -u git://github.com/RaghuVarma331/Twrp-Manifest.git -b android-10.0 --depth=1
    repo sync
    git clone https://github.com/RaghuVarma331/android_device_samsung_m12-TWRP.git -b android-11.0 device/samsung/m12
    . build/envsetup.sh && lunch omni_m12-eng && make -j$(nproc --all) recoveryimage
    cd out/target/product/m12
    mv recovery.img twrp-3.4.0-0-m12-11.0-$(date +"%Y%m%d").img
