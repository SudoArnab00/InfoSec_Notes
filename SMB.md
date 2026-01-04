
## SMB - Server Message Block
Allows clients to communicate with a server like file share.
In networks that use Microsoft AD, SMB governs everything from inter-network file-sharing to remote administration

On large Windows networks, these protocols allow hosts to perform their own local DNS resolution for all hosts on the same local network.
Rather than overburdening network resources such as the DNS servers, hosts can first attempt to determine if the host they are looking for is on the same local network by sending out [LLMNR](/LLMNR.md) requests and seeing if any hosts respond. The [NBT-NS](/NBT-NS.md) is the precursor protocol to LLMNR, and WPAD requests are made to try and find a proxy for future HTTP(s) connections. 



### Attack
We can use our rogue device to stage a man in the middle attack, relaying the SMB authentication between the client and server, which will provide us with an active authenticated session and access to the target server. 
Since these protocols rely on requests broadcasted on the local network, our rogue device would also receive these requests. Usually, these requests would simply be dropped since they were not meant for our host. However, Responder will actively listen to the requests and send poisoned responses telling the requesting host that our IP is associated with the requested hostname. By poisoning these requests, Responder attempts to force the client to connect to our AttackBox. In the same line, it starts to host several servers such as SMB, HTTP, SQL, and others to capture these requests and force authentication. 

**Things to consider-**
We need a couple of things to play in our favour:
- SMB Signing should either be disabled or enabled but not enforced. When we perform a relay, we make minor changes to the request to pass it along. If SMB signing is enabled, we won't be able to forge the message signature, meaning the server would reject it.
- The associated account needs the relevant permissions on the server to access the requested resources. Ideally, we are looking to relay the challenge and response of an account with administrative privileges over the server, as this would allow us to gain a foothold on the host.
- Since we technically don't yet have an AD foothold, some guesswork is involved into what accounts will have permissions on which hosts. If we had already breached AD, we could perform some initial enumeration first, which is usually the case.
