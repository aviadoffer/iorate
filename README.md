# iorate
iorate I/O performance testing tool (originally by EMC)

iorate was created in 1997 by Vince Westin of EMC. EMC had a tool, but it was for internal use at EMC. So to fill this need, iorate was created.

Iorate is freely available for public use now. Though this was developed by EMC staff with EMC resources, the code is available for anyone to use. There are no EMC-specific pieces to the testing, it just tests storage. It has been used by EMC customers around the world. 
Use the downloads pages to get a copy to try for yourself. Be sure to read the User Guide.

This is an attempt to keep it alive since it's a very useful and intutive tool

## Quick Start Guide

This quick start example demonstrates how to configure and run a simple mixed random workload: **8K block size, 70% read, and 30% write**. 

To get started, you need to create three simple configuration files in your working directory:

### 1. `devices.ior`
Define your target storage device and thread count (e.g., one device with one thread).
```text
Device = "/dev/mapper/mpathXXX" count 1 capacity 100GB offset 64KB;
```

### 2. `patterns.ior`
Define the I/O sizes and access patterns.
```text
Pattern 1 = "8K Random Read" io size 8KB random read;
Pattern 2 = "8K Random Write" io size 8KB random write;
```

### 3. `tests.ior`
Define the test duration and the ratio of the patterns you defined above.
```text
Test 1 = "Example 70/30 8K random" for 5 min 70% pat 1, 30% pat 2;
```

### 4. Run
Once your configuration files are ready, simply run the executable:
```bash
./iorate
```

## Release notes
			IORATE Version 3.23 Notes

Added metadata for NAS benchmarking support.  IORATE can now measure
    metadata operation rates against directory-based devices (e.g. NFS
    mounts, local filesystems) in addition to traditional block I/O.

New pattern keywords for metadata operations in pattern.ior :

	Pattern 1 = "NFS meta stat" stat ;
	Pattern 2 = "NFS meta open_close" openclose ;
	Pattern 3 = "NFS meta readdir" readdir ;
	Pattern 4 = "NFS meta mix (33% of each of the above)" meta ;

Directory devices:  When a device name points to a directory path,
    iorate automatically scans the tree (up to depth 100) and builds
    internal file/directory lists for metadata operations.  Example:

    Device = "/mnt/aviadsmb" count 1 ;

Mixed meta + I/O tests:  A single test can combine metadata patterns
    with traditional I/O patterns.  When running against a directory
    device, I/O patterns perform file-based reads/writes against random
    files found during the scan. careful with writes!!

Real-time dashboard:  The dashboard displays metadata OPS and average
    latency per operation type (stat, openclose, readdir).  Mixed tests
    show both metadata OPS and traditional Read/Write IOPS.

CSV output:  Metadata statistics are included in the .csv output file
    with columns for each operation type's OPS and latency.


			IORATE Version 3.22 Notes

Added --dedup-ratio=<x>  Generates dedupable write data at x:1 dedup ratio.
    The write buffer is divided into dedup-block-size chunks.  For a ratio
    of X:1, every X consecutive chunks on disk share the same byte content,
    so the storage array's dedup engine will detect real, byte-identical
    duplicates across different LBA ranges.

Added --dedup-block-size=<nK>  Sets the dedup block granularity in KB
    (default 4).  Must be a power of 2.  Different storage arrays use
    different dedup granularity; set this to match your target array:

Dedup and compression are fully composable.  When both are enabled the
unique blocks are themselves filled with compressible data, simulating
arrays that perform both inline dedup and compression.


            IORATE Version 3.21 Notes

Deprecated --stay-up-after-all-runs; it is now the default when using --report-host-name (passing it is ignored).

Fixed: passing --srr without a report server name no longer crashes.

Added: when --threads is passed, the dashboard 

"You can press +/- keys to increase/decrease number of running threads"
	
The change is applied dynamically and propagated to all remote hosts.
	   
			IORATE Version 3.20 Notes

Added --compressible_ratio=\<x\>. When set, iorate will try to generate
		
write data that is approximately x:1 compressible. The generator works
		
per 4KB chunk: the first bytes are random and the remainder of
		
the chunk is filled with the the same byte. For example, --compressible_ratio=2
		
aims for each 4KB to compress to about 2KB.
		
This is approximate and depends on the compression algorithm, compression
		
block size, and target storage behavior. If the parameter is not provided,
		
iorate uses its traditional fully random write data (with a small historical
		
header for validation).
		

			IORATE Version 3.19 Notes

Fixed Support for AIX, now supporting AIX against rhdisk 

Fixed All compliation warnings are now cleared

			IORATE Version 3.18 Notes
			
Added --list-connected-clients - Listen for client connections to verify connectivity. The clients can run --report-host-name=\<host\> or --srr=\<host\>

Added --realistic-io  - Enable realistic I/O with fluctuating IOPS		

Added --realistic-io-fluctuation=<%n> -  Fluctuation percentage for realistic I/O (default 15%)

		IORATE Version 3.17 Notes
			
Fixed Few display issues after termination (including CTRL-C)

Fixed Dashboard display issue with very large IOPS (>1M)

Fixed a very very old issue where iorate will fail if there is no empty line at the end of .ior file/s

		IORATE Version 3.16 Notes
			
Added color to the Dashboard

Added Reporting server will now summarize the CSV from all the hosts and save it under all-hosts.csv (make sure all clocks/time zones are synced)

Added --no-color to disable color on the Dashboard

Added New logic, if you CTRL-C from the reporting server it will signal the other hosts to stop the run (waiting for the next run if configured)

Added -srr=<host> which is combination of : --stay-up-after-all-runs + --retrieve-test-files + --report-host-name=\<host\>

Fixed --install-completion is no longer an option on Windows (no bash support)

Fixed --help - not displaying help
			
		IORATE Version 3.15 Notes

Added --install-completion, after running it restart bash and now it will \<tab\> complete the -- options 

		IORATE Version 3.14 Notes

Fixed a crash during multiple runs with --retrieve-test-files --stay-up-after-all-runs options

		IORATE Version 3.13 Notes

Reporting server (--listen-as-report-host) now automatically collects CSV

reports from all clients upon run completion, saving them as

\<hostname\>.iorate.csv.


Added support for per-host device configurations. The reporting server can

now serve a specific \<hostname\>.devices.ior file to a matching client.

Clients running with --retrieve-test-files will prefer this specific

remote device configuration if received; otherwise, they default to the

local devices.ior.

Fixed various compilation warnings and linking errors.



			IORATE Version 3.12 Notes
You can now run iorate in listen mode, ex:

on server1:

./iorate --listen-as-report-host

on server2,3,4,5 ....

./iorate --report-host-name=server1 --retrieve-test-files --stay-up-after-all-runs

server1 will send tests.ior and patterns.ior to server2,3,4,5 ... before running the tests 

once the runs are complete server2,3,4,5 will stay running and wait for server1 to run again 

Notice that devices.ior is unique therefore has to be local on each server

Added  --retrieve-test-files     Fetch tests.ior and patterns.ior from the report host

Added --stay-up-after-all-runs   Loop indefinitely, refreshing config from report host

			IORATE Version 3.11 Notes
			
Combined Linux and Windows (Cygwin) code - makefile works on both now 

I/O Dashboard displays improvements - number of threads runnning and CPU util

Added --report-host-name=<host> - send live I/O stats to a central reporting host

Added --listen-as-report-host - act as aggregator; display combined hosts stats

			IORATE Version 3.10 Notes
	
Direct I/O is now the default (-u). to run buffered I/O run with --no-direct-io

Added --no-direct-io - disable direct I/O (use buffered I/O)

Added a new Dashboard I/O during the run and summary at the end 

Added --disable-io-dashboard - do not print the stdout I/O Dashboard section (CSV output still written)

Added a new iorate.csv output aggregating all devices performance 


			IORATE Version 3.08 Notes
	
Improved read/write speed by using one less system call using pread/pwrite


			IORATE Version 3.07 Notes

Increased max volume size from 4TB to 96TB

Allow "reuse" to be 100% - prior above 99% was not allowed 

Allow "reuse" to be 1% - prior below 10% was not allowed, if 0% is needed use "random" without reuse 

Added --threads=<n> - override device copy count (threads) for all devices

Added --scale_threads_by=<n> - add <n> threads to all devices for each additional run

Added  --scale_threads_count=<n> - number of additional runs to perform with scaled threads


			IORATE Version 3.06 Notes

Changed writes from sync to async



