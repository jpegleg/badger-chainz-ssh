# badger-chainz-ssh 🦡
Read ssh logs and create additional copies of each line as events in a chain.

A FAIL_MARK file is created when a new chain is started.
A new chain is started if ssh_host_X_key files get modified, added, or deleted.
```
20220113213044 547b-fb03-a428-1 chain integrity NEW-CHAIN-START <<<---(( total 636K
-rw-r--r--   1 root root  395 Sep 27  2020 ssh_host_rsa_key.pub
-rw-------   1 root root 1.8K Sep 27  2020 ssh_host_rsa_key
-rw-r--r--   1 root root  175 Sep 27  2020 ssh_host_ecdsa_key.pub
-rw-------   1 root root  505 Sep 27  2020 ssh_host_ecdsa_key
-rw-------   1 root root  399 Sep 27  2020 ssh_host_ed25519_key
-rw-r--r--   1 root root   95 Sep 27  2020 ssh_host_ed25519_key.pub
drwxr-xr-x   2 root root 4.0K Mar 13  2021 sshd_config.d
drwxr-xr-x   2 root root 4.0K Mar 13  2021 ssh_config.d
-rw-r--r--   1 root root 1.7K Mar 13  2021 ssh_config
-rw-r--r--   1 root root 565K Mar 13  2021 moduli
-rw-r--r--   1 root root 3.3K Oct  1 10:54 sshd_config
-rw-r--r--   1 root root  603 Jan 13 20:55 ssh_host_dsa_key.pub
-rw-r--r--   1 root root  603 Jan 13 21:00 ssh_host_dsa2_key.pub
drwxr-xr-x   4 root root 4.0K Jan 13 21:16 .
drwxr-xr-x 127 root root  12K Jan 13 21:24 ..
-rw-r--r--   1 root root   12 Jan 13 21:30 ssh_host_dsa2_key
SHA1 checksums:
46080071627cd96df619124aab77ef4db1c1b80f  /etc/ssh/ssh_host_dsa2_key
43b77caccd19536cf57c603757cad7ed6a2be9d8  /etc/ssh/ssh_host_ecdsa_key
17d26e4ccdc47ca9c39d85f4b4dc68cadc1051dc  /etc/ssh/ssh_host_ed25519_key
6f2fa8da539034e00e38a23f6ba13910e19d2c46  /etc/ssh/ssh_host_rsa_key
BLAKE2 checksums:
f75936ab5fd291af997c8511328a0952b36f673cf9fb8201fc7773d0b30da31cba6146ac37241453525bfda89a6b39e1082850e736412f51667c1b1496fe1ff5  /etc/ssh/ssh_host_dsa2_key
cb2a8e74fe82e1a0aeec7c5cf6a5ccbbefa197a80c91aa943e25168d66aeea976414ab5194a62a20ab74e4169a5a6b7c1bc39fbdfcc656047afceccd9b09341d  /etc/ssh/ssh_host_ecdsa_key
4a122b58d173232254a4f4dfbc0062308c58634bb23480b43453030b0fae7c7ce76f0dbefc9e28e38b1946ad8e371d249cbd2aef3fa683a549cadc20cc6a6f67  /etc/ssh/ssh_host_ed25519_key
0915c4cf492781940e033a8cd6bc22ba1381950c2cb6f59d65c3a0c3ca8f04690e97acf76465b4405cc667ffdf4d30f9782d2d4718c19c8b93aee5041f3c97df  /etc/ssh/ssh_host_rsa_key
full hash used to create nodeid of 547b-fb03-a428-1:
547bfb03a4281bbd5a5dbd92e59a63aec31c1ab55d13e2955ff35e74554a4aa5200c9dc542bb6eeda4e8b470496f668100a6d5b071e17016dab9fb6de9ece4c0  -
```

Here is an example of a small chain just getting started:
```
20220113213044248332998 786a02f742015903c6c6fd852552d272912f4740e15847618a86e217f71f5419d25e1031afee585313896444934eb04b903a685b1448b755d56f701afe9be2ce my-hostname_547b-fb03-a428-1
20220113213044250478116 786a02f742015903c6c6fd852552d272912f4740e15847618a86e217f71f5419d25e1031afee585313896444934eb04b903a685b1448b755d56f701afe9be2ce my-hostname_547b-fb03-a428-1
20220113213957202215352 2f589808db440c49c3c6633373d90f4dfd585141a31f06ccaa77c1de99b5d65a15b735cb7c1a66af9a4434780889efebb38f6b9a2d99475a6b37f0bc39bc6a0f my-hostname_547b-fb03-a428-1
20220113213957203179681 2f589808db440c49c3c6633373d90f4dfd585141a31f06ccaa77c1de99b5d65a15b735cb7c1a66af9a4434780889efebb38f6b9a2d99475a6b37f0bc39bc6a0f my-hostname_547b-fb03-a428-1
20220113213959204233043 c2d5d5c54bb8a5a4a25bdd547fe9655e919d4abcefdb2b22685f08636df78806231dde04f2d0a9cc67c65790ce2eec0015ec79dd30c03c11119c39249517474f my-hostname_547b-fb03-a428-1
20220113214000221470258 5e95cdf375681565c843bfd0386624e631ddb37426479c66dfddc13388626a5915d9430aceb42a5796928f1fbe34b748406de924e9af223f281538d578be930e my-hostname_547b-fb03-a428-1
20220113214000422961912 e1efced29ed300995b6b28cedb3a746c6d8893c36beaaf6e97b1f5e23977451c9a67d3377b6244501b1e4eb62945df03576135ce745914d81a92980ff0ecb9b6 my-hostname_547b-fb03-a428-1
20220113214000424489687 e1efced29ed300995b6b28cedb3a746c6d8893c36beaaf6e97b1f5e23977451c9a67d3377b6244501b1e4eb62945df03576135ce745914d81a92980ff0ecb9b6 my-hostname_547b-fb03-a428-1
20220113214001431182422 9eab421a3fe9327d47e354df74704bfe5b35cdef569d9551b6eb7e42bee522b139026c3b3d37312a8ef440df74a370c962cdfebbb314f79bd20d6d91d30d738a my-hostname_547b-fb03-a428-1
20220113214002436673517 0c09a50c01f6f4822fa518d5975be3110fcd31f854e1be14b4e7f22ba3e023d4a7297263d315da1cf98ddb8fca180bdd6a0846a4986efc0b9599ef2df3bea9d9 my-hostname_547b-fb03-a428-1
20220113214002438455395 0c09a50c01f6f4822fa518d5975be3110fcd31f854e1be14b4e7f22ba3e023d4a7297263d315da1cf98ddb8fca180bdd6a0846a4986efc0b9599ef2df3bea9d9 my-hostname_547b-fb03-a428-1
20220113214003648018703 02a138eaa945ce2042e65b6e191e2047b56f0473c8fd020d00f0adbed48cd51f23c10428ee87903ca8b428f77b27f4999af0d67f42e269bb9caed6371dc28c33 my-hostname_547b-fb03-a428-1
20220113214004357105417 311aa1557dce09a1acd8d0c3d839c79c8e399c66f1cc568717b39a669801c76d3e3b2340cef8f0b34b1f98b3f64f826d1466dbebf2f8b2ca08950c82fbc5d2b2 my-hostname_547b-fb03-a428-1
20220113214004656937206 0652f2063a68bad6eb1eb99f78ef2bdd354f34ad54902da32906ac99562ef442d48f4c7914b37b6f1343161a83556bc95ed50b028103a57fd0ebb3be2e3fae62 my-hostname_547b-fb03-a428-1
20220113214005362578314 407e75f27af482ce5aaff9ea2046f2175d41767166fdd1482dd927bd3ebd02160cc20cf397490f84705ab251c0dba11a9f26c3e2776ad899f56670a9b92f3088 my-hostname_547b-fb03-a428-1
20220113214005662196702 ecc5018a63f7e876e2ccb9f0037b69d8f08bf5c34832dfbe782c40ba7ff679fc4bfb4b47d953c9a14dfd6098c74c93666a57e8c311fd2f0f6833c2b31f85ebbf my-hostname_547b-fb03-a428-1
20220113214006366314574 ffef96d947a5c3c80bbc63c589e8f4a6bb44315fb9bb4e57f6358602e6c8a7b46dd4ac9eef3f00a03242f8ea5fa437481077b2e43e6631c21964817f1a82e624 my-hostname_547b-fb03-a428-1
20220113214007374788204 b70856937937904b40f130ff85275193a643e4ec49a6c5bbdf5752f057667508be395a1cd62631dc97d78b6934db25835ce67944d58e362ab9e4df5282a9d0f4 my-hostname_547b-fb03-a428-1
20220113214007376162671 b70856937937904b40f130ff85275193a643e4ec49a6c5bbdf5752f057667508be395a1cd62631dc97d78b6934db25835ce67944d58e362ab9e4df5282a9d0f4 my-hostname_547b-fb03-a428-1

```

Manually verifying back up the chain can be done by taking the last line and removing all hashes matching that hash in that last line before that, then calculating a BLAKE2 hash of the rest of the chain. This removal may remove one or more lines that are considered transaction groups.
That hash with the last line removed should match the hash in the line before the hash you removed. If it doesn't, then we can assume tampering or that it was the first transaction group for a given set of ssh host keys. A new chain triggers the checksum and ls of the individual host keys, the concatinated data hash, and the node id full hash.

A "transaction set" in sbadger is when a group of messages are processed "at the same time", which is actually a useful quality.
This creates multiple lines in the chain with the same hash but different timestamps. The validation still works because we remove all of them
matching the group hash before calculating if that transaction group was indeed the last event to occur.

Example manual validation from the previous example block:
```
$ grep -v b70856937937904b40f130ff85275193a643e4ec49a6c5bbdf5752f057667508be395a1cd62631dc97d78b6934db25835ce67944d58e362ab9e4df5282a9d0f4 sshchain_547b-fb03-a428-1.block | b2sum
ff18a447a99ab5bfc70248c6d4ae51481d8ea078fbf98266a4db72f9d99b2fe87ad86ed176cddc7f359c936fea68bce2fcb04582ec2a30720f21e44c2fb9b189  -
```

 This program has a dependancy of Simple Event Correlator (GPL)
 see https://simple-evcorr.github.io/
 
 This version uses GNUPG2 (GPL) as well, however that could be swapped out fairly
 quickly in an edit to this script, where as SEC is being used as the event
 process that reads the data and triggers this script, as well as other
 scripts that can be configured to match patterns, thresholds, and time periods.
 
 sbadger (badger-chainz-ssh) is not intended to be a primary security control for ssh.
 Instead the intent is to supplement existing controls and data collection.
 
 This system is not inherently tamper proof by itself although makes tampering more difficult.
 
 It does not prevent tampering with the /var/log/auth.log itself, but increases the chances of tampering
 being detected. So if someone removes activity from the auth.log and perhaps remote syslog
 is not working correctly or not being reviewed, but they didn't erase the additional encrypted copies
 and sbadger chain files. So let us say they do notice SEC and remove the encrypted logs,
 then if someone is checking for those logs existing, they would notice the intrusion.
 Taking the time to reconstruct those files by refeeding in the ssh data won't
 work easily because of the time associations, and it is unlikely they have
 time to reconstruct the local chain.


 If someone attempts to remove data from the auth log after it was written,
 the data will have also been set by sbadger, so they would have to know to go
 remove the sbadger data. And if they go and remove lines from the chain file,
 they will have to remove all lines from the start of the attack onwards
 which invalidates normal ssh activity in the chain. It would be easier for
 an attack to blow away all of the sbadger data at that point, although that
 shows that they were there. So an attacker might not have time to deal with
 sbadger. Ideally ssh data is sent off to a remote logging/security server,
 regardless of sbadger or not. You might also have a program that runs
 periodically to pack up /opt/sbgr/ and send it off to a centralized
 storage location, then truncate the files. If you truncate /opt/sbgr/sshchain_$nodeid.block
 the local chain is effectively reset and the next transaction will mark a FAIL as it
 starts a new local chain.


 The $nodeid is another control feature, that is a UID generated from a BLAKE2 hash
 of all of the host_keys in /etc/ssh/ concatinated. This UID is used to mark the
 chain, so if the hostkey changes or a new one is added, a new chain is started and logged.
 On the first run of sbadger on a new chain with 0 previous lines, a FAIL mark
 file will be created with no hash and a new chain started.
 The FAIL and PASS MARK files are in /opt/sbgr/$nodeid.FAIL_MARK and /opt/sbgr/$nodeid.PASS_MARK
 These files mark the chain hashes during the chain validtion phase, the first
 function called by sbadger. The hashes in these MARK files can be used to
 look up events in the sshchain_$nodeid.block that failed chain validation.
 The data from the local chains can be concatinated/consolidated centrally.
 The chain is sortable by the first field (time) and last field which is the hostname followed by _
 then the $nodeid. If host identifying keys get
 rotated, the nodeid changes. It is still possible that many servers
 named say "localhost" with the samed cloned host keys could exist
 and cause repeating unsortable ids, but that is one of many reasons
 why you should not repeat host keys and hostnames :)
 
 
 sbadger creates a lock directory to control threads.
 With out this lock, instances of badger-chainz-ssh will
 be processed more quickly, but can cause
 verification failures in the chain, so this locking
 is desired per server. This type of private transaction
 chain can be used in distributed systems and the data
 consoludated on demand etc via other application etc.
 /var/run is a local location for tracking application state
 so we will use that local location for the lock dir.
 We can have as many nodes mounted to /opt/sbgr as
 required if that is a mounted volume, etc.
 The validate function is used to verify the integrity of the chain by checking the last entry
 and ensuring that it is a valid entry.
 The transaction function processes the SSH event data regardless of chain validity. Validation errors are FAIL MARKED then the transaction proceeds.
 This function creates the primary key of a timestamp
 that is used to pair the encrypted data files to
 the chain itself. 


## Why?

In incident response and security operations, the centralized data may be overwelming and some messages may not get fully analyzed.
With the addition of scrutinizing and cataloging SSH events locally, we can raise special attention to FAIL MARK events, easily see host key changes,
and have granular (per host) datasets that are more difficult to manipulate, that can be used by an analyst directly rather than having to query from the larger (splunk search etc).
Additionally, if situation doesn't have centralized logging systems, this tooling can be used to increase the difficulty of covering tracks locally.


While these structures are not as easy to use as primary analysis systems, they are very reliable and operate without any external network dependances.


## Notes

Currently if the SEC log is turned on we do see some exit 1 returns from sbadger spawn events. I would like to eliminate this behavior as it could confuse people that may see it as a fatal error when it can be OR could not be.
In my tests, we see exit 1 under "normal" conditions for SOME events. When in doubt, review the files created from the process that had the bad exit code.
By default the SEC log is not written, enable it via parameters in `/etc/defaults/sec`

Here is an example of reconstructing an auth.log from the encrypted lines (assuming you have the private key and the private key password):
```
for x in $(ls *.enc); do echo $x; gpg -d $x >> reconstructed_auth.log; done
```
### Warning

Servers that are low on resources and/or have heavy SSH activity could experience resource exhaustion or DoS from badger-chainz-ssh.
Servers that have a lot of valid ssh traffic, such as automated ssh systems, may not want to use badger-chainz-ssh if availability is priority.

#### Before installing:

Either replace the gpg usage in sbadger entirely or ensure a valid recipient is set for the gpg encryption. The default recipient is a key in the root keyring labelled with the email root@love. 

Additionally ensure SEC is installed. `sudo apt-get install sec` or otherwise install SEC before running the installer.
