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

### RDK-SOC-BOOT-007: Independent BL31 Updates

**Requirement**

The firmware architecture SHALL permit BL31 (or the equivalent runtime firmware stage) to be updated independently of earlier boot stages whenever technically feasible.

Updating BL31 SHALL NOT require updating BL2 unless required by platform architecture.

**Priority**

Mandatory

**Rationale**

Independent firmware updates reduce deployment risk and minimize unnecessary firmware replacement.

**Verification**

- Perform standalone BL31 firmware update.
- Verify successful boot after update.

**Evidence**

- Firmware versions
- Update log

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

### RDK-SOC-SEC-004: Rollback-Protected RTC

**Requirement**

The platform SHALL provide a trusted time source that prevents unauthorized rollback of platform time.

The implementation MAY use:

- Battery-backed RTC
- Capacitor-backed RTC
- Secure monotonic counter
- Other equivalent secure time mechanism

The trusted time source SHALL support Secure Boot and certificate validation use cases.

**Priority**

Recommended

**Rationale**

Preventing time rollback protects against attacks involving expired certificates, firmware rollback, and replay of security-sensitive operations.

**Verification**

- Validate trusted time operation.
- Attempt rollback scenarios.
- Verify monotonic behavior.

**Evidence**

- RTC validation report
- Secure time test logs

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

### RDK-SOC-DT-001: Shipping DTS/DTSI sources

### RDK-SOC-DT-002: Device Tree schema validation

### RDK-SOC-DT-003: Shared Linux/U-Boot Device Tree

### RDK-SOC-DT-004: Upstream Device Tree roadmap

### RDK-SOC-DT-005: Upstream Device Tree bindings

### RDK-SOC-DT-006: Broadband-specific binding exceptions

### RDK-SOC-DT-007: Mandatory upstream bindings for standard Linux subsystems

---

## 6. Linux Kernel Requirements

### RDK-SOC-KERNEL-001: Mainline Linux roadmap

### RDK-SOC-KERNEL-002: Standard Linux drivers

### RDK-SOC-KERNEL-003: Standard kernel configuration

### RDK-SOC-KERNEL-004: Kernel configuration fragments

### RDK-SOC-KERNEL-005: Standard Linux interfaces

---

## 7. Hardware Platform Requirements

### RDK-SOC-HW-001: BSA/SBSA compliance

### RDK-SOC-HW-002: GICv3+

### RDK-SOC-HW-003: Standard UART

### RDK-SOC-HW-004: PCI MSI support

### RDK-SOC-HW-005: Standard SMMU

### RDK-SOC-HW-006: Shared-memory coprocessor support

---

## 8. PCI Express Requirements

### RDK-SOC-PCIE-001: Standard PCIe drivers

### RDK-SOC-PCIE-002: PCIe IOMMU isolation

### RDK-SOC-PCIE-003: MSI/MSI-X

### RDK-SOC-PCIE-004: Generic PCIe enumeration

### RDK-SOC-PCIE-005: PCIe hot-plug

---

## 9. Storage Requirements

### RDK-SOC-STOR-001: Standard Linux block devices

### RDK-SOC-STOR-002: eMMC boot partitions

### RDK-SOC-STOR-003: Dual-bank flash

### RDK-SOC-STOR-004: SSD boot

### RDK-SOC-STOR-005: Standard filesystem support

### RDK-SOC-STOR-006: Storage telemetry

### RDK-SOC-STOR-007: SMART / NVMe health

### RDK-SOC-STOR-008: Storage performance monitoring

---

## 10. Networking Requirements

### RDK-SOC-NET-001: Standard Ethernet

### RDK-SOC-NET-002: Standard Wi-Fi

### RDK-SOC-NET-003: Standard Bluetooth

### RDK-SOC-NET-004: Standard PHY framework

### RDK-SOC-NET-005: Standard Linux networking stack

---

## 11. Fixed Wireless Access Requirements

### RDK-SOC-FWA-001: Standard modem interfaces (QMI / MBIM)

### RDK-SOC-FWA-002: ModemManager orchestration

---

## 12. GPIO and Platform I/O Requirements

### RDK-SOC-IO-001: libgpiod

### RDK-SOC-IO-002: gpio-leds

### RDK-SOC-IO-003: gpio-keys

### RDK-SOC-IO-004: No vendor GPIO frameworks

---

## 13. Linux Telemetry Requirements

### RDK-SOC-TEL-001: Standard Linux telemetry interfaces

---

## 14. Virtualization Requirements

### RDK-SOC-VIRT-001: KVM support

### RDK-SOC-VIRT-002: Hypervisor compatibility

---

## 15. RDK-B Integration Requirements

### RDK-SOC-RDK-001: Yocto machine configuration

### RDK-SOC-RDK-002: RDK-B reference image

### RDK-SOC-RDK-003: Common kernel baseline

### RDK-SOC-RDK-004: RBUS integration

### RDK-SOC-RDK-005: TR-181 object model

### RDK-SOC-RDK-006: Standard Linux userspace interfaces

---

## 16. Validation Requirements

### RDK-SOC-VALID-001: Arm SystemReady validation

### RDK-SOC-VALID-002: Device Tree validation

### RDK-SOC-VALID-003: Boot validation

### RDK-SOC-VALID-004: PCIe validation

### RDK-SOC-VALID-005: Security validation

### RDK-SOC-VALID-006: RDK-B platform validation

---

## 17. Upstreaming Requirements

### RDK-SOC-UPSTREAM-001: Linux kernel

### RDK-SOC-UPSTREAM-002: Device Tree bindings

### RDK-SOC-UPSTREAM-003: Device Tree source

### RDK-SOC-UPSTREAM-004: U-Boot

### RDK-SOC-UPSTREAM-005: Arm Trusted Firmware

### RDK-SOC-UPSTREAM-006: Upstream roadmap

---

## 18. Compliance Evidence

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
