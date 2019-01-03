This is a custom feed of **LXC v3.1.0** for OpenWrt v18.06. I am not yet sure if it works, but to use it with your buildroot:
```
git clone https://github.com/zxdavb/openwrt_custom_packages

cp feeds.conf.default feeds.conf

cat >> feeds.conf << EOF
src-link customfeed /../../openwrt_custom_packages
EOF

./scripts/feeds update  -a
./scripts/feeds install -a

./scripts/feeds uninstall lxc
./scripts/feeds install -p customfeed lxc

make defconfig  
```

Props to **@mgschweidl** (at gitlab.com), and others, on whose work this is based.
