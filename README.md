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

## Meeting 4 (2-1-2024)

[**▶️ Meeting Recording**](https://youtu.be/uQQZtW3SOuI)

### Attendees
- adin (@aschmahmann)
- lidel
- Daniel
- Henrique
- Derrick Hammer
- Fred
- Robin


### Agenda & Meeting notes

- Recap previous session
    - Debugger API in Ed's extension
    - Lidel: using the API comes with a performance penalty
        - The debugger API is used by WebRecorder to take a snapshot of the page. In that instance it's ok in terms of the user experience.
        - This may be a problem for users 
        - What do you do when the verification fails?
    - How could this be productized: 
        - Extension per Dapp which has the trust anchored through a hard coded CID
        - Make it more generic and use an external trust anchor, i.e. DNSLink/ENS resolution. 
    - The Blocking API
        - Only allows blocking or redirection.
        - With manifest v3 we have less capabilities, even though you can still do the same things in an extension with more steps.
    - What we're missing:
        - https://github.com/ipfs/in-web-browsers/issues/212
        - This would take time
    - Derrick:
        - Work on the extension and overcome some challenges around origin isolation: https://git.lumeweb.com/LumeWeb/extension
        - https://git.lumeweb.com/LumeWeb/libkernel/src/branch/develop
        - https://github.com/SkynetLabs/skynet-kernel
- Moving this meeting an hour earlier 
    - No opposition based on vote in call.

### Links

---

## Meeting 3 (19-12-2023)

[**▶️ Meeting Recording**](https://www.youtube.com/watch?v=3HLpjGpq94U)

### Attendees

- Ed
- Adin
- Robin Berjon
- Bengo
- David Justice
- Daniel Norman
- Hannah 
- Mosh
- Rohit
- Harrison Hines

### Agenda

- Ed: [app-integrity-verifier-extension](https://github.com/edmulraney/app-integrity-verifier-extension)
    - This is currently relying on Chrome specific APIs
    - What about supply chain attacks on the distribution of the extension source
        - no concrete thoughtsd
    - next steps:
        - combine this with ipfs:// protocol handler
        - See https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/manifest.json/protocol_handlers
        - Since this is just for verifying the top level resource, when doing this with IPFS, using a raw block for the top level document would ensure easy matching of the CIDs
- https://www.liquity.org//blog/decentralizing-defi-frontends-protecting-users-and-protocol-authors
    - Robin: This is super interesting. A few questions to make sure that I understand the proposal well:
    1. The apps that are installed using this mechanism are then fully offline and can only interact with browser APIs (including the wallet), right? I assume that they are served locally with a strict CSP of sorts?
        - No restrictions or changes. The main difference is that the service worker is pulling the front end's source code and making it available on the local device.
    3. I got a covid booster so my brain isn't fully there, but it seems unclear to me what the installer is in terms of format. Does it need to be more than a manifest?
    4. I'm guessing it's not an immediate concern, but is there interest in private loading of installers? It seems like it could be a plus. (Especially if installing an app means it will regularly phone home to check for updates.)
    5. As always, the client is the weak spot (c.f. what Chrome does with people's data). Have you considered what (legal) rules it needs to operate under to durably ensure alignment with the user?
    6. One of IPFS's weak spots in security is media types. If everyone's on the exact same client it might be less of an issue but to build something rock solid it would be good to nail this. Have you given thought to that?
    - Adin: What are the downsides/limitations to `chrome.debugging`? Example, why isn't everyone struggling with the move from blocking requests in MV3 doing this?
    - 
    - Ed: The idea is to make this as simple as possible without making too many assumptions about the app. 
    - There's obviously still the risk of dapps doing "stupid things" that expose them to a wider risk surface by loading subresources from other origins and supply chain attacks. 
    - This was not build to improve the security of the dapp. Rather it's focused on ensuring local operation and verifiability/integrity. 
    - Robin: not sure if the legal side is solved without ensuring that none of the sub-resources are also not hosted on centralised infrastructure.
    - Ed: This is addressing the problem of ensuring the user is running the same code published by the dapp developer. It doesn't solve the supply chain problem of attacks on specific dependencies, e.g. the ledger SDK.
    - Hannah: would be nice to have the web subresource integrity spec support images and other tags. 
    - Ben: Could we just pass static files around, i.e. index.html
    - Robin: You need the extension to verify the root resource you’re loading.
-  Helia Fetch efforts
    - https://github.com/ipfs/helia/issues/348
- [IPFS Signer](https://signer.ipfs.garden/): 
    - Experimental playground for IPFS & Dapp patterns in browsers

### Action items
- Investigate whether we have any problems with range requests using the js-unixfs-exporter
- 

### Links
* https://github.com/edmulraney/app-integrity-verifier-extension?tab=readme-ov-file
* https://blog.ipfs.tech/2023-ipfs-companion-mv3-update/#what-is-mv3
* https://github.com/olizilla/see-other
* This is a good video of Mauve's ipfs `fetch` from Iceland 2022 https://www.youtube.com/watch?v=_omtM02_uYw
* https://github.com/web3-storage/dagula
* https://github.com/pknowles/ImageFilter/blob/master/imagefinder.js
* https://github.com/filecoin-saturn/js-client
* https://github.com/filecoin-saturn/browser-client
* "Here’s some helia fetching via bitswap via content-claims in js 'helia bitswap + content-claims' hot off the press" -bengo  https://github.com/web3-storage/hoverboard/pull/20/files#diff-0386cb667aac3b2cdcfcbc94c8ff1027f42ed5945e2835d9fe3d3f40565f919cR136

---
## Meeting 2 (5-12-2023)

[**Recording**](https://www.youtube.com/watch?v=eiGGnOdcAVs)

### Attendees
* Adin
* Ed (Liquity)
* Daniel Norman
* David Justice
* Dietrich Ayala
* Lidel
* Russell


### Agenda
- Revisit the next steps from the previous session
  - Library for fully compliant gateway resolution in JS (_redirects, index.html, ...) that can be used by Dapp developers and imported into their service worker.
    - There's some progress on the base components in Helia. 
    - This includes work on the [Helia gateway implementation](https://github.com/ipfs/helia-http-gateway/)
        - Conformance tests to ensure compatibility
    - Additionally there's work by ProbeLab to monitor performance
  - Service worker integration that handles relative paths, trustless gateway fetching, etc.
      - https://github.com/ipfs-shipyard/helia-service-worker-gateway
  - Installer UX integration with service worker
 - adin: Reach out around the verifiable ETH RPC
   - I've reached out to some people working on Helios (https://github.com/a16z/helios)
 - lidel: spec local pet names proposal for gateways
     - Some progress on the proposal, working to ensure it fits into the web's security model
     - Still some open questions to resolve before sharing the draft
     - Some research on where to store the pet name mappings and making them available in the different subdomains
  - Ed: install.land ENS deploy + architecture diagram 
      - Some progress on the [architecture diagram](https://excalidraw.com/#json=g8BJsFitQRcMOAmpKqX5p,MMwIbiLamBNPiuMwm03hjg)
- Pet Names: a local "user-friendly" mapping from a user-provided string to a CID
- Ed:
    - Working on a write-up
    - Working with their legal council to determine whether the approach that their exploring meets the legal requirements of the current regulation.
- Ed:
    - Top level integrity is one of the main challenges facing 
- Adin:
    - Reached out to Helios devs
    - Seems feasible to verify ENS names in the browser


### Next steps
- Russell:
    - Working on the Helia gateway
    - Monitoring Helia gateway performance with the help of ProbeLab
    - Abstracting some of the IPFS fetch stuff to be more generic and usable in the browser too
- Daniel: 
    - Write up on the current state of matters for IPFS docs
- Lidel:
    - Work on a the draft spec for the pet name proposal and local resolution 
- Ed:
    - Continue with the architecture diagram and the article
    - Hopefuly get to deploying the install.land approach to IPFS
    - Liquity hosting a Twitter space with some of legal folks. Rokti devs will also be joining. Would be good to get us to also join.


## Meeting 1 (28-11-2023)

[**Recording**](https://www.youtube.com/watch?v=KVYze_VqWnI)

### Attendees
* Adin
* Russell
* lidel
* Yiannis
* Ed (Liquity)
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
