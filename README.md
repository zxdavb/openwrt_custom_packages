# OpenWrt Custom Feed

This is a custom feed for OpenWrt v18.06 packages.

## LXC v3.1.0
Currently, it contains only **LXC v3.1.0** (OpenWrt 18.06 has LXC v2.1.0)

To use it with your buildroot:
**a)** add the custom feed to you feeds.conf
**b)** use my feed's LXC instead of the 'offical' feed

One way to achieve the above may be via:
```
cp feeds.conf.default feeds.conf

cat >> feeds.conf << EOF
src-git customfeed https://github.com/zxdavb/openwrt_custom_packages.git
EOF

./scripts/feeds update  -a
./scripts/feeds install -a

./scripts/feeds uninstall lxc
./scripts/feeds install -p customfeed lxc

make defconfig  
```

Props to **@mgschweidl** (at gitlab.com), and others, on whose work this is based.

## .config file

Provided without explaination nor justification...

NB: I have not install `luci-app-lxc`, nor `lxc-lua`.

```bash
  echo CONFIG_PACKAGE_lxc=y                                         >> .config
  echo CONFIG_LXC_KERNEL_OPTIONS=y                                  >> .config  # Enable kernel support for LXC
  echo CONFIG_LXC_SECCOMP=y                                         >> .config  # is CONFIG_KERNEL_SECCOMP needed too?
  echo CONFIG_LXC_NETWORKING=y                                      >> .config  # Enable networking support for LXC containers
  echo CONFIG_LXC_BUSYBOX_OPTIONS=y                                 >> .config  # Enable busybox support for lxc-create tool

  cat << 'EOF' >> .config
CONFIG_KERNEL_CFS_BANDWIDTH=y
CONFIG_KERNEL_RESOURCE_COUNTERS=y
CONFIG_KERNEL_MEMCG_SWAP=y
CONFIG_RSYNC_xattr=y
CONFIG_USE_STRIP=y
EOF

  for APP in auto common configs hooks init templates unprivileged; do
    echo CONFIG_PACKAGE_lxc-${APP}=y                                >> .config
  done

  for APP in \
      attach copy create destroy info ls snapshot start stop \
      autostart cgroup checkconfig config console device execute freeze \
      monitor monitord top unfreeze unshare user-nic usernsexec wait
  do
    echo CONFIG_PACKAGE_lxc-${APP}=y                                >> .config
  done

  echo CONFIG_PACKAGE_gnupg=y                                       >> .config  # lxc-create with SSL support
  echo CONFIG_PACKAGE_gnupg-utils=y                                 >> .config  # lxc-create with SSL support
  echo CONFIG_BUSYBOX_CONFIG_MOUNTPOINT=y                           >> .config  # lxc-download uses mountpoint command

```

## Hacks needed or otherwise...

Provided without explaination nor justification...

```bash
    cat << 'EOF-ZX'  > ./files/etc/uci-defaults/zx-fix-lxc-cgroup
#!/bin/sh
  if ! grep -iq ZX /etc/init.d/lxc-auto; then
    cat >> /etc/init.d/lxc-auto <<EOF

### ZX: Create missing LXC resources (cgroup mount points)
boot() {
  if [ ! -d /sys/fs/cgroup/systemd ]; then
    echo "Creating systemd cgroup..."
    mkdir -p /sys/fs/cgroup/systemd
    mount -t cgroup -o rw,nosuid,nodev,noexec,relatime,none,name=systemd cgroup /sys/fs/cgroup/systemd
  fi

  start
}
EOF
  fi
EOF-ZX
```


