# Hardened Kernel for Host and VMs #

This is a hardened kernel configuration for Whonix / Kicksecure.
hardened-vm-kernel is designed specifically for virtual machines and
hardened-host-kernel is designed for hosts.

Both configs try to have as many hardening options enabled as possible and
have little attack surface. hardened-vm-kernel only has support for VMs
and all other hardware options are disabled to reduce attack surface and
compile time.

During installation of hardened-vm-kernel, it compiles the kernel on your own
machine and does not use a pre-compiled kernel. This ensures the kernel
symbols in the compiled image are completely unique which makes it far harder
for kernel exploits. This is possible due to hardened-vm-kernel having only VM
config options enabled which drastically reduces compile time.

During installation of hardened-host-kernel, the kernel is not compiled on
your machine and it uses a pre-compiled kernel. This is because the host
kernel needs most hardware options enabled to support most devices which makes
compilation take a very long time.

The VM kernel is more secure than the host kernel due to having less attack
surface and not being pre-compiled but if you want more security for the host,
it is recommended to edit the hardened host config, enable only the hardware
options you need and compile the kernel yourself. This makes the security of
the host and VM kernel comparable.

Both configs were based on the default Debian config.

These kernels use the linux-hardened patch for further hardening. Custom
hardening patches should be
sent there.

This only supports LTS kernels as they have the least attack surface (stable
kernels have more code and more bugs) and the best stability.

Build script /usr/share/hardened-vm-kernel/build does not run automatic yet.

Kernel does not get installed automatic yet.

See also development discussion:
http://forums.whonix.org/t/kernel-recompilation-for-better-hardening
## How to install `hardened-kernel` using apt-get ##

1\. Download Whonix's Signing Key.

```
wget https://www.whonix.org/patrick.asc
```

Users can [check Whonix Signing Key](https://www.whonix.org/wiki/Whonix_Signing_Key) for better security.

2\. Add Whonix's signing key.

```
sudo apt-key --keyring /etc/apt/trusted.gpg.d/derivative.gpg add ~/patrick.asc
```

3\. Add Whonix's APT repository.

```
echo "deb https://deb.whonix.org bullseye main contrib non-free" | sudo tee /etc/apt/sources.list.d/derivative.list
```

4\. Update your package lists.

```
sudo apt-get update
```

5\. Install `hardened-kernel`.

```
sudo apt-get install hardened-kernel
```

## How to Build deb Package from Source Code ##

Can be build using standard Debian package build tools such as:

```
dpkg-buildpackage -b
```

See instructions. (Replace `generic-package` with the actual name of this package `hardened-kernel`.)

* **A)** [easy](https://www.whonix.org/wiki/Dev/Build_Documentation/generic-package/easy), _OR_
* **B)** [including verifying software signatures](https://www.whonix.org/wiki/Dev/Build_Documentation/generic-package)

## Contact ##

* [Free Forum Support](https://forums.whonix.org)
* [Professional Support](https://www.whonix.org/wiki/Professional_Support)

## Donate ##

`hardened-kernel` requires [donations](https://www.whonix.org/wiki/Donate) to stay alive!
