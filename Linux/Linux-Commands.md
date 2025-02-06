## Linux Commands

<br/>

### Unattended Silent Updates & Upgrades Without User Interaction: Type it and go about your work.
###### I'm aware some of this stuff may be redundant, these are actually that way on purpose, to make sure that for example, the updates, are actually finished. <p></p>
**Updates & Upgrades, No Release Upgrade:** <br/>`apt update && apt upgrade -y && apt update && apt dist-upgrade -y && apt update && apt upgrade`<p></p>
**Updates, Upgrades, _With_ Release Upgrade:** <br/>`apt update && apt upgrade -y && apt update && apt dist-upgrade -y && apt update && apt upgrade && do-release-upgrade` <br/>

<br/>

### Common Tools and Packages We Use
Common tools, packages, including some basic troubleshooting tools that are invaluable on pretty much any Linux system. <br/>

**Commons** <br/>
`apt install curl wget dnsutils net-tools ndisc6 nano openssh-server htop inetutils-ping tmux` <br/>

**Networking Specific** <br/>
`apt install tcpdump netcat`
