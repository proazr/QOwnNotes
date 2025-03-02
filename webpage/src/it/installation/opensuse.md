# Installa su openSUSE Linux

<installation-opensuse/>

<!-- <Content :page-key="getPageKey($site.pages, '/installation/ubuntu.md')" /> -->


## Su tutte le versioni openSUSE

Puoi installare QOwnNotes usando lo strumento [OBS Package Installer](https://github.com/openSUSE/opi)

Eseguire i seguenti comandi della shell come root per installare `opi`:

```bash
zypper install opi
```

E poi installa QOwnNotes con questo:

```bash
opi qownnotes
```

Lo strumento interrogherà l'intero servizio OBS per essere sicuro di scegliere `qownnotes`, e non `qownnotes-lang` se richiesto.

Controlla anche che il repository scelto sia quello ufficiale `home:pbek:QOwnNotes` e non uno di terze parti.
:::

::: tip
You need to choose the option to keep the repository after installation to get updates.
:::

## openSUSE 13.2

Run the following shell commands as root to add the repository and install QOwnNotes from there.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_13.2/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Direct Download](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_13.2)

## openSUSE Leap 15.3

Run the following shell commands as root to add the repository and install QOwnNotes from there.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_15.3/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Direct Download](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Leap_15.3)

## openSUSE Leap 15.2

Eseguire i seguenti comandi della shell come root per aggiungere il repository e installare QOwnNotes da lì.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_15.2/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Leap_15.2)

## openSUSE Leap 15.1

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_15.1/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Leap_15.1)

## openSUSE Leap 15.0

Run the following shell commands as root to add the repository and install QOwnNotes from there.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_15.0/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Leap_15.0)

## openSUSE Leap 42.3

Eseguire i seguenti comandi della shell come root per considerare attendibile il repository.

```bash
su -
rpm --import http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_42.3/repodata/repomd.xml.key
```

Eseguire i seguenti comandi della shell come root per aggiungere il repository e installare QOwnNotes da lì.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_42.3/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Leap_42.3)

## openSUSE Leap 42.2

Eseguire i seguenti comandi della shell come root per considerare attendibile il repository.

```bash
su -
rpm --import http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_42.2/repodata/repomd.xml.key
```

Eseguire i seguenti comandi della shell come root per aggiungere il repository e installare QOwnNotes da lì.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_42.2/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Leap_42.2)

## openSUSE Leap 42.1

Eseguire i seguenti comandi della shell come root per considerare attendibile il repository.

```bash
su -
rpm --import http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_42.1/repodata/repomd.xml.key
```

Eseguire i seguenti comandi della shell come root per aggiungere il repository e installare QOwnNotes da lì.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Leap_42.1/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Leap_42.1)

## openSUSE Tumbleweed

Run the following shell commands as root to add the repository and install QOwnNotes from there.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/openSUSE_Tumbleweed/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/openSUSE_Tumbleweed)


## SLE 12 SP3 Backports

Run the following shell commands as root to add the repository and install QOwnNotes from there.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/SLE_12_SP3_Backports/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/SLE_12_SP3_Backports)

## SLE 15

Eseguire i seguenti comandi della shell come root per aggiungere il repository e installare QOwnNotes da lì.

```bash
zypper addrepo -f http://download.opensuse.org/repositories/home:/pbek:/QOwnNotes/SLE_15/home:pbek:QOwnNotes.repo
zypper refresh
zypper install qownnotes
```

[Download diretto](https://build.opensuse.org/package/binaries/home:pbek:QOwnNotes/desktop/SLE_15)
