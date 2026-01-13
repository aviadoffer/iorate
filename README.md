# iorate
iorate I/O performance testing tool (originally by EMC)

iorate was created in 1997 by Vince Westin of EMC. EMC had a tool, but it was for internal use at EMC. So to fill this need, iorate was created.

Iorate is freely available for public use now. Though this was developed by EMC staff with EMC resources, the code is available for anyone to use. There are no EMC-specific pieces to the testing, it just tests storage. It has been used by EMC customers around the world. 
Use the downloads pages to get a copy to try for yourself. Be sure to read the User Guide.

This is an attempt to keep it alive since it's a very useful and intutive tool

		IORATE Version 3.17 Notes
			
Fixed Few dispaly issues after termination (including CTRL-C)

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



