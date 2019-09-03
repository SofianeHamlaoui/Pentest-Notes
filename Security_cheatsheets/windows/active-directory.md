# Introduction

Active Directory enables centralized, secure management of an entire network, which might span a building, a city, or multiple locations throughout the world.

![Active Directory Introduction](https://i-technet.sec.s-msft.com/dynimg/IC196825.gif)

> https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10)

# Components

![Structure](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/images%5ccc759186.ccf65c10-edb1-4a3a-ad87-38775ee43b8a%28ws.10%29.gif)

> https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759186(v%3dws.10)

| Component | Description |
| --------- | ----------- |
| Organizational Units | <ul><li>Container object</li><li>Used to arrange other objects</li><li>Easier to locate and manage</li><li>Can delegate the authority to manage</li><li>Can be nested in other organizational units </li></ul>|
| Domains | <ul><li>Container object</li><li>Collection of administratively defined objects that share a common directory database, security policies, and trust relationships with other domains</li><li>Each domain is an administrative boundary for objects.</li><li>A single domain can span multiple physical locations or sites</li></ul>|
| Domain Trees | <ul><li>Collections of domains that are grouped together in hierarchical structures</li><li>When you add a domain to a tree, it becomes a child of the tree root domain</li><li>The domain to which a child domain is attached is called the parent domain.</li><li>A child domain might in turn have its own child domain.</li><li>The name of a child domain is combined with the name of its parent domain to form its own unique Domain Name System (DNS) name such as Corp.nwtraders.msft.</li><li>.:. a tree has a contiguous namespace.</li></ul>|
| Forests | <ul><li>Instance of Active Directory</li><li>Each forest acts as a top-level container in that it houses all domain containers for that particular Active Directory instance</li><li>A forest can contain one or more domain container objects, all of which share a common logical structure, global catalog, directory schema, and directory configuration, as well as automatic two-way transitive trust relationships.</li><li>The first domain in the forest is called the `forest root domain`.</li><li>The name of that domain refers to the forest, such as Nwtraders.msft.</li><li>By default, information in Active Directory is shared only within the forest.</li><li>.:. the forest is a security boundary for the information that is contained in that instance of Active Directory</li></ul>|
| Site Objects | <ul><li>Leaf and container objects</li><li>Topmost object in the hierarchy of objects that are used to manage and implement Active Directory replication</li><li>Stores the hierarchy of objects that are used by the `Knowledge Consistency Checker (KCC)` to effect the replication topology</li><li>Some of the objects located in: `NTDS Site Settings objects`, `subnet objects`, `connection objects`, `server objects`, and `site objects` (one site object for each site in the forest)</li><li>Hierarchy is displayed as the contents of the Sites container, which is a child of the Configuration container</li></ul>|

> https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc759073(v%3dws.10)

* **Schema** - Defines objects and attributes
* **Query and index mechanism** - Ability to search and publish objects and properties
* **Global Catalog** - Contains info about every object in directory
* **Replication Service** - Distributes information across domain controller

# Detect Firewall Blocking AD

PortQryUI - http://www.microsoft.com/download/en/details.aspx?id=24009
* Run the “Domains & Trusts” option between DCs, or between DCs and any machine
* “NOTLISTENING,” 0x00000001, and 0x00000002, that means there is a port block
* Can ignore UDP 389 and UDP 88 messages
* TCP 42 errors, that just means WINS is not running on the target server

> https://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/

# Implementing Least Privilege Model

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models

# Scanning

```
pingcastle.exe --healthcheck --server <DOMAIN_CONTROLLER_IP> --user <USERNAME> --password <PASSWORD> --advanced-live --nullsession
```

- Automating AD Enumeration (Bloodhound, PowerUp, Responder, CrackMapExec): https://medium.com/bugbountywriteup/automating-ad-enumeration-with-frameworks-f8c7449563be

# Attack Patterns

- Wagging the Dog: Abusing Resource-Based Constrained Delegation to Attack Active Directory: https://shenaniganslabs.io/2019/01/28/Wagging-the-Dog.html
- Escalating privileges with ACLs in Active Directory: https://blog.fox-it.com/2018/04/26/escalating-privileges-with-acls-in-active-directory/

# Securing

- Active Directory Core Security Principles & Best Practices:  https://ernw.de/download/AD_Summit_2018/01_AD_Summit_CoreSecPrinciples_fk_hw_v.1.2_signed.pdf
- Active Directory Kill Chain Attack & Defense: https://github.com/infosecn1nja/AD-Attack-Defense
- Microsoft-Blue-Forest: https://github.com/rootsecdev/Microsoft-Blue-Forest
  - Welcome to building your first domain controller!: https://github.com/rootsecdev/Microsoft-Blue-Forest/blob/master/FirstDomainControllerInstall.md
- Pwn and Defend - Active Directory Domain Enumeration: https://www.youtube.com/watch?v=YxeXfHkHAUI&feature=youtu.be
