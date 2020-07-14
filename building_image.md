repo init -m icefish-9.0.0.xml -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo -b sandbox/walzert/can_build

source meta-agl/scripts/aglsetup.sh -m qemux86-64 agl-demo agl-devel

repo init  -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo -b sandbox/walzert/can_build
repo sync
source meta-agl/scripts/aglsetup.sh -m qemux86-64 agl-demo agl-devel agl-profile-graphical-html5 agl-profile-graphical

