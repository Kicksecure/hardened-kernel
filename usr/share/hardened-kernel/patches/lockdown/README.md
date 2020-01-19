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

# 0003-lockdown-acpi_rsdp:

Ignore the acpi_rsdp kernel parameter.

Not implemented yet.

> This option allows userspace to pass the RSDP address to the kernel, which
makes it possible for a user to modify the workings of hardware. Reject
the option when the kernel is locked down. This requires some reworking
of the existing RSDP command line logic, since the early boot code also
makes use of a command-line passed RSDP when locating the SRAT table
before the lockdown code has been initialised. This is achieved by
separating the command line RSDP path in the early boot code from the
generic RSDP path, and then copying the command line RSDP into boot
params in the kernel proper if lockdown is not enabled. If lockdown is
enabled and an RSDP is provided on the command line, this will only be
used when parsing SRAT (which shouldn't permit kernel code execution)
and will be ignored in the rest of the kernel.

# 0004-lockdown-pci-bar-access.patch:

Locks down PCI BAR access.

> Any hardware that can potentially generate DMA has to be locked down in
order to avoid it being possible for an attacker to modify kernel code,
allowing them to circumvent disabled module loading or module signing.
Default to paranoid - in future we can potentially relax this for
sufficiently IOMMU-isolated devices.

# 0005-lockdown-perf.patch:

Locks down perf.

The official lockdown patchset only locks down REGS_INTR but this patch
disables perf_event_open entirely to further reduce attack surface.

This will be dropped if perf_event_paranoid=4 support is merged into
linux-hardened.

> Disallow the use of certain perf facilities that might allow userspace to
access kernel data.

# 0006-lockdown-tiocsserial.patch

Locks down TIOCSSERIAL.

> Lock down TIOCSSERIAL as that can be used to change the ioport and irq
settings on a serial port.  This only appears to be an issue for the serial
drivers that use the core serial code.  All other drivers seem to either
ignore attempts to change port/irq or give an error.

# 0007-lockdown-ioport.patch:

Locks down IO port access (specifically, the ioperm() and iopl() syscalls).

> IO port access would permit users to gain access to PCI configuration
registers, which in turn (on a lot of hardware) give access to MMIO
register space. This would potentially permit root to trigger arbitrary
DMA, so lock it down by default.
>
> This also implicitly locks down the KDADDIO, KDDELIO, KDENABIO and
KDDISABIO console ioctls.

# 0008-lockdown-pcmcia.patch:

Locks down PCMCIA.

Only useful for the host kernel.

> Prohibit replacement of the PCMCIA Card Information Structure when the
kernel is locked down.

# 0009-lockdown-module-params.patch:

Locks down module parameters.

> Provided an annotation for module parameters that specify hardware
parameters (such as io ports, iomem addresses, irqs, dma channels, fixed
dma buffers and other types).
