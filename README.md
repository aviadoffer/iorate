# iorate
iorate I/O performance testing tool (originally by EMC)

iorate was created in 1997 by Vince Westin of EMC. EMC had a tool, but it was for internal use at EMC. So to fill this need, iorate was created.

Iorate is freely available for public use now. Though this was developed by EMC staff with EMC resources, the code is available for anyone to use. There are no EMC-specific pieces to the testing, it just tests storage. It has been used by EMC customers around the world. 
Use the downloads pages to get a copy to try for yourself. Be sure to read the User Guide.

This is an attempt to keep it alive since it's a very useful and intutive tool

Most recent update is for the Linux version only:

			IORATE Version 3.07 Notes
Increased max volume size from 4TB to 96TB
Allow "reuse" to be 100% - prior above 99% wast not allowed 
Allow "reuse" to be 1% - prior below 10% was not allowed, if 0% is needed use "random" without reuse 
Added --threads=<n> - override device copy count (threads) for all devices
Added --scale_threads_by=<n> - add <n> threads to all devices for each additional run
Added  --scale_threads_count=<n> - number of additional runs to perform with scaled threads



