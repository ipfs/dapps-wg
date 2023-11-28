---
title: IPFS DApps Working Group
description: This IPFS DApps WG is for those invested in making the experience of IPFS DApps better, whether via improving IPFS implementations or building other tooling. It’s a forcing function to engage on this critical topic for dapp stakeholders and to problem solve together with action.
# image: https://hackmd.io/screenshot.png
---

[![hackmd-github-sync-badge](https://hackmd.io/LS4eUSZXTriQ0j8UIjDl4A/badge)](https://hackmd.io/LS4eUSZXTriQ0j8UIjDl4A)


To join the conversation:
- [📆 **Join the WG meetings**](https://lu.ma/ipfs-dapps)
- [💬 **Take part in the discussions**](https://github.com/ipfs/dapps-wg/discussions)


[toc]

## 2023-11-28

### Attendees
* Adin
* Russell
* lidel
* Yiannis
* Ed (Liquidy)
* Daniel Norman
* David Justice
* Henrique Dias
* Luke Schoen


### Agenda

- Group Intro
- People Intros
- Discussion topics
  - What is the highest priority from now to end of 2024Q1? (question to find answers? spec to design? thign to prototype? tool/library to write?)
      - how does end game look like for loading dapp trustlessly (e.g. Brave user and native ipfs:// and ipns://)
  - What are some example dApps with issues that Adin mentioned?
  - Should we build (a) IPFS dApp Hackathon template(s) for hackers? (starter kit, etc) 
  - We are still dependent upon DNS for accessing "apps", is there a way around that (without all browsers natively supporting IPFS) or will we always be limited to centralizing where DNS is concerned?
  - ADD ITEM HERE

### Meeting Notes

- ED: The part of the stack that was hardest to decentralise the frontend
- Figuring out what are the basic tools we need to give developers publishing their dapps through IPFS
- Dapps are using IPFS to varying degrees as part of their frontend distribution. Some are just publishing to IPFS as a plan B
- A big part of what we want to do is figure out what it *could* look like
- Ed: 
    - Ideally browsers would be able to speak IPFS natively without developing too many custom solutions
    - How to achieve maximum trustlessness while recognising that with existing browsers some trust is delegated. 
    - Can we move away from trusted hosts serving the frontends, to a world where **normal** users can download and verify frontends while being compatible with the web2 experience.
    - Why?
        - User protection: DNS takeovers leading to lost funds due to malicious frontends being served to users.
        - Legal: MiCA and the European legislation coming to DeFi has criteria for what gets categorised as decentralised, e.g., number of frontends being hosted.
- Adin: 
    - Some of the work that brings us closer to EOY goals
    - Currently, Liquidy has the install.land experiment.
    - Step 1: 
        - We should get it to fetch from a trusless gateway
        - Support ENS resolution
        - Supporting the various frontend requirements
        - With this, we still rely on a centralised point to do the ENS resolution and verification 
        - Get UX feedback 
    - Step 2:
        - Move more of the fetching and verification to the browser
- Ed:
    - Many users already have some browser extensions or Brave which assist in ENS resolution
    - If a user doesn't have any of those, and you end up using a single resolver like eth.limo or eth.link 
- Lidel:
    - Do we know when the MiCA is getting rattified and its implications on projects?
    - How can we leverage the fact that you can prove that you have multiple providers for a given CID. 
    - How do we design the scenario where we don't make trust concession and work back from that giving the user the full spectrum of trustlessness 
    - How do we leverage the fact that we already have multiple gateways 
- Ed: 
    - install.land is **not** the thing we actually want it to be
    - The goal would for install.land to be a frontend deployed to IPFS with an ENS link
    - "real installer should be requested from installer.eth (browser can somehow resolve ENS) or installer.eth.limo - we degrade to a public ENS gateway. In both cases the installer is an IPFS CID"
    - The weakest link would be the ENS resolver
- Possible user journeys for loading a Dapp:
    - Immmutable request: user asks for a CID
    - mutable request: First resolve ENS/IPNS/DNSLink, and then the CID
    - Pet name: user asks for a CID only the first time, user gives a pet name and the
- Ed: DNS vs. ENS
    - We see those as different, where DNS is opaque in comparison to ENS which is open source and transparent
- Ed: 
    - Installer isn't the ideal use
- Lidel:
    - The benefits of mutable pet names is that they allow upgrades that retain the origin which is important because of all local browser data associated with the origin, e.g. localstorage
-  ED: 
    - immutable pet names: uniswap.installer.eth <- auto routes uniswap.eth contentsource IPFS CID and stores it immutably - doesn't matter if uniswap.eth record is mutable, wouldn't affect existing users, only new users.
- Adin:
    - The only way for Kubo to resolve the _redirects or the basic web pathing (index.html resolution) is via the gateway. Not possible via the CLI
    - Currently, it's tied to the gateway implementation
    - It should be possible to do all of this independently of a gateway

### Next steps:
  - Library for fully compliant gateway resolution in JS (_redirects, index.html, ...) that can be used by Dapp developers and imported into their service worker.
  - Service worker integration that handles relative paths, trustless gateway fetching, etc.
  - Installer UX integration with service worker
  - Background tasks
     - adin: Reach out around the verifiable ETH RPC
     - lidel: spec local pet names proposal for gateways
  - Ed: install.land ENS deploy + architecture diagram 
