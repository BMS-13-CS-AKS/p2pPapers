## 1. Computer Networks
*David Wetherall; Tanenbaum, Andrew S. (2011). Computer networks. Upper Saddle River, NJ: Pearson Prentice Hall.*

### Content Delivery

- More about content than communication
- Build content delivery networks to handle traffic
- Two architectures : CDN and P2P
- CDN -> distributed collection of machines at locations in internet
- P2P -> collection of computers pool in their resources

#### Content and Internet traffic

- Shift from FTP,Email -> Web -> P2P -> Video stream
- Zipf's law and Zipf's  Distribution = one of Power laws
- Tail can't be ignored, i.e, Unpopular sites matter too for traffic

#### Server Farms and Web Proxies

##### Server Farms

- Use of more than one computer to make a Web Server
- Set of computers must look like a single logical web site
- Use DNS to spread requesta accross the servers in server Farms
- Use front end to spray request over pool of servers
- One way is broadcasting but waste of bandwidth
- Another way is to inspect packet headers and arbitarily map to servers
- Mapping is called load balancing

##### Web Proxies

- Client side
- Web proxy = second level of caching ; First level = browser iteself
- Upstream proxy and downstream Proxies

#### Content Delivery networks

- Server Farms and Web proxies not sufficient for truly popular web sites handling traffic on global scale
- Traditional idea of web caching on server side
- Use of distribution trees
- How to organize clients to utilize CDN ?
- First way : proxy server
- Second way : mirroring
- Third way : DNS redirection (clever use of Name Server)

## 2. Incentives Build Robustness in Bittorrent
*B. Cohen. Incentives build robustness in bittorrent,
May 2003. http://bitconjurer.org/BitTorrent
/bittorrentecon.pdf.*

### Abstract
- Tit-for-tat to obtain pareto efficiency
- cooperative technique

### What bittorrent does

- Redistribution of cost of upload to downloaders

#### Interface

- Link upon clicking given standard dialog
- shows progress dialog (upload and download rate)

#### Deployment

- No. of incomplete downloaders increases rapidly , eventually peaks and then falls off at exponential rate same happens for complete downloaders

### Technical Framework

#### Publishing Content

- static file with .torrent extension
- contains file info, length, name, hashing information, url of tracker
- trackers responsible to find peers

#### Peer distribution

- Return random list of peers from trackers- random graph = good robustness property
- to track which peer have what, divide file into chunks(1/4 mb)
- Each peer reports to other peers what file it has , data integrity applied
- SHA1 hash for file integrity

#### Pipelining

- important to always have several requests pending at once to avoid delay between pieces being sent
- this is done by breaking pieces into further subpieces and always keeping some number of requests pipelined at once

### Piece Selection

- very important for good performance
- poor algorithm can result in having all pieces which are currently on offer, or not having any pieces to upload to peers

#### Strict Priority

- once single sub-piece requested then remaining sub-piece requested before sub-pieces from any other pieces

#### Rarest First

- While selecting which piece to start downloading next, peers download pieces which the fewest of their peers have
- If one seed then better for different peers to get different pieces from seed
- If original seeder offline, also no problem

#### Random First piece

- rarest first not applied to first piece being downloaded as rarest is slow to download

#### Endgame Mode

- Sometime piece requested from peer having slow transfer rate
- Request for actively requested sub-pieces sent to all peers

### Choking Algorithms

- cooperate = upload, not cooperate = choke
- choking = temporary refusal to upload

#### Pareto efficiency

- pareto efficient = no two counterparties can make exchange and both be happy
- in bittorrent, if peers are getting poor reciprocation then can choke and find other peers

#### Bittorrent's Choking Algorithms

- each bittorrent peer always unchokes fixed other peers (no. =4)
- bittorrent peers calculate who they want to choke once every ten seconds

#### Optimistic Unchoking

- at all times, a bittorrent peer has a single 'optimistic choke' which is unchoked regardless of current download rate

### Anti-snubbing

- when over a minute goes without getting a single peice from a particular peer, bittorrent assumes it is 'snubbed' by that peer and doesn't upload to it

### Upload-only

- Once done downloading, seeds to those which one-one else happens to be uploading at the moment

## 3. The BitTorrent Protocol Specification
*BEP_0003. The BitTorrent Protocol Specification, Cohen. http://www.bittorrent.org/beps/bep_0003.html*

- protocol for distributing files
- identifies content by url and seamlesly integrates with web

### Entities

- ordinary web server
- static metainfo file
- bittorrent trackers
- original downloaders
- end user web browsers
- end user downloaders

### Serving file

- start tracker
- start web server
- set mimetype application/x-bittorrent
- generate torrentfile
- put torrentfile into webserver
- link the torrent file
- start downloader which already has complete file

### To start downloading

- install bittorrent
- surf web
- click .torrent file
- select where to save locally
- wait for download to complete
- tell downloader to exit

### bencoding

- string 4:spam
- integer i3e, i-3e, i0e
- lists l4:spam4:eggse
- dictionaries d:3:cow3:moo4:spam4:eggse = {'cow':'moo', 'spam':eggs'} keys must be string and appear in sorted order

### metainfo files

- torrent files are bencoded dictionaries with following keys:
  - **announce** : url of tracker
  - **info** : dictionary

- all strings in torrent file must be utf-8 encoded

#### info dictionary

- name => maps to string which is suggested name to save the file
- piece length => number of bytes in each piece the file is split into ; usually power of 2 (256K)
- pieces => string whose length is multiple of 20; each string sha1 hash of corresponding index
- length / files : if length then there is single file otherwise hierarchy; length maps to length of files in bytes; files maps to list of dictionary with keys length and path

### trackers

trackers GET request has following keys

- info_hash : 20 byte sha1 hash of info value from metainfo file
- peer_id : 20 length string ; randomly generated at start of new download
- ip : optional paramater giving ip address or dns name
- port : port no. peer is listening on commonly 6881
- uploaded : total amount uploaded so far
- downloaded : total amount downloaded so far
- left : number of bytes peer still has to download
- event : optional key which maps to started , completed , or stopped

Tracker responses are bencoded dictionaries. if failure reason key is present then maps to human readable string reasoning the failure. Otherwise, two keys **interval** and **peers**

it is common to announce over udp tracker as well

### peer Protocol

- operates over tcp or utp
- peer connections are symmetrical
- refers to piece of file by index; when finishes downloading check if hash matches and announces that it has piece to all of its peers
- connections contain two bits of states : choked or not, and interested or not
- data transfer happens when one side is interested and other side is not choking
- handshaking and all happens

### peer messages

- choke
- unchoke
- interested
- not interested
- have
- bitfield
- request
- piece
- cancel

## 4. The Bittorrent P2P File-Sharing System: Measurements and Analysis
*Pouwelse, Johan; et al. (2005). "The Bittorrent P2P File-Sharing System: Measurements and Analysis". Peer-to-Peer Systems IV. Berlin: Springer. pp. 205–216. doi:10.1007/11558989_19. ISBN 978-3-540-29068-1. Retrieved 4 September 2011.*

### Abstract

- Relies on other components for file search
- Moderator shystem to ensure data integrity
- bartering technique to prevent freeriders
- measurement study focusing on four issues
- avaibility, integrity, flashcrowd handling, and download performance

### Introduction

- Detailed measurement study of combination of bittorrent and supernova
- 8 months traces(jun 03 to march 04) of > 2000 global components

### Bittorrent file-sharing System

- bittorrent and supernova form a unique infrastructure
- uses mirroring for web servers
- meta-data distribution for load balancing
- bartering technique for fair resource sharing
- p2p moderation system to filter fake files

### Experimental Setup

- Measurement software consist of two parts with three scripts each
- first part : used for monitoring global components
  - mirror script : measures avaibility and response time of supernova mirrors
  - html script : scrapes html pages and downloads all torrent files
  - tracker script : parses torrent for new trackers and checks status of all trackers
- second part : monitor actual peers(used 100 nodes of distributed supercomputer)
  - hunt script : selects file to follow and initiates measurement of all peers downloading this particular file
  - getpeer script : contacts tracker for given file and gathers ip addresses of peers downloading the file
  - peerping script : contacts numerous peers parallely and abuses bittorrent protocol to measure their download progress and uptime

### Measurement Results

#### Overall system activity

- No. of active users in the system is strongly influenced by avaibility of global acomponents

#### avaibility

- to increase avaibility of the whole system, functionality of gloval components have to be distributed , possibly accross the ordinary peers.
- to do so peers should be given Incentives

#### integrity

- moderation = effective

### flashcrowd

- global components can effectively handle flashcrowds

## Discussion and conclusion

- decentralization solution for avaibility but needs further reasearch
- incentives to seed
