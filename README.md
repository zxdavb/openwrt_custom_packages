# openwrt_custom_packages

This is a custom feed for OpenWrt v18.06 packages.

Currently, it contains only LXC v3.1.0 (OpenWrt 18.06 has LXC v2.1.0)

To use it with your buildroot:
**a)** add the custom feed to you feeds.conf
**b)** use my feed's LXC instead of the 'offical' feed

One way to achieve the above may be via:
```
git clone https://github.com/zxdavb/openwrt_custom_packages

cp feeds.conf.default feeds.conf

cat >> feeds.conf << EOF
src-link customfeed /../../openwrt_custom_packages
EOF

./scripts/feeds update  -a
./scripts/feeds install -a
```
