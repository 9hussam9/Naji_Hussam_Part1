# Logbook - Björklunda kommun
**Namn:** [Hussam Naji]
**E-post:** [mhdall990103a@student.jenseneducation.se]
**Inlämning:** Del 1 & 2 - Grundmiljö och Planering

---

## Arbetslogg

### 2026-04-26
**Arbetat med:** Del 1: Förberedelser och Del 2: Planering.

**Vad jag gjorde:**
- Konfigurerade Git med namn och e-post.
- Skapade projektstrukturen med mappar för screenshots, scripts, data och results.
- Initierade ett lokalt Git-repo.
- Skapade signaturskript för både Linux (.sh) och Windows (.ps1).
- Genomförde research om filsystem och Identity Management inför installationen.

**Problem och lösningar:**
- Vid körning av PowerShell-skriptet blockerades det av systemets säkerhetspolicy. Jag löste detta genom att köra kommandot `Set-ExecutionPolicy RemoteSigned` i en administrativ terminal.

**Beslut jag fattade:**
- Jag valde att följa Red Hats standardrekommendationer för partitionering men lade till en marginal för /var på IdM-servern för att framtidssäkra databasen.

**Källor jag använde:**
- Red Hat Enterprise Linux Documentation (access.redhat.com)
- Git Documentation (git-scm.com)

---

## Del 1: Förberedelse och sätta upp repo
Mappstrukturen har skapats och Git är initierat. Signaturskripten är placerade i `/scripts` och fungerar korrekt för att verifiera identitet och tidsstämpel vid skärmdumpar.

## Del 2: Planering

### Research-frågor
**1. Vilken storlek rekommenderar Red Hat för /boot-partitionen?**
Red Hat rekommenderar minst 1 GB för `/boot`. Detta krävs för att rymma flera kernel-versioner och tillhörande boot-filer (initramfs) vid uppdateringar.

**2. Vad är skillnaden mellan XFS och ext4?**
XFS är standardfilsystemet i RHEL 9 och är optimerat för stora filsystem och hög parallellitet i läs- och skrivoperationer. Ext4 är mer traditionellt och kan i vissa fall vara effektivare på mindre volymer, men saknar vissa av de avancerade skalbarhetsfunktionerna som finns i XFS.

**3. Vad är RHEL IdM och vad används det till?**
RHEL Identity Management (IdM) är en lösning baserad på FreeIPA som används för att centralisera hantering av användare, grupper och autentisering i Linux-miljöer. Det kombinerar tekniker som LDAP och Kerberos för att skapa en säker och enhetlig inloggningsmiljö.

**4. Hantering av svårigheter:**
Det var till en början oklart hur swap-partitionen påverkar prestanda på en virtuell maskin. Genom att läsa Red Hats manual förstod jag att 2 GB är en bra balans för en server med denna profil för att hantera minnestoppar utan att slösa för mycket diskutrymme.

**5. Källor:**
- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9
- https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux

### Partitioneringsplan
Följande plan gäller för `srv-linux01` och `srv-idm01`:

| Monteringspunkt | Storlek | Filsystem | Motivering |
| :--- | :--- | :--- | :--- |
| /boot | 1 GB | xfs | Red Hats rekommenderade minimum. |
| / (root) | 20 GB | xfs | Standardstorlek för operativsystem och applikationer. |
| /home | 10 GB | xfs | För att separera användardata från systemfiler. |
| swap | 2 GB | swap | För att hantera minnesallokering vid behov.