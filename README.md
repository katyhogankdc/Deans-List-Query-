# Deans-List-Query-

Overlapping OSS - either starting on the same day or overlapping dates 

```SQL
/* 
1. overlapping OSS - either starting on the same day or overlapping dates
2. 1 incident with OSS and expulsion - based on the incident ID
*/

select 
p.penaltyname
,i.incidentid
,p.startdate
,p.enddate
,i.studentschoolid
from
custom.custom_dlincidents_raw i 
left join custom.custom_dlpenalties_raw p on p.incidentid = i.incidentid
	and p.startdate = p.startdate
	and p.enddate = p.enddate
	where penaltyname = 'OSS'
inner join(select penaltyname, startdate, incidentid, enddate, studentschoolid
	from custom.custom_dlpenalties_raw p 
	where penaltyname = 'OSS') sub on
	sub.studentschoolid = i.studentschoolid 
	and sub.incidentid != i.incidentid 
	and p.startdate = sub.startdate
	and p.enddate = sub.enddate
where p.penaltyname = 'OSS'
```

1 incident with OSS and expulsion - based on Incident ID
