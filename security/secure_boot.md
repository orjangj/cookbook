# Secure Boot

Security is a journey, not a destination.

## Abstract

Most modern embedded devices allow the firmware to be updated either locally,
remotely or both. Standard (non-secure) bootloaders often just rely on a simple
checksum to ensure the next image is whole before it is accepted and executed.
From a security perspective, this is quite dangerous as it provides no mechanism
to protect the embedded system from low-level malware and being seized by an
adversary. Nearly every embedded system requires some level of security to
ensure the device cannot be easily compromised or tampered with. This is
especially true with the proliferation of Internet of Things (IoT) where edge
devices are being integrated into larger IoT infrastructures and environments. A
single weak link may compromise the entire IoT infrastructure, allowing
adversaries to steal intellectual property and sensitive data, or disrupt
business-critical operations. Secure Boot represents the first layer (or
barrier) for any layered security approach and provides the initial boot-up
protections to ensure only legitimate firmware is allowed to execute before
loading the Operating System (OS). Developing secure boot requires not only
hardware support for security related controls, but also embedded engineers with
expert understanding of the hardware capabilities and secure boot solutions
needed to ensure higher-layer security controls can be trusted.

## Acronyms

| Abbreviation | Meaning                                           |
| :----------- | :------------------------------------------------ |
| CoT          | Chain-of-Trust                                    |
| ECDSA        | Elliptic Curve Digital Signature Algorithm        |
| HSM          | Hardware Security Module                          |
| IoT          | Internet-of-Things                                |
| OEM          | Original Equipment Manufacturer                   |
| OS           | Operating System                                  |
| PKI          | Public Key Infrastructure                         |
| ROM          | Read-only Memory                                  |
| RoT          | Root-of-Trust                                     |
| RSA          | Rivest-Shamir-Adleman (A public-key cryptosystem) |

## Disclaimer

Any mention of commercial products or reference to commercial organizations is
for information only; it does not imply recommendation or endorsement by the
author(s), nor does it imply that the products mentioned are necessarily the
best available for the purpose.

## Scope

This document is mostly targeting Secure Boot for embedded systems such as
microcontrollers or application processors used in IoT edge devices. However,
many of the principles discussed within this document are applicable to other
general purpose computing systems.

## Audience

The primary audience for this document are embedded security engineers and
system architects. However, other hardware/software engineers may benefit from
reading this document as well.

## Introduction

A bootloader is a program that is executed on a device when it is powered on or
reset. Bootloaders come in many flavors, but in general bootloaders aim to
facilitate the following primary goals:

- Prepare the hardware and the embedded environment before loading the Operating
  System (OS) or main application.
- Allow the system software to be updated without the need for specialized
  hardware equipment such as a JTAG programmer.

A central boot program found in embedded processors is the Boot ROM - An
immutable piece of code stored in mask ROM or write-protected flash inside the
processor during production. It is the first significant code that is executed
by the processor after power-on or reset, and contains instructions for initial
hardware setup needed to boot the next-stage bootloader or main application.
Where and how to load and execute the next image from (and in which order) is in
many cases configurable through strap pins, electronic fuses, or internal flash
settings. Sometimes, the Boot ROM may contain additional functionality, possibly
extendable to user code during or after boot, such as the ability to verify the
next image for validity and correctness before transitioning control.

The capabilities of the Boot ROM ... varies ..., but in most cases the Boot ROM
is not flexible enough to support complex boot-up requirements (such as loading
an operating system from a file system). In most cases, more than one bootloader
is required to accomplish the task of updating or booting the system - A
so-called multi-stage boot process, where each stage in the boot sequence
executes its own boot program with specific set of capabilities and
responsibilities.

```plantuml
@startuml

!theme spacelab
skinparam nodesep 50

(*) -right-> "bootROM"
-> "First Stage\nBootloader"
-> "Second Stage\nBootloader"
-> "Operating \nSystem"

@enduml
```

This is especially true for application processors capable of running embedded
Linux where the processor's On-Chip RAM (OCRAM) may not be large enough to hold
the bootloader (OS loader) responsible for loading the Linux kernel. A so-called
Secondary Program Loader (SPL) is used to prepare external RAM on which the OS
loader may be loaded and executed.

Regardless of the boot process topology, it's important to note that bootloaders
runs on the system with a very high level of privilege. Standard (non-secure)
bootloaders often just rely on a simple checksum to ensure the next image is
whole before it is allowed to execute. From a security perspective, this is
quite dangerous as it allows the presence of low-level malware.

```plantuml
@startuml

!theme spacelab
skinparam nodesep 50
skinparam linetype ortho

(*) -right-> "bootROM"
-> "First Stage\nBootloader"
-> "Second Stage\nBootloader"
-> "Operating\nSystem"
"Adversary" -up-> "bootROM"
"Adversary" -up-> "First Stage\nBootloader"
"Adversary" -up-> "Second Stage\nBootloader"

@enduml
```

Low-level malware may enable adversaries to steal intellectual property,
credentials and other sensitive or critical data. Without an audit trail,
low-level malware may run on the device undetected for a prolonged period of
time (if detected at all), and serve as a starting point to compromise other
devices in a distributed system. Or even worse, it has the potential to render
the device unusable or permanently damaged which can disrupt business and
safety-critical operations - potentially leading to substantial financial costs
or health hazards.

Nearly every embedded system requires some level of security to ensure the
device cannot be easily compromised or tampered with. Secure Boot represents the
first layer (or barrier) for any layered security approach and provides the
initial boot-up protections to ensure higher-layer security controls can be
trusted by only allowing legitimate firmware to be executed.

## Secure Boot

The primary purpose of Secure Boot is to ensure only legitimate and authorized
firmware are allowed to execute before the OS (or main application) is loaded.
To prevent malicious code from compromising the boot process, Secure Boot relies
on a Chain-of-Trust (CoT) which originates from an immutable hardware level
Root-of-Trust (RoT). CoT is a method whereby each boot image is required to
cryptographically validate the digital signature of the next image (or any
pertinent boot component) against known and trusted keys before transitioning
control. The known and trusted keys are usually installed by the device
manufacturer (OEM) during production. If a problem is detected during the secure
boot process, the bootloader may either try to load an older version that was
known to work (or a golden image), or it may halt the system from booting
further.

```plantuml
@startuml

!theme spacelab
skinparam nodesep 50
skinparam linetype ortho

(*) -right-> "bootROM"
->[Auth] "First Stage\nBootloader"
->[Auth] "Second Stage\nBootloader"
->[Auth] "Operating\nSystem"

@enduml
```

More broadly, we want Secure Boot to have the following resiliency properties:

- Detection
- Protection
- Recovery

### Root-of-Trust

The Chain-of-Trust can only be trusted if it originates from an immutable
Root-of-Trust, which typically includes (but not limited to):

- A primary bootloader (Boot ROM) that supports Secure Boot
- A hardware-accelerated cryptographic engine
- A place to immutably store trusted keys or certificates
- Critical boot configuration settings

When an embedded system is powered on or reset, we want to ensure that the first

### Digital signatures

Secure Boot leverages digital signatures to provide the following security
services (TODO: REF):

- Data Integrity
- Source Authentication
- Non-repudiation

**Data Integrity** is a property whereby data has not been modified since it was
created, transmitted or stored. Digital signatures rely on Secure Hashing
Algorithms (SHA) to ensure data is in a state of integrity. Similar to checksums
(i.e. CRC32), SHA's detect accidental or intentional data corruption. However,
SHA's are designed to create unique digital fingerprints that are
computationally infeasible to forge and replace, thus making them suitable for
cryptographic operations.

**Source Authentication** provides assurance that the data originates from a
legitimate source (the private key-holder). This is achieved through use of
Public Key Cryptography (TODO: REF).

**Non-repudiation**, in a cryptographic sense, ... that the private key-holder
cannot deny having signed the data.

Code Signing

```plantuml
@startuml

!theme spacelab
skinparam nodesep 60
skinparam linetype ortho

"Firmware\nImage" -right-> "SHA"
->[Digest] "Encrypt Digest"
-> "Digital Signature"
"Private Key" -down-> "Encrypt Digest"

@enduml
```

Code Verification

```plantuml
@startuml

!theme spacelab
skinparam nodesep 60
skinparam linetype ortho

"Firmware\nImage" -right->[FW] "SHA"
->[Calc.\nDigest] "Identical?"
"Firmware\nImage" -up->[Signature] "Decrypt Signature"
"Public Key" -down-> "Decrypt Signature"
"Decrypt Signature" -->[Orig. digest] "Identical?"

@enduml
```

### Detection

### Protection

Normally, updating a system's firmware is managed by some application running as
part of the operating system. I.e. Mender or Rauc. Sometimes however, a
bootloader may provide functionality that allows system firmware to be updated
through some communication channel such as UART, USB, CAN, or Ethernet.

In such cases it's important that the bootloader protects the memory storage by
requiring authenticated update procedures before allowing the new firmware to be
installed.

### Recovery

Golden images, etc...?

Recover the system to a state of integrity.

## Other Responsibilities

Secure Boot is often tasked with several other security control mechanisms, such
as performing:

- Key revocation
- Rollback protection
- ...?

## Hardware selection

Every embedded device (ie. microcontroller or processor) have their nuances when
it comes to secure boot, but in general (although not an exhaustive list) the
following should be considered:

- Supply-chain attacks? Cloning of firmware, replacing keys, dummy chips, ...

## Key and algorithm selection

National Institute of Standards and Technology (NIST) is widely regarded as the
... (defacto standard) ... for security ...

- ECDSA
- RSA
- EdDSA

## Final words

Bootloaders in general are often overlooked and implemented late in the product
development cycle, despite being a complicated and critical component to
implement. Secure boot makes the effort even harder as you are responsible for
your own security.

Using third-party or vendor provided secure boot solutions decreases the
knowledge requirements a team needs in order to implement a secure boot solution
on their device. If those solutions are not enough, it is critical that the
implementation of secure boot is started as early as possible as it may take
several months to ensure the secure bootloader is built, tested and verified
successfully.

Every microcontroller or processor have their nuances when it comes to secure
boot, so make sure you read and understand the security reference manual before
deciding whether the processor fits your security requirements.

In general (although not an exhaustive list) you should consider the following:

- Built-in Root-of-Trust in hardware

  - This may take even longer if you do not have the infrastructure to support
    secure boot in place either. Such as a secure key management solution, or a
    process (including code signing tools) for generating digital signatures.

- You are responsible for your own security.
  - Selecting a strong key
  - Ensuring private keys are kept secret, protected and only accessible to
    trusted users
- Failing to burn the hash/keys into the processor properly
  - Do you trust your EOL procedure and surrounding process?
- False sense of security
  - Secure Boot does not lock the entire system down. It only secures the boot
    process until the OS takes over. It's possible to write some malware that
    runs on top of the OS which, if loaded successfully, could compromise the
    system.
- Using weak keys
- Using signature keys for more than just signing... undermines the security?
- Not following the security recommendations of the vendor?
  - Or not understanding the security limitations of your processor.
- Not protecting the surrounding processes such as key storage and usage
- Not testing the secure boot implementation
  - Confirm the process is authenticating correctly (both positive and negative
    test cases)
  - Ensure code has been properly written for secure boot
  - TIP: Avoid dynamic allocation ...
  - It's better to take a known, good implementation of a secure bootloader and
    match it to your needs. ...Use a well tested and ... proprietary/community
    bootloader?
- Not authenticating all steps in the boot process
- Not write-protecting boot configuration settings
- Make sure your silicon version does not have errata that may compromise the
  secure boot process.
  - nRF52 example?

Secure Boot Infrastructure is comprised of three ... integral parts ... to
ensure a device, on which secure boot is implemented, cannot (or the risk is
reduced to a minimum) be compromised.

1. Secure Boot (device)
   - HW root of trust
   - SW root of trust
   - Boot configuration settings
2. Secure Key Management (OEM)
   - Protecting keys from unauthorized disclosure
   - Access control and identity authentication
   - Audit control
3. Secure Signing Environment (OEM)
   - Protecting/Limit Key Management from unauthorized access.
   - Only trusted personnel/machines have direct access to the Key management
     service.

Other (equally important) considerations:

1. Cryptographic algorithms and key strength
   - Weak ...
2. Key revocation and rollback protection
3. Code safety and reliability
4. Hardware based isolation
   - Security begins with isolation.
   - Isolate applications into different domains that each have their own
     privileges and access to only specific areas of memory.
   - Ie. on multicore processors one core could be dedicated to security
     features. ARM TrustZone provides isolation on single core...

```mermaid
flowchart LR
A(Secure Boot) <--> B(Secure Key Management)
B <--> C(Secure Signing Environment)
C <--> A
```

If Secure Boot has been properly implemented on a target device, then what
attack vectors do we have left to exploit?

KEY SIZES and approved algorithms: See FIPS 186

## Secure Boot

The responsibility of Secure Boot is to ensure only authorized code can execute
before the OS is loaded, thus preventing malicious software (such as rootkits)
from being loaded. Secure Boot cryptographically validates the digital signature
of all boot components against known/trusted keys, starting from the primary
bootloader, to one or more intermediate boot loading stages, and finally to the
OS and all components that run it (i.e. drivers and applications).

To ensure a device is securely booting only trusted and legitimate code, a chain
of trust must be established which originates from an immutable Root-of-Trust.

Root-of-Trust ...:

- Primary Boot Loader (vendor bootROM implementation)
- Electrical fuses to store hash of trusted OEM public keys/certificates
- Boot configuration settings

> It's important that Root-of-Trust is immutable. This includes the primary
> bootloader, hash and boot configurations. If any of these are mutable, the
> Root-of-Trust can be compromised.

Secure Boot leverages digital signatures to provide the following security
services:

- **Data Integrity**
  - A property whereby data has not been modified since it was created,
    transmitted or stored.
- **Source Authentication**
  - Used to verify the identity of the entity that created and/or sent
    information.
- **Identity Authentication**
  - If combined with a PKI system?
  - ...?
- **Non-repudiation**
  - If combined with a PKI system
  - binds the name of the certificate subject to a public key... Signer is who
    they claim to be.

A Chain-of-Trust must be established and originate from the Root-of-Trust
(hardware). To establish trust between ... Root-of-Trust ...

```mermaid
flowchart LR
    A(Root-of-Trust) --> B(Secure Bootloader)
    B --> C(Secure Bootloader)
    C --> D(Application/OS)
```

In case a problem is detected during the boot process, ... may either halt the
system from booting further or it may try to load an earlier version of the code
that was known to work.

The known keys are usually determined by the device manufacturer (OEM).

```mermaid
flowchart LR

A(Signed Data) --> B(Signer Certificate)
C(Trust Anchor) -->|Verify identity| B
```

### Hardware Root-of-Trust

Root-of-Trust is the first foundational link in a Chain-of-Trust...

Keys (or certificates) are normally stored in electrical fuses as a secure hash.

TODO:

- Is it possible to have Root-of-Trust on devices which doesn't have
  Root-of-Trust built-in to the microcontroller/processor?

Establish root of trust.

Once programmed the keys and configuration settings must be immutable!

- Hardware Root-of-Trust
  - eFuse
    - One-time programmable
    - write-protection (immutable - cannot be changed or erased)
  - public keys / certificates
  - configuration fuses
- Software Root-of-Trust

  - bootROM
  - certified cryptographic algorithm implementations
  - ...

- Immutable keys/certificates on which the signature verification is based on
- Normally stored in electrical fuses which are write-protected after being
  burned, thus ensuring the keys cannot be tampered with. Similarily, essential
  boot configuration settings are also locked, ensuring it cannot be tampered
  with.

### Software Root-of-Trust

- immutable (vendor) code that ensures boot configuration settings are adhered
  to.
- If secure boot is enabled, the bootROM will ensure the next boot component is
  valid before allowing it to execute (ensures the initial boot-up is protected)

### Boot Configuration Settings

SECURITY HARDENING!!

### Public Key Infrastructure

- Some devices support Public Key Infrastructure (PKI), in which identity
  verification can be leveraged to ensure the owner of the key is who they claim
  to be.

Hardware Root of Trust ... Some devices may also support Public Key
Infrastructure (PKI).

## Secure Key Management

To maintain the security of a secure boot enabled device, protecting the private
key is critical. If the private key is known to an adversary, then secure boot
provides no security against executing malicious software (i.e. rootkits).
Secure Key Management is the process of managing a key throughout its lifecycle,
including its secure generation, storage, distribution, use and destruction.

Protection of the private key used for generated digital signatures. This ...
both having direct access to the private key material (in plaintext) or having
credentials that allow an adversary to use the protected key.

Industry best practices dictates ... use of ... certified (FIPS-140) Hardware
Security Modules.

--- FIPS-140 ---

So when a key is compromised, you have at most three options:

1. Revoke the compromised key such that devices in the field will not approve
   software signed with the key.
2. Or decommission/brick the device permanently, so it cannot be exploited by
   the adversary.
3. Or accept the fact that you cannot prevent malicious software from being
   installed (same as with non-secure/normal boot).

Option 1 is preferred if your device supports multiple sets of keys and has key
revocation capabilities (either through electrical fuses or through an internal
database of revoked keys). If your device does not support key revocation
mechanisms then option 2 is the preferred choice, but this might depend on the
purpose of the device. Option 3 is not really an option, but you run it at your
own risk.

So what measures do we have to ensure the private key is protected?

1. HSM
2. ...

- Implications of key compromise?

Secure Boot is "secure" only if the algorithms remain strong and the keys have
not been compromised. Key compromise occurs when the key can no longer be
trusted to provide the required security, i.e. as a result of failing to protect
the key from an adversary. If a key is compromised, all use of the key shall
cease, and the compromised key shall be revoked.

However, if digital signatures have been time-stamped ... blablabla... Normally,
small embedded microcontrollers/processors usually doesn't implement mechanisms
to adhere to time-stamped... simply because it relies on having an accurate
knowledge of the current time (which is generally not known during the early
boot process on such devices).

Key revocation is the process of letting devices know whether a key is still
safe to use...

- Lifetime of a signing key?
- Lifetime of a digital signature?
- Access control (Restrict resource access to only authorized entities)
- Accountability
  - Holding individuals who has been assigned key management responsibilities
    accountable
  - Ensure the actions of an entity may be traced uniquely to that entity
- Key Revocation

Secure boot is the process of cryptographically validating all boot components
before the operating system is loaded.

Public Key Cryptography

Why use it? When to use it?

- When device can be updated

Secure Key Management!

- HSM
- Vault ... Secure Signing Environment!
- Access control
- Audit control
- Code signing What keys to use? NIST, etc.. ? RSA vs ECDSA? Key revocation!
- eFuse

```mermaid
flowchart LR
C(Public Key/Certificate) --> B
A(Signed Image) --> B(Decrypt signature)
A --> D(Calculate hash)
B -->|Original hash| E{Compare hash}
D --> E
```

## OEM requirements

Series of tasks during manufacturing

- Provisioning secure boot keys
- Blowing configuration fuses
  - JTAG
  - Enable secure boot
  - Test access (i.e. vendor specific test access)
  - etc...

Other considerations

- Replay Protected Memory Block (RPMB)
  - Bind specific eMMC part and the SoC component in the device together.
    Prevents reuse of eMMC in another device with an OS that makes use of the
    RPMB.

## Multistage bootloading

In each stage, the bootloader preceeding is responsible for authenticating the
next image before allowing it to be executed. Additionally, it can be required
to authenticate additional images such as device tree, ramdisk image, scripts,
environment/configuration files, etc...

Primary bootloader (vendor) First stage bootloader (custom) Second stage
bootloader (custom) ...

```mermaid
A(Primary Boot Loader) -->|Authenticate| B(First Stage Boot Loader)
B -->|Authenticate| C(Second Stage Boot Loader)
C -->|Authenticate| D(Main OS)
```

## Conclusion

### Pitfalls

Bootloaders are often overlooked during the initial stages of product
development, despite the fact that bootloaders are complex and critical
components to implement. Every microcontroller or application processor have
their nuances when it comes to secure boot.

The lack of focus on implementing bootloaders in early development is primarily
because the bootloader is not the primary end product that provides the services
sold to customers. Secure boot makes the effort even more complicated as you're
responsible for your own security, and even a simple bug or mistake may be
detrimental to the security and integrity of your product.

> Rule #1: Start secure boot and bootloader development as early as possible

> Rule #2: Use trusted third-party or vendor provided secure boot solutions if
> possible

- Every microcontroller or application processor have their nuances when it
  comes to secure boot.
- At the very least, given an experienced developer familiar with secure boot,
  you should estimate 6-8 weeks of work. Using third-party or vendor provided
  secure boot solutions decreases the knowledge requirements a team needs in
  order to implement a secure boot solution on their device. If those solutions
  are not enough, it is critical that the implementation of secure boot is
  started as early as possible as it may take several months to ensure the
  secure bootloader is built, tested and verified successfully.
- This may take even longer if you do not have the infrastructure to support
  secure boot in place either. Such as a secure key management solution, or a
  process (including code signing tools) for generating digital signatures.

Using weak keys .... bla bla

> Rule #TODO: Ensure you select a strong key-pair ... (see NIST) ... over the
> life-time of the product.

> Rule #TODO: Ensure private keys are kept secret, protected and only accessible
> to trusted entities.

> Rule #TODO: Ensure you have a proper plan for key revocation before product is
> released to market.

- Failing to burn the hash/keys into the processor properly
  - Do you trust your EOL procedure and surrounding proccess?
- False sense of security
  - Secure Boot does not lock the entire system down. It only secures the boot
    process until the OS takes over. It's possible to write some malware that
    runs on top of the OS which, if loaded successfully, could compromise the
    system.
- Using signature keys for more than just signing... undermines the security?
- Not following the security recommendations of the vendor?
  - Or not understanding the security limitations of your processor.
- Not protecting the surrounding processes such as key storage and usage
- Not testing the secure boot implementation
  - Confirm the process is authenticating correctly (both positive and negative
    test cases)
  - Ensure code has been properly written for secure boot
  - TIP: Avoid dynamic allocation ...
  - It's better to take a known, good implementation of a secure boot loader and
    match it to your needs. ...Use a well tested and ... proprietary/community
    boot loader?
- Not authenticating all steps in the boot process
- Not write-protecting boot configuration settings
- Make sure your silicon version does not have errata that may compromise the
  secure boot process.
  - nRF52 example?

Don't be fooled by a false sense of security. Secure boot, if implemented
properly, ensures only authorized firmware is allowed to run on the device.
However, ... the device can be compromised if security policies or ... in the
OS/application is not ... implemented correctly ...

Should also consider:

- Secure storage
  - Protection against reverse engineering
  - Protection against leaking sensitive data such as encryption keys, user and
    service credentials and other system or customer data.
  - The purpose of secure storage is to prevent private/sensitive data from
    being leaked such that it can be used or cloned by adversaries.

List of well known and tested implementations (TODO verify):

IMPORTANT: Check any security advisories for the secure boot implementation.

|     name | vendor | architecture |
| -------: | ------ | ------------ |
|   U-Boot | ...    | ARM, ...     |
| WolfBoot | ...    | ...          |
|  MCUboot | ...    | 32-bit MCUs  |
|     UEFI | ...    | x86, ...     |

What if bootROM does not validate boot loader?

## Other considerations

0. Encrypted Boot
1. Measured Boot And Remote Attestation
2. Trusted Boot
3. Verified Boot
4. OPTEE
5. ...

## Resources

- [1]
  [NIST Key Management Guidelines](https://csrc.nist.gov/Projects/Key-Management/Key-Management-Guidelines)
- [2]
  [SP 800-57 Part 1](https://csrc.nist.gov/publications/detail/sp/800-57-part-1/rev-5/final)
- [3]
  [5 Elements to Secure Embedded System (part 1-5)](https://www.beningo.com/5-elements-to-secure-embedded-systems-part-1-hardware-based-isolation/)
- [4]
  [Hardware-Enabled Security](https://nvlpubs.nist.gov/nistpubs/ir/2022/NIST.IR.8320.pdf)
- [5]
  [Platform Firmware Resiliency Guidelines](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-193.pdf)
- [6]
  [Fundamentals of Booting Embedded Processors](https://www.embedded.com/fundamentals-of-booting-for-embedded-processors)
