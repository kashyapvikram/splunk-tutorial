# splunk-tutorial


SHA256 checksum (splunk-dashboard-examples_701.tgz) 3a49e71b27a113aad45a909907dbe9f5e2732a68632cc236c2e1bd019332670e


To install apps and add-ons from within Splunk Enterprise

    Log into Splunk Enterprise.
    On the Apps menu, click Manage Apps.
    Click Install app from file.
    In the Upload app window, click Choose File.
    Locate the .tar.gz file you just downloaded, and then click Open or Choose.
    Click Upload.
    Click Restart Splunk, and then confirm that you want to restart.

To install apps and add-ons directly into Splunk Enterprise

    Put the downloaded file in the $SPLUNK_HOME/etc/apps directory.
    Untar and ungzip your app or add-on, using a tool like tar -xvf (on *nix) or WinZip (on Windows).
    Restart Splunk.




Example  queries : 

rex field=http_user_agent "Chrome/(?<Chrome_version>.+?)?Safari" | top Chrome_version
rex field=http_user_agent "Chrome/(?<.*>.+?)?Safari" | top $1


DAY3 

=====

1) wrote a  simple  py script to   test  adding  data  via  script : 

abc@se:/opt/splunk/bin/scripts$ cat input.py 
#!/usr/bin/env python
import time 
now = time.strftime("[%d-%m-%Y %H:%M:%S]")
print now + '  hello world from python'
abc@se:/opt/splunk/bin/scripts$ 

OpsDataGen.spl


2)  downloaded  OpsDataGen.spl
 and  installed it 

also  wrote  query to  extratc data 

index=main sourcetype="access_combined" | eval browser=useragent | replace *Firefox* with Firfox, *Chrome* with Chrome, *MSIE* with IE , *version*Safari* with Safari, *Opera* with Opera in browser | top limit=5 browser


sourcetype="access_combined" |timechart span=6h avg(response) as AVG_RSP |eval AVG_RSP = round(AVG_RSP/1000,2) | table _time, AVG_RSP


Monitor  if  people  are watching   but not  buying a  product : 

index=main sourcetype="access_combined"  uri_path="/viewItem" OR uri_path="/addItem" status=200 | dedup JSESSIONID  uri_path item | chart count(eval(uri_path="/viewItem")) AS view, count(eval(uri_path="/addItem")) AS add by item | sort - view


index=main sourcetype="log4j" perfType="MEMORY"




 
**Get remaining  memory : 

index=main sourcetype="log4j" perfType="MEMORY"|  eval mem_used_pc=round((mem_used/mem_total)*100) | eval mem_remained_pc=(100-mem_used_pc) | timechart span=4h avg(mem_used_pc) as MEM_USED avg
(mem_remained_pc) as MEM_REMAINED | where MEM_REMAINED != " "


**Get  number  of DB connections   which has  crossed  the threshold  limit  of  allowed  connections : 

index=main sourcetype="log4j" perfType="DB"|  eval threshold=con_total/100*50|where con_used > threshold| timechart span=2h count(con_used) AS CountOverThreshold








