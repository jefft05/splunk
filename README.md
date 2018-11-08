# Splunk License

    index=_internal source=*license_usage.log* type=Usage 
    | timechart span=1d sum(b) as bytes
    | eval GB = round(bytes/1024/1024/1024,3)


## Splunk License usage by sourcetype
	index=_internal source=*license_usage.log type="Usage" 
 	 | eval indexname = if(len(idx)=0 OR isnull(idx),"(UNKNOWN)",idx)
 	 | eval sourcetypename = st
 	 | bin _time span=1d 
  	 | stats sum(b) as b by _time, pool, indexname, sourcetypename
  	 | eval GB=round(b/1024/1024/1024, 3)
 	 | fields _time, indexname, sourcetypename, GB
 	 
 	 
 	 
 #11/08/2018