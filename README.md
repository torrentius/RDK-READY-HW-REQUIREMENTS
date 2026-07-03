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

### RDK-SOC-BOOT-001: EBBR UEFI compliance

### RDK-SOC-BOOT-002: UEFI Secure Boot

### RDK-SOC-BOOT-003: EFI Capsule firmware updates

### RDK-SOC-BOOT-004: Arm PSCI support

### RDK-SOC-BOOT-005: Reserved memory exposed through UEFI

### RDK-SOC-BOOT-006: EFI Runtime Variables

### RDK-SOC-BOOT-007: Independent BL31 updates

### RDK-SOC-BOOT-008: eMMC boot partitions

### RDK-SOC-BOOT-009: Dual-bank / fallback boot

### RDK-SOC-BOOT-010: SSD boot support

---

## 4. Platform Security Requirements

### RDK-SOC-SEC-001: TPM2 or fTPM

### RDK-SOC-SEC-002: OP-TEE support

### RDK-SOC-SEC-003: Secure storage

### RDK-SOC-SEC-004: Rollback protected RTC

### RDK-SOC-SEC-005: Arm Chain of Trust

### RDK-SOC-SEC-006: Secure Boot validation

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
