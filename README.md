# badger-chainz-ssh 🦡
Read ssh logs and create additional copies of each line as events in a chain.


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

Currently if the SEC log is turned on we do see some exit 1 returns from sbadger spawn events. I would like to eliminate this behavior as it could confuse people.
In my tests, we see exit 1 under "normal" conditions for SOME events. When in doubt, review the files created from the process that had the bad exit code.
By default the SEC log is not written, enable it via parameters in `/etc/defaults/sec`

Here is an example of reconstructing an auth.log from the encrypted lines:
```
for x in $(ls *.enc); do echo $x; gpg -d $x >> reconstructed_auth.log; done
```
### Warning

Servers that are low on resources and/or have heavy SSH activity could experience resource exhaustion or DoS from badger-chainz-ssh.
Servers that have a lot of valid ssh traffic, such as automated ssh systems, may not want to use badger-chainz-ssh if availability is priority.

#### Before installing:

Either replace the gpg usage in sbadger entirely or ensure a valid recipient is set for the gpg encryption. The default recipient is a key in the root keyring labelled with the email root@love. 

Additionally ensure SEC is installed. `sudo apt-get install sec` or otherwise install SEC before running the installer.
