# Splunk License

    index=_internal source=*license_usage.log* type=Usage 
    | timechart span=1d sum(b) as bytes
    | eval GB = round(bytes/1024/1024/1024,3)


## Splunk License usage by sourcetype

```
index=_internal source=*license_usage.log type="Usage" 
| eval indexname = if(len(idx)=0 OR isnull(idx),"(UNKNOWN)",idx)
| eval sourcetypename = st
| bin _time span=1d 
| stats sum(b) as b by _time, pool, indexname, sourcetypename
| eval GB=round(b/1024/1024/1024, 3)
| fields _time, indexname, sourcetypename, GB
``` 	 

```
index=_internal source=*license_usage.log type="Usage" earliest=-2d@d latest=@d
| fields _time, st, b, h
| bucket span=1d _time
| stats sum(b) as b by _time, st, h
| eval b=round(b/1024/1024, 4)
| rename h as host, st as sourcetype, b as MB
| eval time=strftime(_time, "%Y-%m-%d")
| eval temp = host . "@@" . sourcetype
| xyseries temp, time, MB
| rex field=temp "^(?<host>.+?)@@(?<sourcetype>.+?)$"
| fields - temp
| stats first(*) as * by host, sourcetype
```


```  
index="_internal" source="*metrics.log*" group=tcpin_connections 
| dedup hostname
| table hostname,sourceIp,fwdType,version,build,os,arch
```



11/9/2018
  



