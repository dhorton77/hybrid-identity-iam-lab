# On-Premises Active Directory Foundation

## Objective

Build and validate the on-premises identity foundation for a hybrid Microsoft Entra ID lab.

The environment uses Windows Server 2025 Active Directory Domain Services (AD DS) and DNS, with Windows 11 Enterprise domain-joined clients. This foundation will later be extended into Microsoft Entra ID to demonstrate hybrid identity, identity lifecycle management, Conditional Access, privileged access, and Zero Trust principles.

## Lab Environment

- Domain: ad.sc300lab.test
- Domain Controller: SC300-DC01
- Domain Controller IP: 192.168.1.2
- Client 1: SC300-PC01
- Client 2: SC300-PC02
- Client OS: Windows 11 Enterprise
- Server OS: Windows Server 2025
- Firewall/Router: IPFire

## Network Architecture

The lab is isolated behind an IPFire firewall/router.

- IPFire GREEN interface: 192.168.1.1
- IPFire provides DHCP services to the lab network.
- SC300-DC01 uses the static address 192.168.1.2.
- SC300-DC01 provides authoritative DNS for the Active Directory domain.
- Domain clients use 192.168.1.2 as their DNS server.
- SC300-DC01 forwards external DNS queries to Cloudflare (1.1.1.1).
- Internet-bound traffic is routed through IPFire.
- Active Directory clients do not use public DNS directly.

This design ensures that domain clients can locate Active Directory services through the domain controller while external DNS resolution is handled through the DNS forwarding configuration.

## Active Directory Domain Services Deployment

Windows Server 2025 was promoted as the first domain controller in a new Active Directory forest.

- Forest root domain: ad.sc300lab.test
- NetBIOS domain name: SC300LAB
- Forest functional level: Windows Server 2025
- Domain functional level: Windows Server 2025
- DNS Server role: Enabled
- Global Catalog: Enabled
- SYSVOL replication: DFSR
- Domain Controller: SC300-DC01

As the first domain controller in a new forest, SC300-DC01 initially holds all five FSMO roles: Schema Master, Domain Naming Master, PDC Emulator, RID Master, and Infrastructure Master.

## Troubleshooting: Domain Controller Rename

During the initial deployment, the server was promoted to a domain controller while still using its automatically generated Windows hostname.

Rather than interrupting the AD DS promotion or immediately forcing a rename, the deployment was allowed to complete and the health of Active Directory was validated first.

After confirming AD DS, DNS, SYSVOL, NETLOGON, replication, and FSMO role availability, a supported domain controller rename was performed using 
etdom computername.

The remediation process included:

- Adding SC300-DC01.ad.sc300lab.test as an alternate computer name.
- Making SC300-DC01.ad.sc300lab.test the primary computer name.
- Restarting the domain controller.
- Confirming Active Directory recognised SC300-DC01 as the domain controller.
- Removing the original automatically generated hostname.
- Verifying DNS records and LDAP SRV records.
- Verifying SYSVOL and NETLOGON shares.
- Confirming all five FSMO roles remained assigned to SC300-DC01.
- Checking for duplicate SPNs with setspn -X.
- Confirming the obsolete hostname was no longer present in the domain controller SPNs.

This demonstrated that the naming issue could be remediated without rebuilding the domain while preserving Active Directory functionality.

## DNS Configuration

Active Directory DNS is hosted on SC300-DC01.

- SC300-DC01 static IPv4 address: 192.168.1.2
- SC300-DC01 DNS client address: 192.168.1.2
- Domain clients use 192.168.1.2 for DNS.
- The Active Directory DNS zone is ad.sc300lab.test.
- AD service discovery records, including LDAP SRV records, resolve through SC300-DC01.
- External DNS queries are forwarded by SC300-DC01 to Cloudflare DNS at 1.1.1.1.
- Domain clients do not query public DNS directly.

This design ensures Active Directory clients can reliably locate domain services while external name resolution is handled through the domain DNS server.

## Windows 11 Domain-Joined Clients

Two Windows 11 Enterprise clients were added to the Active Directory domain.

- SC300-PC01
- SC300-PC02
- Domain: ad.sc300lab.test
- DNS server: 192.168.1.2
- Logon server: SC300-DC01

After each domain join, the computer was restarted and authenticated using the SC300LAB domain.

The Active Directory secure channel was validated on both clients using Test-ComputerSecureChannel -Verbose.

Both clients returned True and confirmed that the secure channel between the local computer and ad.sc300lab.test was in good condition.

The LOGONSERVER environment variable was also checked and confirmed that both clients were authenticating against SC300-DC01.

## Security Rationale

The Active Directory environment was designed around least privilege, controlled trust, and defence-in-depth principles.

- Active Directory clients use the domain controller for DNS rather than querying public DNS directly.
- SC300-DC01 provides authoritative DNS and Active Directory service discovery for the internal domain.
- External DNS resolution is forwarded through the domain controller rather than configured directly on domain clients.
- IPFire provides the network boundary between the isolated lab network and external networks.
- Domain membership establishes centrally managed identities and computer trust relationships.
- Secure channel validation confirms that each domain-joined client maintains a valid trust relationship with Active Directory.
- DNS, LDAP SRV records, SYSVOL, NETLOGON, FSMO roles, and SPNs were validated rather than assuming successful configuration.

This provides the on-premises identity foundation that will later be extended into Microsoft Entra ID for hybrid identity, Conditional Access, privileged access management, identity lifecycle management, and Zero Trust testing.
