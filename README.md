# badger-chainz-ssh
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
 they if someone is checking for those logs exiting, they would notice the intrusion.
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
 so we will use that local location for the lock file.
 We can have as many nodes mounted to /opt/sbgr as
 required if that is a mounted volume, etc.
 The validate function is used to verify the integrity of the chain by checking the last entry
 and ensuring that it is a valid entry.
 The transaction function processes the SSH event data.
 This function creates the primary key of a timestamp
 that is used to pair the encrypted data files to
 the chain itself. This data is typically confirmed
 via the regular rsyslog data or other ssh log collection
 systems, ideally with remote storage duplication.
 The main function builds the sbadger workspace if not there.
 And sets permissions to 600, then validates the chain with the validate function,
 then processes the transaction with the transaction function.

