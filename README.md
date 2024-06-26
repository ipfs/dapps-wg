# IPFS DApps Working Group

[![hackmd-github-sync-badge](https://hackmd.io/LS4eUSZXTriQ0j8UIjDl4A/badge)](https://hackmd.io/LS4eUSZXTriQ0j8UIjDl4A)

- [📆 **Join the WG meetings**](https://lu.ma/ipfs?tag=dapps)
- [🎥 **Watch previous meetings**](https://youtube.com/playlist?list=PLuhRWgmPaHtRdxz9aOAJyJxbO7hJZVPL6&feature=shared)

### Problem statement: trust or verify

Today, IPFS stands out as the predominant decentralized network for hosting dapp frontends and static assets, such as NFT images. Nevertheless, users commonly retrieve these CIDs from trusted gateways with browsers without verifying. This undermines the benefits of verifiablity in IPFS, as users place implicit trust in gateways, leaving them vulnerable to various attacks.

The challenge of verifying CIDs within the browser context varies depending on what the CID is. It proves more straightforward to verify static assets like images and JSON compared to verifying the CID of a frontend. In the case of a frontend's CID, the only viable method involves running a separate IPFS node that exposes a gateway for verification purposes.


### High level goals for the IPFS Dapps WG

- Establish verified retrieval as the norm for retrieving CIDs on the web
- Decrease the reliance on trusted gateways
- Improve the experience of Dapps on IPFS with better tooling, both for developers and users

### Concrete goals for Q1

- **IPFS Shipyard:** Fetch like library [`@helia/verified-fetch`](https://github.com/ipfs/helia-verified-fetch) for verified in-browser retrieval of CIDs with better DX
    - Initially support configurable [trustless gateways](https://specs.ipfs.tech/http-gateways/trustless-gateway/), e.g. `ipfs.io`, `trustless-gateway.link`, `cloudflare-ipfs.com` or self hosted.
    - Later uses [delegated routing HTTP endpoint](https://specs.ipfs.tech/routing/http-routing-v1/) and direct retrieval 
    - Can be integrated into a service worker
    - See [`@helia/verified-fetch`](https://github.com/ipfs/helia/tree/main/packages/verified-fetch)
- Research and prototype tooling to help users of dapps on IPFS reduce trust through verification
    - “Top level”/”end-to-end integrity” Dapp verification **without** Kubo
        - Ed (liquity): [Local Dapp installer](https://www.liquity.org/blog/decentralizing-defi-frontends-protecting-users-and-protocol-authors)
        - Ed: [Browser extension using Chrome’s debugger api to verify hash of initial payload](https://github.com/edmulraney/app-integrity-verifier-extension)
    - **IPFS Shipyard:** Service worker in-browser gateway built on top of  `@helia/verified-fetch`
- Advocate for verified retrieval
    - Read-to-run examples
    - Docs
    - Blog posts

## Meeting 12 (23-04-2024)

### Agenda
* Housekeeping: [New Calendar link on Luma](https://lu.ma/ipfs?tag=dapps)
* Status updates on ongoing initiatives
    * [Helia](https://github.com/ipfs/helia/)
        * Sessions shipped last week
            * Sending requests to a subset of peers, i.e. "narrowing"
            * [Sessions with trustless gateways](https://github.com/ipfs/helia/pull/515):
                * We take the results from the delegated routing endpoint
                * We don't really get that many actionable/usable gateway from the delegated routing endpoint
                * The gateway block broker currently has a bunch of static gateways
                * The idea is to refactor the code a bit so that these preconfigured static gateways implement the routing interface.
        * Problems with someguy peer routing
            * https://github.com/ipfs/someguy/issues/53
            * No active probing, means it would sometimes just return providers with no peer records
            * Recent Boxo release filters out private IPs (impacts someguy)
        * [Helia Benchmarks are also merged and looking good](https://github.com/ipfs/helia/tree/main/benchmarks/transfer)
        * 
    * [Verified-fetch](https://github.com/ipfs/helia-verified-fetch/)
        * [Announcement Blog post](https://blog.ipfs.tech/verified-fetch/)
    * [Service Worker Gateway](https://github.com/ipfs-shipyard/service-worker-gateway)
        * Currently uses helia/http

### Notes

```
add --cid-version 1 -Q  -n
bafkreibel3v5edjm2tebwxkkyj6hg5dcph2dnvrph32syrjkg2pg555wca

ipfs routing findprovs bafkreibel3v5edjm2tebwxkkyj6hg5dcph2dnvrph32syrjkg2pg555wca → relay providers
```


## Meeting 11 (9-4-2024)

### Agenda

* Status updates on ongoing initiatives
    * Helia
        * TTL is respected in IPNS records for cache
        * Bitswap/Trustless Gateway sessions
            * One of the immediate benefits is that we are not constantly hitting the same configured gateways in favour of a more diverse set of providers.
            * Sessions are a way to scope block requests to peers based on which peers are providing the root block
            * Initial testing reveals this is more efficient
            * Question: How are those gateways discovered/selected?
                * You can configure two things: 
                    * recursive gateways (ones that fetch blocks they don't have) 
                    * delegated routing endpoints aka routers
                * results from the delegated routing endpoint will be used to fetch individual blocks
            * Question: how far are we from being able to put Helia in my browser extension and making data added there discoverable to the network?
                * (see recording for Adin's full answer)
                * TL;DR: currently connectivity poses a limitation on the content routing options you can use. With WebTransport and WebRTC  
    * [Service worker gateway](https://github.com/ipfs-shipyard/service-worker-gateway/)
        * new [README](https://github.com/ipfs-shipyard/service-worker-gateway/#readme) with project goals
        * wip [productization](https://github.com/ipfs-shipyard/helia-service-worker-gateway/issues/31) / [milestones](https://github.com/ipfs-shipyard/service-worker-gateway/milestones)
        * Noteworthy here that once helia is released with sessions work, SW gateway would be able to leverage direct retrival 
        * Gateway conformance tests: Russel is working on integrating the existing conformance tests into the SW gateway release process
    * [@helia/verified-fetch](https://github.com/ipfs/helia-verified-fetch)
    * ipfs/specs:
        * [IPIP-462: Ipfs-Path-Affinity on Gateways](https://github.com/ipfs/specs/pull/462)
    * Firefox beta has WebTransport + certificates hashes! Stable is out next week
    * Meet Interplanetary Shipyard: https://blog.ipfs.tech/shipyard-hello-world/
    * [Salief](https://github.com/salieflewis) from [River](https://www.river.ph)
        * [Source code](https://github.com/1ifeworld/river)
        * [Email salief](salief@lifeworld.co)

### Meeting notes

### Action items

- [] Document recursive vs. non-recursive gateways
- [name=Daniel Norman] Add to SW gateway README how you integrate into your app, or existing sw


## Meeting 10 (26-3-2024)

### Agenda
* Status updates on ongoing initiatives
    * [Service worker gateway](https://github.com/ipfs-shipyard/service-worker-gateway/)
        * TL;DR: Still a lot of work in progress until can be used instead of `dweb.link`. 
        * An instance at `inbrowser.dev` WIP: Problems with DNSLink leading to problems loading the "loader", i.e. the SW gateway
        * [Epic: service-worker-gateway Productionisation](https://github.com/ipfs-shipyard/service-worker-gateway/issues/31)
    * [@helia/verified-fetch](https://github.com/ipfs/helia-verified-fetch)
        * Example: https://github.com/ipfs-examples/helia-examples/pull/285
        * TL;DR: Mostly ready (HAMT, index.html for web hosting, HTTP URLs)
    * ipfs/specs: [IPIP-462: Ipfs-Path-Affinity on Gateways](https://github.com/ipfs/specs/pull/462)
        * TL;DR: Decrease routing latency and the reliance on fetching all blocks from a single gateway

### Meeting notes


## Meeting 9 (12-3-2024)

### Agenda
* Status updates on ongoing initiatives
    * [Service worker gateway](https://github.com/ipfs-shipyard/helia-service-worker-gateway/)
       * HAMT support landed
    * [@helia/verified-fetch](https://github.com/ipfs/helia-verified-fetch)
        * range requests
        * DNS over HTTPS endpoints in Helia (https://github.com/ipfs/helia-verified-fetch/pull/13)


### Meeting notes

- improved cache-control in someguy responses: https://github.com/ipfs/boxo/pull/584 (no released yet)
- Default IPNS values
    - Boxo/Kubo: 
        - lifetime: ~48 hours
        - TTL: 1 hour
* Somehow related, Delegated Routing via HTTP /routing/v1 can be tracked in:
  - https://specs.ipfs.tech/routing/http-routing-v1/
      - read-only routing server: https://github.com/ipfs/someguy
  - [IPIP-379](https://github.com/ipfs/specs/pull/379) introduces signed provider and peer records which can be delegated to other services
  - wip implementation of server with support of POST /routing/v1: https://github.com/ipfs/someguy/pull/40



## Meeting 8 (27-2-2024)

### Agenda
* Status updates on ongoing initiatives
    * [Service worker gateway](https://github.com/ipfs-shipyard/helia-service-worker-gateway/)
        * Development deployment for tracking progress at `inbrowser.dev`. 
            * Demo: https://docs.ipfs.tech/ website loaded and verified in SW: https://docs-ipfs-tech.ipns.inbrowser.dev
            * Remaining work for MVP: https://github.com/ipfs-shipyard/helia-service-worker-gateway/issues/31
        * We will move it at some point to the ipfs org, but postponing this to avoid breaking deployment to fleek.
* [@verified-fetch](https://github.com/ipfs/helia/tree/main/packages/verified-fetch)
    * Besides the HAMT fix, we should be good to go
    * Could release the more flexible API after (with support for traditional HTTPS gateway urls) 
    * Could move out of the helia mono-repo

## Meeting 7 (13-2-2024)

### Agenda
* Status updates on ongoing initiatives
    * @helia/verified-fetch
        * Merged [initial PR](https://github.com/ipfs/helia/pull/392/)
        * Open discussion around how to handle [dag-json and dag-cbor](https://github.com/ipfs/helia/pull/426)
        * [Browser example PR in helia-example repo](https://github.com/ipfs-examples/helia-examples/pull/285) 
    * Service worker gateway
        * https://github.com/ipfs-shipyard/helia-service-worker-gateway/pull/17
        * Demo: https://blog-libp2p-io.ipns.sw.sgtpooki.com/

### Meeting notes

- Try loading the following CID from the SW gateway: https://ipfs.io/ipfs/QmYXenhk6uREiF6zWDCCEv3doNYManYTaqiJBqPLJwUBbq

### Attendees
 - Achingbrain
 - Tj (https://tap.spaceaware.io/ https://github.com/digitalarsenal/spacedatastandards.org)
 - Russel
 - Adin
 - Lidel
 - Kanishk (Fleek)
 - Mohsin (Ceramic)

## Meeting 6 (30-1-2024)

[**▶️ Meeting Recording**](https://www.youtube.com/watch?v=5n3ieAb-FWE)

### Agenda

* Status updates on ongoing initiatives
   * https://github.com/ipfs/helia/pull/392/
   * https://github.com/ipfs/helia-http-gateway/pull/63
   * https://github.com/ipfs-shipyard/helia-service-worker-gateway/pull/17
   * blog.ipfs.tech: [The State of Dapps on IPFS: Trust vs. Verification](https://blog.ipfs.tech/dapps-ipfs/)
* Russell: What we want to initially test against
   * ipns://app.aave.com
   * ipns://app.spark.fi
   * ipns://app.olympusdao.finance
* https://www.dpid.org/ (CID-based,  ISBN-like decentralized identifiers with greated transparency and verifiability)
    * https://desci.com/ example https://nodes.desci.com/dpid/46

## Meeting 5 (16-1-2024)

[**▶️ Meeting Recording**](https://www.youtube.com/watch?v=mAha0YR9Qqk)

### Agenda

* Daniel: Update on the WG's goal
* Status updates on ongoing initiatives
    * https://github.com/ipfs/helia/pull/372
    * https://github.com/ipfs/web3-fetch/pull/1
* Hannah: Trustless CAR Verification via ipfs-unixfs-exporter
    * Lack of DFS: https://github.com/ipfs/js-ipfs-unixfs/issues/359
        * Saturn Fork Implementation: https://github.com/filecoin-saturn/js-ipfs-unixfs/commit/ee5a574742b6958d8699f8f0807be023d80a49a0
        * PR with blockReadConcurrency option: https://github.com/ipfs/js-ipfs-unixfs/pull/361
    * Range requests appear to work! (previous report was fake news)
        * Additional Saturn work to make range requests go all the way to service worker
            * https://github.com/filecoin-saturn/js-client/pull/48
            * https://github.com/filecoin-saturn/browser-client/pull/28

### Meeting notes

- Adin: Good news here is the UnixFS/CARs/etc. story is roughly the same story as happened in Go (with bifrost-gateway, etc.). Can mostly replicate the solutions here too 😄
- Alex: You can control concurrency to replicate DFS with the regular importer - set these both to 1
    - https://github.com/ipfs/js-ipfs-unixfs/blob/master/packages/ipfs-unixfs-importer/src/index.ts#L137
    - https://github.com/ipfs/js-ipfs-unixfs/blob/master/packages/ipfs-unixfs-importer/src/index.ts#L143
- Alex (16 Jan 2024, 4:22 pm) As in, you can use those settings to write blocks to the blockstore one at a time, in the order the CAR file expects
- Adin: I recall Rod making some comment about this working for files, but that if downloading an entire directory would traverse the directory as BFS instead of DFS. Might be wrong here though.  Either way Saturn seems unlikely to care here
- Oli: some curious goodies from the dag.haus
    - a CAR-centric re-write of unixfs-importer - https://github.com/ipld/js-unixfs (we use that in the web3.storage upload client as we're focused on streaming CARs rather than persistent blockstores
    - work just started on extracting a block (not unixfs-entity centric) the ipfs dag traversal logic to a stand alone library from unix-fs-exporter and trustless gateway spec (currently trapped in dagula, soon to be standalone lib)
- Hannah
    - Complete work on range requests
    - Ideally they'd like to avoid continuing to use a fork of js-ipfs-unixfs.
- Ben from eth.limo
    - Working on a migration of their infra for better performance
    - Seeing some problems with IPNS records
    - For Q2, exploring trustless gateways, maybe exploring Rainbow
    - Also exploring local installers to reduce trust reliance
    - According to a query on Dune, a large percentage of ENS names are using IPNS
- 
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
