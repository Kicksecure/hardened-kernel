# Hardened VM Kernel #

Can recompile a hardened Linux kernel on the user's machine.

Currently only for VMs.

Maybe in future for host too.

Build script /usr/share/hardened-vm-kernel/build does not run automatic yet.

Kernel does not get installed automatic yet.

See also development discussion:
http://forums.whonix.org/t/kernel-recompilation-for-better-hardening
## How to install `hardened-kernel` using apt-get ##

1\. Download [Whonix's Signing Key]().

```
wget https://www.whonix.org/patrick.asc
```

Users can [check Whonix Signing Key](https://www.whonix.org/wiki/Whonix_Signing_Key) for better security.

2\. Add Whonix's signing key.

```
sudo apt-key --keyring /etc/apt/trusted.gpg.d/whonix.gpg add ~/patrick.asc
```

3\. Add Whonix's APT repository.

```
echo "deb https://deb.whonix.org buster main contrib non-free" | sudo tee /etc/apt/sources.list.d/whonix.list
```

4\. Update your package lists.

```
sudo apt-get update
```

5\. Install `hardened-kernel`.

```
sudo apt-get install hardened-kernel
```

## How to Build deb Package ##

Replace `apparmor-profile-torbrowser` with the actual name of this package with `hardened-kernel` and see [instructions](https://www.whonix.org/wiki/Dev/Build_Documentation/apparmor-profile-torbrowser).

## Contact ##

* [Free Forum Support](https://forums.whonix.org)
* [Professional Support](https://www.whonix.org/wiki/Professional_Support)

## Donate ##

`hardened-kernel` requires [donations](https://www.whonix.org/wiki/Donate) to stay alive!
