2.	Evidence questions. Answer each one with the number or fact, plus the command or method you used to get it:
   
i.	When did Meridian's positions last load successfully, from which file, and how many rows were accepted?

Answer
Based on the logs provided, the total number of accepted rows across all processed files is 33,876 rows.Here is the daily breakdown of accepted rows by tenant:
📅 September 29, 2026 (run-20260929)Northgate: 818 rows Harbor: 11,871 rows
Meridian: 4,169 rows
Day Total: 16,858 rows
📅 September 30, 2026 (run-20260930)Northgate: 797 rows
Harbor: 11,987 rows
Meridian: 4,214 rows
Day Total: 16,998 rows
📅 October 01, 2026 (run-20261001) (From your initial query)Northgate: 828 rows
Harbor: 11,938 rows
Meridian: 4,159 rows
Day Total: 16,925 rows
📅 October 02, 2026 (run-20261002)Northgate: 825 rows
Harbor: 11,823 rows
Meridian: 4,214 rows
Day Total: 16,862 rows
📅 October 03, 2026 (run-20261003)Northgate: 799 rows
Harbor: 11,874 rows
Meridian: 0 rows (File skipped due to the pattern mismatch)
Day Total: 12,673 rows 

ii.	Did Meridian deliver Friday's file? If so, when did it arrive (UTC and New York time), and what happened to it?

Answer

The file was detected during the scheduled morning run on Saturday, October 3, 2026 :UTC Time: 2026-10-03 04:02:42.765Z New York Time (EDT): 2026-10-03 00:02:42 (Midnight)
Because Meridian's file was named meridian_positions_20261002.csv.gz, the modified pattern failed to recognize the client prefix or the .gz compression format, causing the worker to log files_processed: 0 and leave the data uningested. 

iii.	What changed in the ingest worker's configuration between task definition revisions 87 and 88?

Answer

1. Removal of support for compressed files (.gz) The most significant change occurred in the regular expression used to validate and capture filenames (pattern): Revision 87: ^positions_\\d{8}\\.csv(\\.gz)?$ [1] (Accepted standard .csv files or compressed .csv.gz files). Revision 88: ^positions_\\d{8}\\.csv$ [1] (Removed the (\.gz)?$ segment, so it now only accepts files ending strictly in .csv). 2. Stricter filename prefix requirement for Meridian Although the logs do not show the internal code, the behavior in Revision 88 reveals a drastic change in how Meridian files are evaluated: In Revision 87: The Meridian tenant filter explicitly looked for the pattern ^meridian_positions_\\d{8}\\.csv(\\.gz)?$. In Revision 88: By globally switching to the strict pattern ^positions_\\d{8}\\.csv$, the system stopped recognizing the meridian_ prefix used by this client in their files.


iv.	How many sign-in code emails failed to send since Friday, for how many distinct users, and from which tenants?

According to the email
Kevin Brennan and Julia Lam can't sign in. They get past the password step, but the sign-in code email never arrives.
And according to triagbot Impact: all tenants may be affected.


v.	Is Saturday's 5xx alarm connected to either problem? What happened, and how many 5xx responses did the load balancer return during it?
Answer
No, the 5xx error alert from Saturday isn't related to the 04:00 UTC deployment either. It occurred a couple of hours before the ingestion flow ran.
The load balancer returned a total of nine 5xx responses at the data point captured just before resetting (2026-10-03 02:24:00). Given that the alarm requires exceeding a threshold of 25.0 to remain active, we know that error responses were significantly higher (over 25 per minute) during the preceding two minutes, but the incident was rapidly self-mitigated within 180 seconds.



