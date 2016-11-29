# Deans-List-Query-

Overlapping OSS - either starting on the same day or overlapping dates 

```SQL

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

```SQL
select 
p.penaltyname
,i.incidentid
from
custom.custom_dlincidents_raw i 
left join custom.custom_dlpenalties_raw p on p.incidentid = i.incidentid
	 where penaltyname in ('OSS', 'Expulsion')
```

Referrals more than 30 days old that have not been resolved

Missing infraction
```SQL
select 
infraction
,incidentid
,studentschoolid
from custom.custom_dlincidents_raw
where infraction IS NULL
```

Missing injury type 
```SQL
select 
injurytype
,infraction
from custom.custom_dlincidents_raw
where injurytype IS NULL 
and infraction in ('Bullying', 'Fighting', 'Sexual Misconduct or Harrassment', 'Theft', 'Threatening Physical Harm', 'Violent Incident (WITH physical injury) (VIOWINJ)')
```
