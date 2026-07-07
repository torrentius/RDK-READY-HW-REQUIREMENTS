# RDK-READY-HW-REQUIREMENTS
# RDK-B SoC Reference Platform Requirements

## 1. Introduction

## 2. Compliance Model

The RDK-B SoC Reference Platform Requirements define the minimum technical criteria that a System-on-Chip (SoC) platform SHALL satisfy to be considered compatible with the RDK-B Reference Platform.

Each requirement in this specification is uniquely identified and classified according to its compliance level, implementation priority, ownership, and verification method.

---

### RDK-SOC-COMP-001: Compliance Levels

**Requirement**

Every platform SHALL be evaluated against all requirements defined in this specification.

Requirements are classified using the following compliance levels:

| Level | Description |
|--------|-------------|
| **Mandatory** | The requirement SHALL be implemented. Non-compliance prevents certification as an RDK-B Reference Platform. |
| **Recommended** | The requirement SHOULD be implemented. Non-compliance does not prevent certification but may reduce interoperability, portability, or maintainability. |
| **Optional** | The requirement MAY be implemented where applicable to a specific hardware platform or deployment model. |

Platform compliance SHALL be determined by evaluating all Mandatory requirements together with any Recommended or Optional requirements applicable to the target platform.

**Rationale**

Using standardized compliance levels enables consistent evaluation across SoC vendors while allowing flexibility for optional platform capabilities.

---

### RDK-SOC-COMP-002: Mandatory vs Recommended Requirements

**Requirement**

Each requirement in this specification SHALL explicitly identify its implementation priority.

Mandatory requirements define the minimum capabilities required for an RDK-B SoC Reference Platform.

Recommended requirements identify industry best practices that improve portability, interoperability, maintainability, security, or long-term upstream compatibility.

Optional requirements describe capabilities that are only applicable to specific platform configurations or deployment scenarios.

Implementations SHALL NOT replace a Mandatory requirement with an alternative proprietary mechanism unless explicitly permitted by this specification.

**Rationale**

Separating mandatory and recommended requirements allows vendors to prioritize implementation effort while maintaining a common baseline for RDK-B compatibility.

---

### RDK-SOC-COMP-003: Evidence Requirements

**Requirement**

Each requirement SHALL define an objective verification method and the evidence required to demonstrate compliance.

Compliance evidence SHOULD consist of objective artifacts whenever possible.

Examples include:

- Boot logs
- Kernel logs (`dmesg`)
- Device Tree validation (`dtbs_check`)
- SystemReady certification reports
- Linux kernel configuration
- Firmware version information
- Yocto build logs
- RDK-B validation reports
- Functional test results
- CI/CD pipeline artifacts
- Static analysis reports
- Security validation reports

Every requirement SHALL identify:

- Requirement ID
- Compliance level
- Owner
- Verification method
- Required evidence

The compliance status of each requirement SHALL be recorded as one of:

- Not Started
- In Progress
- Compliant
- Non-Compliant
- Waived (with justification)

**Rationale**

Objective evidence provides traceability, simplifies vendor onboarding, and enables repeatable compliance assessments across multiple SoC platforms and RDK-B releases.

### RDK-SOC-COMP-004: Requirement Traceability

Every requirement SHALL have a unique identifier and SHALL be traceable to one or more of the following:

- Arm SystemReady
- EBBR
- Arm BBR
- Linux Kernel
- Device Tree Specification
- UEFI
- Yocto Project
- RDK-B
- TR-181
- Internal RDK Architecture Requirements
When a requirement extends an external specification, the originating specification and section SHALL be referenced.

---
## 3. Boot and Firmware Requirements

The boot firmware establishes the contract between the hardware platform and the operating system. The platform SHALL implement industry-standard boot interfaces to ensure portability, interoperability, secure boot, and long-term maintainability.

---

### RDK-SOC-BOOT-001: EBBR UEFI Compliance

**Requirement**

The platform SHALL implement the mandatory UEFI requirements defined by the Embedded Base Boot Requirements (EBBR) specification.

**Priority**

Mandatory

**Rationale**

Compliance with EBBR provides a standardized firmware interface, enabling the same operating system image to boot across multiple hardware platforms without platform-specific modifications.

**Verification**

- Execute Arm SystemReady EBBR compliance tests.
- Verify successful boot of the RDK-B reference image.

**Evidence**

- EBBR compliance report
- Boot log
- Firmware version

**References**

- EBBR Section 2

---

### RDK-SOC-BOOT-002: UEFI Secure Boot

**Requirement**

The platform SHALL support UEFI Secure Boot, including secure management of platform key databases for certificate enrollment and revocation.

**Priority**

Mandatory

**Rationale**

UEFI Secure Boot establishes a trusted execution chain by ensuring that only authenticated firmware and operating system components are executed.

**Verification**

- Execute Secure Boot validation tests.
- Verify certificate enrollment and revocation.

**Evidence**

- Secure Boot test report
- Firmware configuration
- Boot log

**References**

- UEFI Specification
- Arm SystemReady Security Tests

---

### RDK-SOC-BOOT-003: EFI Capsule Firmware Updates

**Requirement**

The platform SHALL support secure firmware updates using the UEFI EFI Capsule Update mechanism.

The implementation SHALL support updates of firmware components such as:

- TF-A
- BL31
- U-Boot
- Platform firmware

without requiring proprietary update mechanisms.

**Priority**

Mandatory

**Rationale**

EFI Capsules provide a standard firmware update mechanism supported across Linux distributions.

**Verification**

- Execute EFI Capsule update tests.
- Verify firmware version changes after update.

**Evidence**

- Firmware update log
- Capsule update report

**References**

- Arm BBR
- EFI Update Capsule

---

### RDK-SOC-BOOT-004: Arm PSCI Support

**Requirement**

The platform SHALL implement the Arm Power State Coordination Interface (PSCI) to provide standardized CPU power management services.

**Priority**

Mandatory

**Rationale**

PSCI enables Linux to manage CPU bring-up, shutdown, suspend, reboot, and power-off operations without vendor-specific firmware interfaces.

**Verification**

- Verify Linux detects PSCI.
- Validate reboot and power management operations.

**Evidence**

- Kernel boot log
- PSCI detection output

**References**

- Arm PSCI
- EBBR

---

### RDK-SOC-BOOT-005: Reserved Memory Exposed Through UEFI

**Requirement**

Any memory reserved for firmware, secure world, co-processors, or other platform components SHALL be described through the UEFI memory map.

Reserved memory SHALL NOT rely solely on Device Tree reserved-memory nodes when booting through UEFI.

**Priority**

Mandatory

**Rationale**

Linux obtains the authoritative memory map from UEFI during boot. Reserved memory must therefore be represented consistently within the firmware interface.

**Verification**

- Inspect UEFI memory map.
- Validate Linux reserved memory regions.

**Evidence**

- UEFI memory map
- Kernel boot log

**References**

- Arm BBR

---

### RDK-SOC-BOOT-006: EFI Runtime Variables

**Requirement**

The platform SHOULD provide EFI Runtime Variable support.

Where persistent storage is available, runtime variables SHOULD survive system reboot.

**Priority**

Recommended

**Rationale**

EFI Runtime Variables provide standardized storage for boot configuration, Secure Boot databases, and firmware state.

**Verification**

- Read and modify EFI runtime variables.
- Verify persistence across reboot.

**Evidence**

- efivar output
- Runtime variable test report

**References**

- EBBR

---
### RDK-SOC-BOOT-007: Independent Firmware Component Updates

**Requirement**

The firmware architecture SHALL support independent updates of firmware components wherever technically feasible.

Updating one firmware component (for example, TF-A, OP-TEE, U-Boot, or equivalent runtime firmware) SHALL NOT require updating unrelated boot stages unless mandated by the platform architecture.

**Priority**

Mandatory

**Rationale**

Supporting independent firmware updates reduces deployment risk, minimizes unnecessary firmware replacement, and simplifies security updates throughout the platform lifecycle.

**Verification**

- Perform an update of an individual firmware component.
- Verify successful platform boot and operation without modifying unrelated firmware stages.

**Evidence**

- Firmware version information
- Update logs
- Boot logs
---

### RDK-SOC-BOOT-008: eMMC Boot Partitions

**Requirement**

Platforms using eMMC as the primary boot device SHALL store boot firmware within the standard eMMC Boot Partitions.

The platform SHALL support standard eMMC boot partition management.

**Priority**

Mandatory

**Rationale**

Using standardized eMMC boot partitions improves compatibility across boot firmware implementations.

**Verification**

- Verify boot partition configuration.
- Validate boot firmware location.

**Evidence**

- eMMC partition table
- Firmware image layout

**References**

- Arm SystemReady
- JEDEC eMMC Specification

---

### RDK-SOC-BOOT-009: Dual-Bank / Fallback Boot

**Requirement**

Platforms booting from flash memory SHALL support either:

- Dual-bank firmware updates, or
- Automatic fallback to an alternate boot source.

Fallback SHALL permit recovery from interrupted or failed firmware updates.

**Priority**

Mandatory

**Rationale**

A resilient boot architecture reduces the risk of rendering devices unbootable following firmware update failures.

**Verification**

- Simulate interrupted firmware update.
- Verify successful recovery.

**Evidence**

- Firmware recovery test report
- Boot log

---

### RDK-SOC-BOOT-010: SSD Boot Support

**Requirement**

The platform SHOULD support booting Linux from standard high-performance storage devices, including:

- NVMe
- SATA
- USB 3.x Mass Storage

where supported by the hardware design.

**Priority**

Recommended

**Rationale**

Supporting SSD boot improves developer productivity, reduces wear on embedded flash devices, and simplifies deployment of development and validation environments.

**Verification**

- Boot the RDK-B reference image from supported storage.
- Verify normal system operation.

**Evidence**

- Boot log
- Storage enumeration
- Functional validation report

**References**

- Arm SystemReady
---

## 4. Platform Security Requirements

The platform SHALL implement industry-standard security mechanisms to establish a trusted execution environment, protect sensitive assets, and ensure the integrity and authenticity of firmware and software throughout the device lifecycle.

---

### RDK-SOC-SEC-001: TPM2 or fTPM

**Requirement**

The platform SHALL provide Trusted Platform Module (TPM) version 2.0 capabilities, either through:

- A discrete hardware TPM, or
- A firmware TPM (fTPM) executing within a Trusted Execution Environment such as OP-TEE.

The TPM SHALL be accessible through standard Linux TPM interfaces.

**Priority**

Mandatory

**Rationale**

TPM2 provides standardized services for secure key storage, cryptographic operations, attestation, and secure measurements without requiring vendor-specific implementations.

**Verification**

- Verify Linux detects the TPM.
- Execute standard TPM2 commands.
- Validate persistent key storage.

**Evidence**

- `tpm2-tools` output
- Kernel boot log
- TPM functional test report

**References**

- TPM 2.0 Specification
- Arm SystemReady

---

### RDK-SOC-SEC-002: OP-TEE Support

**Requirement**

The platform SHALL support OP-TEE, or an equivalent standards-based Trusted Execution Environment (TEE), capable of executing trusted applications independently from the Rich Execution Environment.

The TEE SHALL support:

- Secure monitor integration
- Trusted Applications (TAs)
- Secure storage services
- Standard Linux OP-TEE driver interfaces

**Priority**

Mandatory

**Rationale**

A Trusted Execution Environment provides hardware-isolated execution for security-sensitive services while maintaining interoperability through standard interfaces.

**Verification**

- Verify OP-TEE boots successfully.
- Validate communication through the Linux OP-TEE driver.
- Execute Trusted Application tests.

**Evidence**

- OP-TEE boot log
- Kernel log
- Trusted Application test results

**References**

- OP-TEE
- Arm Trusted Firmware

---

### RDK-SOC-SEC-003: Secure Storage

**Requirement**

The platform SHALL provide secure persistent storage for confidential platform assets.

Secure storage SHALL support protection of:

- Cryptographic keys
- Certificates
- Credentials
- Secure configuration data

Secure storage SHOULD be implemented using hardware-backed protection mechanisms where available.

**Priority**

Mandatory

**Rationale**

Protecting sensitive assets prevents unauthorized extraction or modification of credentials and cryptographic material.

**Verification**

- Store and retrieve secure objects.
- Validate access restrictions.
- Confirm persistence across reboot.

**Evidence**

- Secure storage test report
- Functional validation logs

**References**

- OP-TEE Secure Storage
- Arm SystemReady

---
### RDK-SOC-SEC-004: Trusted Time or Monotonic Counter

**Requirement**

The platform SHOULD provide a mechanism to detect or prevent rollback of security-sensitive time or counters.

Suitable implementations MAY include:

- Battery-backed RTC
- Secure RTC
- Secure monotonic counter
- TPM-backed monotonic counter
- Replay Protected Memory Block (RPMB)
- Other equivalent secure time mechanism

Where certificate validity, firmware update policies, or secure storage depend on trusted time, the implementation SHOULD ensure that rollback attacks cannot invalidate those security mechanisms.

**Priority**

Recommended

**Rationale**

Protecting trusted time or monotonic counters helps prevent replay attacks, firmware rollback, and misuse of expired credentials while allowing platforms to select an implementation appropriate for their hardware capabilities.

**Verification**

- Verify the trusted time or counter mechanism.
- Attempt rollback scenarios where supported.
- Confirm replay protection mechanisms operate correctly.

**Evidence**

- Platform security architecture
- Validation report
- Secure storage test results

**References**

- Arm Platform Secure Boot Guide

---

### RDK-SOC-SEC-005: Arm Chain of Trust

**Requirement**

The platform SHALL implement the Arm Chain of Trust (CoT) principles for firmware authentication.

Each boot stage SHALL authenticate the next stage before execution using cryptographic signatures and X.509 certificates or equivalent mechanisms.

The Root of Trust SHALL originate from immutable hardware or equivalent trusted platform mechanisms.

**Priority**

Mandatory

**Rationale**

The Chain of Trust ensures that only authenticated firmware components are executed throughout the secure boot process.

**Verification**

- Validate each firmware stage.
- Verify signature validation.
- Confirm failure on modified firmware.

**Evidence**

- Secure boot report
- Firmware signing configuration
- Boot logs

**References**

- Arm Platform Secure Boot Guide
- Arm Trusted Firmware

---

### RDK-SOC-SEC-006: Secure Boot Validation

**Requirement**

The platform SHALL successfully complete Secure Boot validation using the applicable Arm SystemReady Security test suite.

Validation SHALL confirm:

- Secure Boot is enabled.
- Unsigned firmware is rejected.
- Modified firmware is rejected.
- Certificate enrollment and revocation operate correctly.
- Boot measurements complete successfully where applicable.

**Priority**

Mandatory

**Rationale**

Conformance testing provides objective evidence that Secure Boot has been correctly implemented according to industry standards.

**Verification**

- Execute Arm SystemReady Secure Boot tests.
- Execute negative boot tests using invalid firmware images.

**Evidence**

- Security certification report
- Test logs
- Boot validation report

**References**

- Arm SystemReady Security Interface Testing
- UEFI Secure Boot Specification

---

## 5. Device Tree Requirements

The Device Tree defines the contract between platform firmware and the operating system. To maximize portability, interoperability, and long-term maintainability, platforms SHALL use upstream Linux Device Tree bindings and descriptions wherever possible.

The Device Tree provided with the platform SHALL accurately describe all hardware components that are visible to the operating system.

---

### RDK-SOC-DT-001: Shipping DTS/DTSI Sources

**Requirement**

The SoC vendor SHALL provide the complete Device Tree source files (DTS/DTSI) used to generate the shipping Device Tree Blob (DTB).

The delivered sources SHALL be sufficient to reproduce the DTB included with the platform firmware.

**Priority**

Mandatory

**Rationale**

Providing the original Device Tree sources enables reproducible builds, simplifies debugging, and facilitates long-term platform maintenance.

**Verification**

- Compare the generated DTB against the shipping DTB.
- Verify successful DTB generation from the provided sources.

**Evidence**

- DTS/DTSI source repository
- Shipping DTB
- Build instructions

---

### RDK-SOC-DT-002: Device Tree Schema Validation

**Requirement**

The shipping Device Tree SHALL successfully pass upstream Linux Device Tree schema validation for all operating system visible nodes.

Validation SHALL be performed using:

```bash
make dtbs_check
```

Validation errors SHALL be corrected prior to platform acceptance.

Documented waivers MAY be accepted for known upstream limitations.

**Priority**

Mandatory

**Rationale**

Schema validation verifies that the Device Tree conforms to upstream Linux bindings and prevents incompatible firmware/kernel interfaces.

**Verification**

- Execute `make dtbs_check`.
- Review validation report.

**Evidence**

- dtbs_check report
- Waiver list (if applicable)

---

### RDK-SOC-DT-003: Shared Linux/U-Boot Device Tree

**Requirement**

The Device Tree used by Linux and U-Boot SHALL be derived from the same common source.

U-Boot specific modifications SHALL be isolated within dedicated `*-u-boot.dtsi` files.

Forking or independently maintaining separate Linux and U-Boot Device Trees SHALL NOT be permitted.

**Priority**

Mandatory

**Rationale**

Maintaining a single Device Tree source eliminates duplication and reduces long-term maintenance effort.

**Verification**

- Compare Linux and U-Boot Device Trees.
- Review source hierarchy.

**Evidence**

- DTS source tree
- U-Boot DTS source

---

### RDK-SOC-DT-004: Upstream Device Tree Roadmap

**Requirement**

The SoC vendor SHALL provide an upstreaming roadmap for the Device Tree.

At minimum, the roadmap SHALL identify:

- Current upstream status
- Planned Linux kernel release
- Outstanding dependencies
- Expected completion milestone

Platforms integrated into RDK-B SHOULD achieve upstream Device Tree support by the next Yocto LTS release.

**Priority**

Recommended

**Rationale**

Maintaining an upstream roadmap minimizes long-term maintenance and aligns the platform with the Linux ecosystem.

**Verification**

- Review upstream roadmap.
- Verify upstream submissions.

**Evidence**

- Roadmap document
- Upstream patch series
- Linux mailing list references

---

### RDK-SOC-DT-005: Upstream Device Tree Bindings

**Requirement**

All operating system visible hardware SHALL use upstream Linux Device Tree bindings.

New hardware components SHALL introduce new Device Tree bindings only through the upstream Linux Device Tree binding review process.

Vendor-specific bindings SHALL NOT replace existing upstream bindings.

**Priority**

Mandatory

**Rationale**

Using upstream bindings ensures interoperability with the Linux kernel, enables automatic schema validation, and minimizes platform-specific maintenance.

**Verification**

- Verify all compatible strings correspond to upstream YAML bindings.
- Execute `make dtbs_check`.
- Review any vendor-specific bindings.

**Evidence**

- YAML binding documentation
- dtbs_check report
- Upstream references

---

### RDK-SOC-DT-006: Broadband-Specific Binding Exceptions

**Requirement**

Vendor-specific Device Tree bindings MAY be used only for broadband-specific hardware blocks for which no suitable upstream Linux binding exists.

Examples include:

- DOCSIS
- OMCI / PON
- MoCA
- xDSL

Such bindings SHALL:

- Be fully documented.
- Follow Linux Device Tree binding conventions.
- Be reviewed during platform onboarding.
- Not replace existing upstream bindings.

**Priority**

Mandatory

**Rationale**

Broadband-specific hardware may require platform-specific descriptions until standardized upstream bindings become available.

**Verification**

- Review proprietary bindings.
- Confirm no equivalent upstream binding exists.
- Verify documentation.

**Evidence**

- Binding documentation
- Platform architecture documentation

---

### RDK-SOC-DT-007: Mandatory Upstream Bindings for Standard Linux Subsystems

**Requirement**

The following hardware subsystems SHALL use upstream Linux Device Tree bindings and SHALL NOT use vendor-specific alternatives:

- CPU
- GPU
- NPU / AI accelerators
- PCI Express
- USB
- Ethernet
- Wi-Fi
- Bluetooth
- GPIO
- LEDs (`gpio-leds`)
- Buttons (`gpio-keys`)
- UART
- SPI
- I²C
- PWM
- DMA controllers
- Clocks
- Reset controllers
- Interrupt controllers
- Thermal sensors
- Watchdogs
- MMC / SD / eMMC
- NVMe
- SATA
- IOMMU / SMMU

Vendor-specific Device Tree bindings SHALL NOT replace these standard Linux subsystem bindings.

**Priority**

Mandatory

**Rationale**

Standard Linux subsystems already define stable Device Tree bindings. Reusing these bindings maximizes portability across SoCs, reduces BSP complexity, and ensures compatibility with upstream Linux.

**Verification**

- Review all compatible strings.
- Execute Device Tree schema validation.
- Confirm use of upstream subsystem bindings.

**Evidence**

- DTS/DTSI source
- YAML binding references
- dtbs_check validation report

---

## 6. Linux Kernel Requirements

The Linux kernel provides the primary hardware abstraction layer for the RDK-B software stack. Platforms SHALL maximize compatibility with upstream Linux and minimize vendor-specific kernel modifications to improve portability, maintainability, and long-term support.

---

### RDK-SOC-KERNEL-001: Mainline Linux Roadmap

**Requirement**

The SoC vendor SHALL provide a roadmap for achieving full upstream Linux kernel support.

The roadmap SHALL identify:

- Current upstream support status.
- Components already accepted into the Linux kernel.
- Outstanding kernel drivers awaiting upstream acceptance.
- Planned Linux kernel release for full upstream support.

Platforms integrated into RDK-B SHOULD achieve full upstream kernel support by the next Yocto LTS release following platform introduction.

**Priority**

Recommended

**Rationale**

Maintaining a clear upstream roadmap reduces BSP maintenance costs and simplifies long-term platform support.

**Verification**

- Review upstream roadmap.
- Verify upstream patch submissions.
- Review Linux kernel support status.

**Evidence**

- Upstream roadmap
- Patch tracking
- Linux kernel version support matrix

---

### RDK-SOC-KERNEL-002: Standard Linux Drivers

**Requirement**

All operating system visible hardware SHALL use standard Linux kernel subsystems and drivers whenever available.

Vendor-specific kernel drivers SHALL NOT replace existing upstream Linux drivers for standard hardware components.

This requirement applies to, but is not limited to:

- PCI Express
- USB
- Ethernet
- Wi-Fi
- Bluetooth
- GPIO
- UART
- SPI
- I²C
- PWM
- MMC
- NVMe
- SATA
- Thermal
- Watchdog
- DMA
- RTC
- IOMMU / SMMU

Vendor-specific drivers MAY be used only when:

- No upstream driver exists.
- The hardware implements broadband-specific functionality (e.g. DOCSIS, PON, MoCA, xDSL).
- An upstreaming plan is provided.

**Priority**

Mandatory

**Rationale**

Using standard Linux drivers maximizes portability, simplifies maintenance, and benefits from continuous improvements made by the upstream Linux community.

**Verification**

- Review kernel configuration.
- Verify loaded kernel modules.
- Review vendor patch set.

**Evidence**

- Kernel configuration
- Loaded module list
- BSP patch list

---

### RDK-SOC-KERNEL-003: Standard Kernel Configuration

**Requirement**

The platform SHALL build using the common RDK-B Linux kernel baseline.

Platform-specific kernel configuration SHALL be minimized.

Kernel configuration SHALL enable required upstream Linux subsystems instead of introducing vendor-specific functionality whenever possible.

Platform-specific configuration SHALL be documented.

**Priority**

Mandatory

**Rationale**

Maintaining a common kernel configuration simplifies maintenance across multiple SoCs and improves compatibility between RDK-B releases.

**Verification**

- Compare platform configuration against the RDK-B reference configuration.
- Review configuration differences.

**Evidence**

- Kernel `.config`
- Configuration diff report

---

### RDK-SOC-KERNEL-004: Kernel Configuration Fragments

**Requirement**

Platform-specific kernel configuration SHALL be implemented using kernel configuration fragments rather than complete platform-specific kernel configuration files.

Configuration fragments SHALL contain only the options necessary to support the target platform.

The common RDK-B kernel configuration SHALL remain reusable across supported platforms.

**Priority**

Mandatory

**Rationale**

Kernel configuration fragments simplify maintenance by separating platform-specific configuration from the common kernel baseline.

**Verification**

- Review kernel configuration fragments.
- Validate successful kernel build using configuration merging.

**Evidence**

- Kernel fragment files
- Build logs

---

### RDK-SOC-KERNEL-005: Standard Linux Interfaces

**Requirement**

Platform functionality SHALL be exposed through standard Linux kernel interfaces.

Vendor-specific user-space APIs SHALL NOT replace existing Linux interfaces for standard platform functionality.

Examples include:

- procfs
- sysfs
- configfs
- netlink
- rtnetlink
- ethtool
- libgpiod
- V4L2
- DRM/KMS
- ALSA
- TPM subsystem
- IIO subsystem
- hwmon
- thermal framework
- LED framework
- input subsystem
- regulator framework
- watchdog subsystem
- Industrial I/O framework

Broadband-specific functionality MAY expose additional interfaces where no equivalent Linux subsystem exists.

**Priority**

Mandatory

**Rationale**

Using standard Linux interfaces enables portable userspace software, simplifies debugging, and reduces dependence on vendor-specific APIs.

**Verification**

- Review user-space interfaces.
- Verify compliance with standard Linux frameworks.
- Confirm vendor APIs are limited to broadband-specific functionality.

**Evidence**

- Platform architecture documentation
- API documentation
- Functional validation reports

---

## 7. Hardware Platform Requirements

The hardware platform SHALL implement standardized Arm architecture components to maximize compatibility with upstream Linux, simplify platform integration, and minimize platform-specific software.

---

### RDK-SOC-HW-001: BSA/SBSA Compliance

**Requirement**

New platform designs SHOULD comply with the applicable Arm Base System Architecture (BSA) and Server Base System Architecture (SBSA) requirements appropriate for embedded platforms.

The SoC SHALL implement standard Arm architectural components, including processors, interrupt controllers, timers, MMU, SMMU (where applicable), and other architectural IP blocks in accordance with Arm recommendations.

**Priority**

Recommended

**Rationale**

Compliance with Arm architectural standards improves operating system portability and reduces platform-specific software dependencies.

**Verification**

- Review SoC architecture.
- Execute applicable Arm SystemReady validation tests.

**Evidence**

- SoC architecture documentation
- SystemReady validation report

**References**

- Arm BSA
- Arm SBSA
- Arm SystemReady

---

### RDK-SOC-HW-002: Standard Interrupt Controller (GICv3+)

**Requirement**

The platform SHALL implement the Arm Generic Interrupt Controller (GIC) version 3 or later.

The interrupt controller SHALL use the standard upstream Linux GIC driver and Device Tree bindings.

Vendor-specific interrupt controller implementations SHALL NOT replace the Arm GIC architecture.

**Priority**

Mandatory

**Rationale**

The Arm Generic Interrupt Controller provides a standard interrupt architecture for Linux, enabling SMP support, interrupt routing, virtualization, and portability across Arm platforms.

**Verification**

- Verify Linux detects GICv3 or later.
- Confirm the standard Linux GIC driver is used.
- Validate interrupt delivery across all CPU cores.

**Evidence**

- Device Tree
- Kernel boot log
- `/proc/interrupts`

**References**

- Arm GIC Architecture Specification

---

### RDK-SOC-HW-003: Standard Console UART

**Requirement**

The platform SHALL provide a system console using one of the standard UART implementations supported by Linux:

- 16550 compatible UART
- Arm PL011 UART
- Arm Generic UART

Firmware SHALL expose the console using standard UEFI console services and/or Device Tree.

The Linux kernel SHALL NOT require platform-specific kernel command line parameters to locate the primary console.

**Priority**

Mandatory

**Rationale**

Using standard UART implementations enables early boot debugging, firmware interoperability, and consistent Linux console support.

**Verification**

- Verify Linux console availability.
- Confirm firmware exposes the console correctly.
- Validate serial console operation.

**Evidence**

- Device Tree
- Boot log
- Serial console output

**References**

- Arm SystemReady
- EBBR

---

### RDK-SOC-HW-004: PCI Message Signaled Interrupts (MSI)

**Requirement**

The platform SHOULD support PCI Express Message Signaled Interrupts (MSI/MSI-X).

Where multiple CPU cores are present, interrupts SHOULD be distributable across CPUs using the standard Linux interrupt framework.

**Priority**

Recommended

**Rationale**

MSI improves interrupt performance, scalability, and multicore load balancing for high-speed PCIe devices such as NVMe SSDs, Wi-Fi adapters, and Ethernet controllers.

**Verification**

- Verify MSI/MSI-X operation.
- Review interrupt distribution.

**Evidence**

- `/proc/interrupts`
- PCIe device driver logs
- Kernel boot log

---

### RDK-SOC-HW-005: Standard System Memory Management Unit (SMMU)

**Requirement**

Platforms implementing DMA-capable peripherals SHALL implement the Arm System Memory Management Unit (SMMU) using the standard Linux SMMU driver.

Firmware SHALL correctly configure Stream IDs, Context Banks, and IOMMU groups for all DMA-capable devices.

Vendor-specific IOMMU implementations SHALL NOT replace the standard Arm SMMU architecture.

**Priority**

Mandatory

**Rationale**

The SMMU provides DMA isolation, improves platform security, and enables virtualization using standard Linux kernel support.

**Verification**

- Verify Linux detects the SMMU.
- Validate IOMMU groups.
- Execute DMA isolation tests.

**Evidence**

- Device Tree
- `/sys/kernel/iommu_groups`
- Kernel boot log

**References**

- Arm SMMU Architecture Specification

---

### RDK-SOC-HW-006: Shared-Memory Coprocessor Support

**Requirement**

Any coprocessor sharing system memory with the application processors (for example, Network Processing Units, Packet Processing Engines, or DSPs) SHALL operate correctly through the standard Arm SMMU when DMA protection is enabled.

The implementation SHALL NOT require disabling SMMU protection or enabling bypass mode to achieve normal operation.

Memory shared between Linux and coprocessors SHALL be explicitly described through standard firmware interfaces.

**Priority**

Mandatory

**Rationale**

Shared-memory coprocessors must coexist with Linux while preserving DMA isolation, memory protection, and system security.

**Verification**

- Validate coprocessor operation with SMMU enabled.
- Confirm no SMMU bypass is required.
- Verify shared memory regions.

**Evidence**

- Device Tree
- Kernel log
- Coprocessor functional validation
- SMMU configuration report

**References**

- Arm SystemReady
- Arm SMMU Architecture Specification

---

## 8. PCI Express Requirements

The platform SHALL implement PCI Express using standard Linux kernel subsystems and firmware interfaces to ensure interoperability with commercially available PCIe devices, including storage, networking, wireless, and accelerator peripherals.

---

### RDK-SOC-PCIE-001: Standard PCIe Drivers

**Requirement**

The platform SHALL support standard Linux PCI Express drivers without requiring platform-specific modifications.

Standard PCIe devices SHALL operate using upstream Linux drivers, including but not limited to:

- NVMe SSDs
- Ethernet controllers
- Wi-Fi adapters
- Bluetooth controllers
- Cellular modems
- USB controllers

Vendor-specific kernel drivers SHALL NOT replace existing upstream Linux PCIe drivers.

**Priority**

Mandatory

**Rationale**

Using upstream Linux PCIe drivers maximizes hardware interoperability and minimizes BSP maintenance.

**Verification**

- Enumerate PCIe devices.
- Verify standard Linux drivers are loaded.
- Execute functional tests for supported PCIe devices.

**Evidence**

- `lspci`
- Kernel boot log
- Loaded kernel modules
- Functional validation report

---

### RDK-SOC-PCIE-002: PCIe IOMMU Isolation

**Requirement**

All PCIe devices capable of DMA SHALL be protected by the Arm System Memory Management Unit (SMMU).

Firmware SHALL correctly configure:

- Stream IDs
- Context Banks
- IOMMU Groups

The platform SHALL NOT require SMMU bypass mode for normal PCIe operation.

**Priority**

Mandatory

**Rationale**

IOMMU isolation prevents unauthorized DMA access and improves platform security and virtualization support.

**Verification**

- Verify Linux detects the SMMU.
- Review IOMMU group assignments.
- Validate DMA isolation.

**Evidence**

- `/sys/kernel/iommu_groups`
- Kernel boot log
- PCIe functional tests

---

### RDK-SOC-PCIE-003: Message Signaled Interrupts (MSI/MSI-X)

**Requirement**

The platform SHOULD support PCI Express Message Signaled Interrupts (MSI and MSI-X).

Where multiple CPU cores are available, interrupt distribution SHOULD support multicore load balancing using the standard Linux interrupt framework.

**Priority**

Recommended

**Rationale**

MSI/MSI-X improves interrupt performance and scalability while reducing interrupt sharing.

**Verification**

- Verify MSI/MSI-X operation.
- Review interrupt distribution.

**Evidence**

- `/proc/interrupts`
- Kernel logs
- PCIe driver diagnostics

---

### RDK-SOC-PCIE-004: Generic PCIe Enumeration

**Requirement**

Firmware SHALL enumerate PCI Express devices using standard PCI Express discovery mechanisms.

The firmware SHALL NOT contain hardcoded assumptions about specific PCIe endpoint devices.

Any standards-compliant PCIe endpoint supported by Linux SHOULD enumerate without firmware modifications.

**Priority**

Mandatory

**Rationale**

Generic PCIe enumeration enables platform extensibility and allows users to install commercially available PCIe devices without firmware customization.

**Verification**

- Install multiple PCIe devices.
- Verify successful enumeration.
- Confirm Linux detects devices without firmware changes.

**Evidence**

- `lspci`
- Kernel boot log
- Device enumeration report

---

### RDK-SOC-PCIE-005: PCIe Hot-Plug

**Requirement**

Platforms that expose externally accessible PCI Express slots SHOULD support standard PCIe hot-plug where supported by the hardware architecture.

Hot-plug events SHALL use the standard Linux PCI hot-plug framework.

Platforms without externally accessible PCIe connectors are exempt from this requirement.

**Priority**

Optional

**Rationale**

Hot-plug support improves serviceability and enables field replacement or expansion of supported PCIe devices.

**Verification**

- Insert and remove supported PCIe devices.
- Verify Linux detects hot-plug events.
- Validate device initialization and removal.

**Evidence**

- Kernel logs
- PCIe hot-plug test report

---

## 9. Storage Requirements

The platform SHALL implement storage devices and storage management using standard Linux kernel subsystems and interfaces to maximize interoperability, portability, and maintainability.

---

### RDK-SOC-STOR-001: Standard Linux Block Devices

**Requirement**

The platform SHALL expose all mass storage devices through the standard Linux block device subsystem.

Supported storage technologies MAY include:

- eMMC
- NVMe
- SATA
- USB Mass Storage
- SD Card

Vendor-specific storage drivers SHALL integrate with the Linux block layer and SHALL NOT expose proprietary userspace interfaces.

**Priority**

Mandatory

**Rationale**

The Linux block layer provides a common abstraction for all storage devices, enabling portable storage management across different hardware platforms.

**Verification**

- Verify block devices are enumerated by Linux.
- Confirm standard Linux block interfaces are used.

**Evidence**

- `lsblk`
- `/sys/block`
- Kernel boot log

---

### RDK-SOC-STOR-002: eMMC Boot Partitions

**Requirement**

Platforms using eMMC as the primary boot device SHALL store boot firmware within the standard eMMC Boot Partitions.

The platform SHALL support standard eMMC boot partition management.

**Priority**

Mandatory

**Rationale**

Using standard eMMC boot partitions improves interoperability between firmware implementations and simplifies firmware updates.

**Verification**

- Verify firmware resides in eMMC boot partitions.
- Validate boot partition configuration.

**Evidence**

- eMMC partition layout
- Firmware image map

**References**

- JEDEC eMMC Specification
- Arm SystemReady

---

### RDK-SOC-STOR-003: Dual-Bank Flash

**Requirement**

Platforms booting from NOR or NAND flash SHALL support one of the following:

- Dual-bank firmware updates
- Automatic fallback to an alternate boot image
- Equivalent firmware recovery mechanism

Firmware updates SHALL NOT permanently render the platform unbootable following an interrupted update.

**Priority**

Mandatory

**Rationale**

Dual-bank or fallback boot mechanisms improve firmware reliability and enable robust over-the-air updates.

**Verification**

- Simulate interrupted firmware update.
- Validate recovery process.

**Evidence**

- Recovery validation report
- Boot log

---

### RDK-SOC-STOR-004: SSD Boot Support

**Requirement**

Platforms SHOULD support booting Linux from high-performance storage devices, including:

- NVMe
- SATA
- USB 3.x Mass Storage

where supported by the hardware architecture.

**Priority**

Recommended

**Rationale**

SSD boot reduces wear on embedded flash devices and improves developer productivity.

**Verification**

- Boot Linux from supported storage devices.
- Validate normal system operation.

**Evidence**

- Boot log
- Storage enumeration

---

### RDK-SOC-STOR-005: Standard Filesystem Support

**Requirement**

The platform SHALL use the standard Linux Virtual Filesystem (VFS) layer for filesystem access.

Filesystem support SHALL use standard Linux kernel implementations.

Vendor-specific filesystem APIs SHALL NOT replace standard Linux filesystem interfaces.

Supported filesystems MAY include:

- ext4
- SquashFS
- UBIFS
- F2FS
- FAT
- XFS (where applicable)

**Priority**

Mandatory

**Rationale**

Using the Linux VFS ensures compatibility across storage technologies and simplifies userspace software.

**Verification**

- Mount and access supported filesystems.
- Verify VFS operation.

**Evidence**

- `/proc/filesystems`
- Mount output
- Functional validation

---

### RDK-SOC-STOR-006: Storage Telemetry

**Requirement**

The platform SHALL expose storage utilization and capacity through standard Linux interfaces.

At minimum, the platform SHALL provide:

- Total capacity
- Available capacity
- Filesystem utilization
- Block device information

Vendor-specific APIs SHALL NOT be required.

**Priority**

Mandatory

**Rationale**

Standard telemetry enables RDK-B components to monitor storage without platform-specific software.

**Verification**

- Verify storage statistics using standard Linux interfaces.

**Evidence**

- `df`
- `lsblk`
- `/proc/diskstats`
- `/sys/block`

---

### RDK-SOC-STOR-007: Storage Health Monitoring

**Requirement**

Storage devices supporting health monitoring SHALL expose their health information using standard Linux interfaces.

Examples include:

- eMMC lifetime estimation
- NVMe SMART information
- SATA SMART attributes

Where supported by the hardware, the platform SHALL expose:

- Device health
- Remaining lifetime
- Media errors
- Temperature
- Critical warnings

Vendor-specific monitoring utilities SHALL NOT be required.

**Priority**

Recommended

**Rationale**

Standard health reporting enables predictive maintenance and improves platform reliability.

**Verification**

- Query storage health.
- Validate SMART or equivalent information.

**Evidence**

- `smartctl`
- `nvme smart-log`
- eMMC EXT_CSD lifetime data

---

### RDK-SOC-STOR-008: Storage Performance Monitoring

**Requirement**

The platform SHALL expose storage performance statistics through standard Linux kernel interfaces.

Where supported by the storage device, statistics SHOULD include:

- Read operations
- Write operations
- Read throughput
- Write throughput
- I/O latency
- Queue depth

Vendor-specific monitoring interfaces SHALL NOT be required.

**Priority**

Recommended

**Rationale**

Standard performance monitoring enables performance analysis and troubleshooting using existing Linux tools.

**Verification**

- Verify storage performance counters.

**Evidence**

- `/proc/diskstats`
- `/sys/block/*/stat`
- `iostat`

---

## 10. Networking Requirements

The platform SHALL implement networking functionality using standard Linux kernel networking subsystems and interfaces. Standard Linux networking APIs SHALL be used wherever possible to maximize interoperability, portability, and long-term maintainability.

---

### RDK-SOC-NET-001: Standard Ethernet

**Requirement**

The platform SHALL implement Ethernet interfaces using the standard Linux networking stack.

Ethernet MACs SHALL use the Linux networking subsystem and standard kernel drivers.

Supported Ethernet features MAY include:

- 10/100/1000BASE-T
- 2.5GBASE-T
- 5GBASE-T
- 10GBASE-T
- SFP/SFP+ interfaces

Vendor-specific networking APIs SHALL NOT replace standard Linux Ethernet interfaces.

**Priority**

Mandatory

**Rationale**

Standard Ethernet support enables interoperability with Linux networking tools and RDK-B networking components.

**Verification**

- Verify Ethernet interfaces enumerate correctly.
- Validate packet transmission and reception.
- Confirm standard Linux drivers are used.

**Evidence**

- `ip link`
- `ethtool`
- Kernel boot log
- Functional network tests

---

### RDK-SOC-NET-002: Standard Wi-Fi

**Requirement**

Wi-Fi devices SHALL integrate with the standard Linux wireless subsystem.

Where supported by the hardware, Wi-Fi implementations SHALL use:

- cfg80211
- mac80211

Vendor-specific wireless management frameworks SHALL NOT replace the standard Linux wireless stack.

Broadband-specific Wi-Fi features MAY be implemented through standard Linux extension mechanisms.

**Priority**

Mandatory

**Rationale**

Using the Linux wireless subsystem maximizes compatibility with userspace networking software and simplifies maintenance.

**Verification**

- Verify wireless interface detection.
- Confirm cfg80211/mac80211 integration.
- Validate wireless connectivity.

**Evidence**

- `iw dev`
- `iw phy`
- Kernel log
- Wi-Fi functional validation

---

### RDK-SOC-NET-003: Standard Bluetooth

**Requirement**

Bluetooth devices SHALL integrate with the standard Linux Bluetooth subsystem.

Implementations SHALL use:

- Bluetooth HCI
- BlueZ userspace stack

Vendor-specific Bluetooth APIs SHALL NOT replace the standard Linux Bluetooth architecture.

**Priority**

Mandatory

**Rationale**

The Linux Bluetooth subsystem provides a common implementation supporting multiple Bluetooth controllers and profiles.

**Verification**

- Verify Bluetooth controller detection.
- Validate Bluetooth communication.

**Evidence**

- `hciconfig`
- `bluetoothctl`
- Kernel log

---

### RDK-SOC-NET-004: Standard PHY Framework

**Requirement**

Ethernet PHY devices SHALL use the standard Linux PHY (phylib) and, where applicable, phylink frameworks.

Vendor-specific PHY management interfaces SHALL NOT replace standard Linux PHY frameworks.

The Device Tree SHALL use upstream PHY bindings.

**Priority**

Mandatory

**Rationale**

Using standard PHY frameworks enables interoperability across MAC and PHY vendors while simplifying driver maintenance.

**Verification**

- Verify PHY detection.
- Validate link negotiation.
- Confirm standard PHY driver usage.

**Evidence**

- `ethtool`
- Kernel log
- Device Tree
- PHY driver information

---

### RDK-SOC-NET-005: Standard Linux Networking Stack

**Requirement**

The platform SHALL expose networking functionality using the standard Linux networking stack.

At minimum, the platform SHALL support standard Linux networking interfaces including:

- Socket API
- Netlink
- rtnetlink
- ethtool
- tc
- nftables
- bridge
- VLAN
- bonding (where applicable)

Networking statistics SHALL be available through standard Linux interfaces.

Vendor-specific networking APIs SHALL NOT replace existing Linux networking interfaces.

**Priority**

Mandatory

**Rationale**

Using the standard Linux networking stack enables portable networking applications and allows RDK-B components to interact with the network using well-established Linux APIs.

**Verification**

- Verify networking functionality using standard Linux tools.
- Validate network statistics.
- Confirm no proprietary networking APIs are required.

**Evidence**

- `ip`
- `ss`
- `bridge`
- `ethtool`
- `/sys/class/net`
- `/proc/net`
- Functional networking validation
---

## 11. Fixed Wireless Access Requirements

Fixed Wireless Access (FWA) platforms SHALL integrate cellular modems using standard Linux kernel interfaces and userspace frameworks to maximize interoperability and portability across modem vendors.

---

### RDK-SOC-FWA-001: Standard Modem Interfaces (QMI / MBIM)

**Requirement**

Platforms integrating cellular modems SHALL expose modem functionality using standard Linux modem interfaces.

Supported modem control interfaces SHALL include, where applicable:

- Qualcomm MSM Interface (QMI)
- Mobile Broadband Interface Model (MBIM)

Modem devices SHALL integrate using standard Linux kernel drivers and networking subsystems.

Vendor-specific modem control interfaces SHALL NOT replace QMI or MBIM when either standard interface is supported by the modem.

**Priority**

Mandatory

**Rationale**

QMI and MBIM are widely supported industry-standard interfaces for modem management. Using these interfaces enables interoperability across modem vendors and simplifies integration with Linux networking and management frameworks.

**Verification**

- Verify modem enumeration.
- Verify QMI and/or MBIM interface availability.
- Validate modem registration.
- Validate data bearer establishment.
- Verify network connectivity.

**Evidence**

- `mmcli`
- `qmicli` or `mbimcli`
- `ip link`
- Kernel boot log
- Functional connectivity test

---

### RDK-SOC-FWA-002: ModemManager Orchestration

**Requirement**

Platforms SHOULD use ModemManager as the standard userspace modem orchestration layer.

ModemManager SHOULD manage:

- Modem discovery
- SIM management
- Registration state
- Bearer establishment
- Connection management
- Signal quality reporting
- Network technology selection
- Modem lifecycle management

Platform software SHOULD interact with the modem through ModemManager rather than directly using vendor-specific modem APIs.

**Priority**

Recommended

**Rationale**

ModemManager provides a standardized abstraction for modem management across multiple modem vendors and technologies, reducing platform-specific software and simplifying integration with RDK-B.

**Verification**

- Verify modem discovery through ModemManager.
- Validate registration and connection management.
- Confirm successful data connectivity.
- Verify modem status reporting.

**Evidence**

- `mmcli`
- ModemManager logs
- Functional connectivity test

---

## 12. GPIO and Platform I/O Requirements

The platform SHALL implement General Purpose Input/Output (GPIO) functionality using the standard Linux GPIO subsystem and associated kernel frameworks. Vendor-specific GPIO frameworks SHALL NOT replace standard Linux interfaces for platform I/O.

---

### RDK-SOC-IO-001: libgpiod

**Requirement**

Userspace applications SHALL manage GPIOs using the Linux GPIO Character Device interface (`libgpiod`).

The legacy GPIO sysfs interface (`/sys/class/gpio`) SHALL NOT be used.

**Priority**

Mandatory

**Rationale**

The GPIO Character Device API is the standard Linux interface for GPIO management and provides improved performance, security, and future compatibility compared to the deprecated sysfs interface.

**Verification**

- Verify GPIO access using `libgpiod`.
- Confirm no platform software depends on `/sys/class/gpio`.

**Evidence**

- `gpiodetect`
- `gpioinfo`
- `gpioset`
- `gpioget`
- Platform software review

---

### RDK-SOC-IO-002: Standard LED Framework

**Requirement**

LED devices SHALL be described using the standard Linux `gpio-leds` Device Tree binding and managed through the Linux LED subsystem.

Platform software SHALL control LEDs through the standard Linux LED framework.

Vendor-specific LED control interfaces SHALL NOT replace the Linux LED subsystem.

**Priority**

Mandatory

**Rationale**

The Linux LED framework provides a common interface for platform status LEDs and supports standard trigger mechanisms such as heartbeat, network activity, and disk activity.

**Verification**

- Verify LED enumeration.
- Validate LED operation.
- Confirm use of the Linux LED subsystem.

**Evidence**

- Device Tree
- `/sys/class/leds`
- Functional LED test

---

### RDK-SOC-IO-003: Standard GPIO Keys

**Requirement**

Platform buttons SHALL use the standard Linux `gpio-keys` Device Tree binding and Linux Input subsystem.

This requirement applies to, but is not limited to:

- Power button
- Reset button
- Recovery button
- WPS button
- User programmable buttons

Vendor-specific button frameworks SHALL NOT replace the Linux Input subsystem.

**Priority**

Mandatory

**Rationale**

The Linux Input subsystem provides a common interface for user input devices and enables portable userspace software.

**Verification**

- Verify button detection.
- Validate input events.
- Confirm use of the Linux Input subsystem.

**Evidence**

- Device Tree
- `evtest`
- `/dev/input/event*`
- Functional validation

---

### RDK-SOC-IO-004: No Vendor GPIO Frameworks

**Requirement**

Vendor-specific GPIO frameworks SHALL NOT replace standard Linux GPIO, LED, or Input subsystem functionality.

Platform-specific GPIO drivers MAY be implemented only when required to support hardware-specific functionality not provided by the standard Linux GPIO subsystem.

All GPIO controllers SHALL integrate with the upstream Linux GPIO subsystem and use standard Device Tree bindings.

**Priority**

Mandatory

**Rationale**

Using standard Linux GPIO frameworks improves portability, reduces maintenance effort, and allows RDK-B components to operate consistently across multiple hardware platforms.

**Verification**

- Review GPIO architecture.
- Verify use of standard Linux GPIO APIs.
- Confirm absence of proprietary GPIO management interfaces.

**Evidence**

- Platform architecture documentation
- Kernel configuration
- Device Tree
- Functional validation

---

## 13. Linux Telemetry Requirements

The platform SHALL expose runtime status, resource utilization, and health information through standard Linux kernel interfaces. Platform management software SHALL be able to retrieve telemetry information without requiring vendor-specific APIs or monitoring daemons.

---

### RDK-SOC-TEL-001: Standard Linux Telemetry Interfaces

**Requirement**

The platform SHALL expose platform telemetry using standard Linux kernel interfaces.

At minimum, the following information SHALL be available:

| Resource | Standard Linux Interface |
|----------|--------------------------|
| CPU utilization | `/proc/stat` |
| CPU information | `/proc/cpuinfo` |
| Memory utilization | `/proc/meminfo` |
| Block device statistics | `/proc/diskstats` |
| Storage devices | `/sys/block` |
| Filesystem usage | `statfs()`, `df` |
| Network interfaces | `/sys/class/net` |
| Network traffic statistics | `/sys/class/net/<iface>/statistics`, rtnetlink |
| Thermal sensors | `/sys/class/thermal` |
| Hardware monitoring | `/sys/class/hwmon` |
| System uptime | `/proc/uptime` |
| Load average | `/proc/loadavg` |
| Interrupt statistics | `/proc/interrupts` |
| IOMMU information | `/sys/kernel/iommu_groups` (where applicable) |
| Power supply information | `/sys/class/power_supply` (where applicable) |
| LED status | `/sys/class/leds` (where applicable) |

Vendor-specific telemetry APIs SHALL NOT be required to obtain basic platform status information.

Where Linux provides a standard subsystem for monitoring a hardware resource, that subsystem SHALL be used.

**Priority**

Mandatory

**Rationale**

Standard Linux telemetry interfaces provide a portable mechanism for monitoring platform health, performance, and resource utilization across multiple SoC vendors. Leveraging existing Linux interfaces minimizes software complexity and enables interoperability with standard Linux monitoring tools and RDK-B platform services.

**Verification**

- Verify each telemetry resource can be queried using standard Linux interfaces.
- Confirm no proprietary API is required for basic monitoring.
- Validate telemetry values during normal platform operation.

**Evidence**

- `/proc` output
- `/sys` hierarchy
- `ip`
- `ethtool`
- `df`
- `free`
- `vmstat`
- `iostat`
- Functional telemetry validation report
---

## 14. RDK-B Integration Requirements

The platform SHALL integrate with the standard RDK-B software architecture using common build, management, and runtime interfaces. Platform-specific implementations SHALL minimize modifications to the RDK-B software stack and maximize reuse of common platform components.

---

### RDK-SOC-RDK-001: Yocto Machine Configuration

**Requirement**

The platform SHALL provide a Yocto machine configuration compatible with the selected RDK-B reference release.

Platform support SHALL be implemented using reusable metadata and configuration within the RDK-B platform layer.

Platform-specific metadata SHALL be isolated from common RDK-B components.

**Priority**

Mandatory

**Rationale**

Using a common Yocto integration model minimizes duplication across SoC vendors and simplifies maintenance of future RDK-B releases.

**Verification**

- Build the complete RDK-B reference image.
- Verify successful image generation.
- Review Yocto machine configuration.

**Evidence**

- Machine configuration
- Build log
- Generated images

---

### RDK-SOC-RDK-002: RDK-B Reference Image

**Requirement**

The platform SHALL boot the standard RDK-B reference image without requiring platform-specific modifications to the RDK-B userspace software.

Platform-specific enablement SHALL be implemented through firmware, Device Tree, kernel configuration, and Yocto integration.

**Priority**

Mandatory

**Rationale**

A common userspace simplifies platform maintenance and allows RDK-B applications to execute consistently across multiple SoCs.

**Verification**

- Boot the RDK-B reference image.
- Execute platform smoke tests.
- Verify standard RDK-B services start successfully.

**Evidence**

- Boot log
- System journal
- Smoke test report

---

### RDK-SOC-RDK-003: Common Kernel Baseline

**Requirement**

The platform SHALL support the common RDK-B Linux kernel baseline.

Platform-specific kernel modifications SHALL be minimized and documented.

Kernel configuration SHALL use standard Linux configuration fragments.

Vendor-specific kernel patches SHALL be maintained independently and SHOULD be submitted upstream whenever practical.

**Priority**

Mandatory

**Rationale**

Maintaining a common Linux kernel baseline reduces long-term maintenance and simplifies migration between RDK-B releases.

**Verification**

- Build using the common kernel baseline.
- Review kernel patch set.
- Verify platform functionality.

**Evidence**

- Kernel version
- Kernel configuration
- Patch list
- Build log

---

### RDK-SOC-RDK-004: RBUS Integration

**Requirement**

Platform-specific software SHALL expose management functionality through RBUS where required by the RDK-B architecture.

Platform services SHALL integrate with RBUS using standard RDK-B interfaces rather than proprietary communication mechanisms.

RBUS SHALL be used only where defined by the RDK-B software architecture and SHALL NOT replace standard Linux kernel interfaces.

**Priority**

Mandatory

**Rationale**

RBUS provides the common management interface used by RDK-B while allowing Linux kernel interfaces to remain the primary hardware abstraction layer.

**Verification**

- Verify RBUS objects are available.
- Validate inter-component communication.
- Execute RBUS functional tests.

**Evidence**

- RBUS object listing
- Functional validation report
- Platform logs

---

### RDK-SOC-RDK-005: TR-181 Object Model

**Requirement**

The platform SHALL expose platform capabilities through the Broadband Forum TR-181 Device Data Model where required by RDK-B.

Platform-specific implementations SHALL map hardware capabilities to standard TR-181 objects whenever applicable.

Vendor-specific TR-181 extensions SHALL be minimized and documented.

**Priority**

Mandatory

**Rationale**

TR-181 provides a standardized management model that enables interoperability between RDK-B components and external management protocols.

**Verification**

- Verify required TR-181 objects.
- Validate object values.
- Execute management interoperability tests.

**Evidence**

- TR-181 object list
- USP/TR-069 validation
- Functional test report

---

### RDK-SOC-RDK-006: Standard Linux Userspace Interfaces

**Requirement**

Platform-specific userspace software SHALL use standard Linux APIs and services wherever available.

Examples include:

- D-Bus
- systemd
- Netlink
- libgpiod
- ModemManager
- NetworkManager (where applicable)
- Standard Linux filesystems
- procfs
- sysfs

Vendor-specific userspace APIs SHALL be limited to hardware-specific functionality for which no suitable Linux or RDK-B interface exists.

**Priority**

Mandatory

**Rationale**

Using standard Linux userspace interfaces minimizes platform-specific software, improves portability across SoCs, and simplifies long-term maintenance of the RDK-B software stack.

**Verification**

- Review userspace architecture.
- Verify use of standard Linux APIs.
- Confirm vendor-specific APIs are justified.

**Evidence**

- Platform architecture documentation
- Software dependency analysis
- Functional validation report

---

## 15. Validation Requirements

The platform SHALL demonstrate compliance with this specification through objective validation activities. Validation SHALL be repeatable and based on standardized test suites wherever possible.

Unless otherwise specified, successful completion of the validation activities defined in this section SHALL be required before a platform is accepted as an RDK-B Reference Platform.

---

### RDK-SOC-VALID-001: Arm SystemReady Validation

**Requirement**

The platform SHALL successfully complete the applicable Arm SystemReady compliance program appropriate for the target platform.

Validation SHALL include all mandatory certification tests defined by the selected SystemReady profile.

**Priority**

Mandatory

**Rationale**

Arm SystemReady provides an industry-standard validation framework ensuring interoperability between firmware and operating systems.

**Verification**

- Execute the complete Arm SystemReady validation suite.
- Review certification results.

**Evidence**

- Arm SystemReady certification report
- Validation logs
- Firmware version
- Platform configuration

---

### RDK-SOC-VALID-002: Device Tree Validation

**Requirement**

The shipping Device Tree SHALL successfully pass upstream Linux Device Tree schema validation.

Validation SHALL include:

- Device Tree schema validation (`dtbs_check`)
- Device Tree compilation without errors
- Verification of upstream bindings
- Review of any documented waivers

Validation SHALL confirm that all operating system visible hardware is described using standard Linux Device Tree bindings unless explicitly exempted by this specification.

**Priority**

Mandatory

**Rationale**

Device Tree validation guarantees compatibility between firmware and the Linux kernel while minimizing platform-specific maintenance.

**Verification**

- Execute `make dtbs_check`.
- Review schema validation results.

**Evidence**

- dtbs_check report
- DTS/DTSI source
- DTB artifact

---

### RDK-SOC-VALID-003: Boot Validation

**Requirement**

The platform SHALL successfully boot the RDK-B reference image using the standard firmware implementation.

Validation SHALL include:

- Cold boot
- Warm reboot
- Power cycle
- Secure Boot (where enabled)
- Boot from supported storage devices
- Firmware update recovery (where applicable)

The platform SHALL boot without requiring platform-specific userspace modifications.

**Priority**

Mandatory

**Rationale**

Boot validation demonstrates that firmware, kernel, and userspace integrate correctly using standard interfaces.

**Verification**

- Execute boot validation scenarios.
- Verify successful system initialization.
- Confirm all required platform services start correctly.

**Evidence**

- Boot log
- Kernel log
- Service status
- Boot timing report

---

### RDK-SOC-VALID-004: PCI Express Validation

**Requirement**

All supported PCI Express functionality SHALL be validated using standard Linux drivers.

Validation SHALL include, where applicable:

- PCIe enumeration
- Driver loading
- MSI/MSI-X
- DMA through the SMMU
- NVMe operation
- Ethernet adapters
- Wi-Fi adapters
- Cellular modems
- Hot-plug (if supported)

**Priority**

Mandatory

**Rationale**

PCIe validation confirms interoperability with commercially available PCI Express devices without requiring vendor-specific software.

**Verification**

- Enumerate supported PCIe devices.
- Execute functional tests.
- Validate interrupt handling.
- Verify IOMMU isolation.

**Evidence**

- lspci output
- Kernel log
- Functional validation report

---

### RDK-SOC-VALID-005: Security Validation

**Requirement**

The platform SHALL successfully validate all implemented security features.

Validation SHALL include, where applicable:

- Secure Boot
- TPM2 or fTPM
- OP-TEE
- Secure Storage
- Chain of Trust
- Firmware authentication
- Rollback protection

Negative testing SHALL verify that modified or unsigned firmware images are rejected.

**Priority**

Mandatory

**Rationale**

Security validation provides objective evidence that platform security mechanisms operate correctly.

**Verification**

- Execute security validation tests.
- Perform negative boot tests.
- Verify certificate management.

**Evidence**

- Security validation report
- TPM logs
- Secure Boot report
- OP-TEE validation logs

---

### RDK-SOC-VALID-006: RDK-B Platform Validation

**Requirement**

The platform SHALL successfully execute the RDK-B platform validation suite.

Validation SHALL confirm:

- Successful build using the RDK-B reference Yocto release.
- Boot of the standard RDK-B reference image.
- Correct operation of mandatory RDK-B platform services.
- Availability of required TR-181 objects.
- Correct RBUS integration.
- Platform telemetry through standard Linux interfaces.
- Functional networking and storage.
- Successful execution of platform smoke tests.

**Priority**

Mandatory

**Rationale**

RDK-B platform validation ensures that the platform satisfies the functional requirements of the RDK-B software stack while remaining compatible with the common reference architecture.

**Verification**

- Execute the complete RDK-B validation suite.
- Review platform test results.
- Verify all mandatory services operate correctly.

**Evidence**

- RDK-B validation report
- Smoke test report
- RBUS validation
- TR-181 validation
- Platform logs
---

## 16. Upstreaming Requirements

The platform SHALL maximize the use of upstream open-source software components. Vendor-specific modifications SHALL be minimized, documented, and progressively contributed to the appropriate upstream projects wherever practical.

---

### RDK-SOC-UPSTREAM-001: Linux Kernel

**Requirement**

The SoC vendor SHALL minimize platform-specific Linux kernel patches.

Kernel functionality SHALL use upstream Linux subsystems wherever available.

Platform-specific kernel patches SHALL:

- Be documented.
- Be maintained by the SoC vendor.
- Include an upstream submission plan where applicable.

**Priority**

Mandatory

**Rationale**

Reducing vendor-specific kernel modifications simplifies long-term maintenance and enables migration to future Linux kernel and Yocto releases.

**Verification**

- Review platform kernel patch set.
- Compare against the upstream Linux kernel.
- Review upstream submission status.

**Evidence**

- Kernel patch list
- Upstream submission references
- Patch tracking documentation

---

### RDK-SOC-UPSTREAM-002: Device Tree Bindings

**Requirement**

All operating system visible hardware SHALL use upstream Linux Device Tree bindings.

New Device Tree bindings SHALL be submitted through the Linux kernel Device Tree binding review process.

Vendor-specific bindings SHALL only be used where explicitly permitted by this specification.

**Priority**

Mandatory

**Rationale**

Upstream Device Tree bindings provide a stable firmware/operating system contract and enable automated schema validation.

**Verification**

- Review Device Tree bindings.
- Execute `make dtbs_check`.
- Verify upstream binding references.

**Evidence**

- YAML bindings
- Device Tree source
- dtbs_check report

---

### RDK-SOC-UPSTREAM-003: Device Tree Source

**Requirement**

The SoC Device Tree and board Device Tree SHOULD be accepted into the upstream Linux kernel.

Platform-specific Device Trees SHALL remain synchronized with the upstream implementation.

Forked Device Trees SHALL be avoided.

**Priority**

Recommended

**Rationale**

Maintaining Device Trees upstream reduces BSP maintenance effort and improves compatibility with future Linux releases.

**Verification**

- Review upstream Device Tree status.
- Compare shipping DTS with upstream.

**Evidence**

- Upstream DTS references
- Linux kernel version
- DTS comparison report

---

### RDK-SOC-UPSTREAM-004: U-Boot

**Requirement**

Platform boot support SHALL use upstream U-Boot wherever practical.

Platform-specific U-Boot modifications SHALL be minimized and documented.

The U-Boot Device Tree SHALL be derived from the same source as the Linux Device Tree.

**Priority**

Recommended

**Rationale**

Using upstream U-Boot improves firmware maintainability and simplifies migration between releases.

**Verification**

- Review U-Boot modifications.
- Compare against upstream.
- Validate common Device Tree usage.

**Evidence**

- U-Boot version
- Patch list
- DTS comparison

---

### RDK-SOC-UPSTREAM-005: Arm Trusted Firmware

**Requirement**

The platform SHOULD use upstream Arm Trusted Firmware-A (TF-A) whenever practical.

Platform-specific modifications SHALL be minimized, documented, and maintained independently.

Binary-only firmware SHALL be limited to components that cannot reasonably be provided as open source (for example, proprietary DDR initialization firmware).

**Priority**

Recommended

**Rationale**

Using upstream TF-A improves firmware maintainability, security, and compatibility with future Arm architecture updates.

**Verification**

- Review TF-A version.
- Compare against upstream.
- Review platform-specific patches.

**Evidence**

- TF-A version
- Patch list
- Platform firmware documentation

---

### RDK-SOC-UPSTREAM-006: Upstream Roadmap

**Requirement**

The SoC vendor SHALL maintain an upstreaming roadmap covering all major platform software components.

The roadmap SHALL identify:

- Current upstream status.
- Components awaiting upstream acceptance.
- Planned submission milestones.
- Known technical blockers.
- Target Linux kernel and Yocto releases.

The roadmap SHALL be reviewed as part of platform onboarding and updated for each major RDK-B release.

**Priority**

Mandatory

**Rationale**

Maintaining an upstream roadmap provides visibility into long-term platform maintainability and helps reduce divergence from upstream open-source projects.

**Verification**

- Review roadmap documentation.
- Review upstream contribution status.
- Validate roadmap against current platform implementation.

**Evidence**

- Upstream roadmap
- Contribution tracker
- Linux mailing list references
- Git repository references

---

## 17. Compliance Evidence

Each requirement SHALL include:

- Requirement ID
- Requirement statement
- Priority
- Rationale
- Owner
- Verification method
- Required evidence
- Status
- Comments

---

# Appendix A – Normative References

- Arm SystemReady
- EBBR
- Arm BBR
- UEFI
- Linux Kernel
- Device Tree Specification
- Yocto Project
- RDK-B
- TR-181
