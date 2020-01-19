This patchset implements features of the lockdown LSM that we need as
lockdown is not available in LTS kernels.

Features that are already mitigated by our kernel configuration are not
implemented.

# 0001-lockdown-kconfig.patch:

Creates the CONFIG_SECURITY_LOCKDOWN kconfig setting to enable/disable
kernel lockdown.

# 0002-lockdown-efivar_ssdt_load.patch:

Disables efivar_ssdt_load.

Only useful for the host kernel.

> efivar_ssdt_load allows the kernel to import arbitrary ACPI code from an
EFI variable, which gives arbitrary code execution in ring 0. Prevent
that when the kernel is locked down.

# 0003-lockdown-pci-bar-access.patch:

Locks down PCI BAR access.

> Any hardware that can potentially generate DMA has to be locked down in
order to avoid it being possible for an attacker to modify kernel code,
allowing them to circumvent disabled module loading or module signing.
Default to paranoid - in future we can potentially relax this for
sufficiently IOMMU-isolated devices.

# 0004-lockdown-perf.patch:

Locks down perf.

The official lockdown patchset only locks down REGS_INTR but this patch
disables perf_event_open entirely to further reduce attack surface.

This will be dropped if perf_event_paranoid=4 support is merged into
linux-hardened.

> Disallow the use of certain perf facilities that might allow userspace to
access kernel data.

# 0005-lockdown-tiocsserial.patch

Locks down TIOCSSERIAL.

> Lock down TIOCSSERIAL as that can be used to change the ioport and irq
settings on a serial port.  This only appears to be an issue for the serial
drivers that use the core serial code.  All other drivers seem to either
ignore attempts to change port/irq or give an error.

# 0006-lockdown-ioport.patch:

Locks down IO port access (specifically, the ioperm() and iopl() syscalls).

> IO port access would permit users to gain access to PCI configuration
registers, which in turn (on a lot of hardware) give access to MMIO
register space. This would potentially permit root to trigger arbitrary
DMA, so lock it down by default.
>
> This also implicitly locks down the KDADDIO, KDDELIO, KDENABIO and
KDDISABIO console ioctls.

# 0007-lockdown-pcmcia.patch:

Locks down PCMCIA.

Only useful for the host kernel.

> Prohibit replacement of the PCMCIA Card Information Structure when the
kernel is locked down.

# 0008-lockdown-module-params.patch:

Locks down module parameters.

> Provided an annotation for module parameters that specify hardware
parameters (such as io ports, iomem addresses, irqs, dma channels, fixed
dma buffers and other types).
