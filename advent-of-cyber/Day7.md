# Day 7: Oh, no. I'M SPEAKING IN CLOUDTRAIL!


Here's what I understand from this:-

1. CloudWatch: Monitors AWS environment on different levels, some CloudWatch Logs terms are: Log Events, Log Streams and Log Groups
   
   CloudTrail: Monitors actions and stores log history in JSON format. 


2. JQ is a command line processor to parse and filter JSON data. Usually an event has userIdentity, eventTime, eventType, eventSource, eventName, sourceIPAddress, userAgent and requestParameters.


3. We can use JQ to filter for certain s3 objects. Logs will be in `~/wareville_logs` directory


I ran this command, which I got from the guide and on top of which I slapped `grep`:-


```

/wareville_logs$ jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"], (.Records[] | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t' | grep glitch

```

The output of this helped me do the 5 initial questions at once! Then I ran this with the help of the guide:-


```

~/wareville_logs$ jq '.Records[] | select(.eventSource=="iam.amazonaws.com" and .eventName== "AttachUserPolicy")' cloudtrail_log.json
```

After looking at the hint, and the policyArn section I saw the anamalous user gained AdministratorAccess, after this I ran another JQ request similar to the first one.


```
/wareville_logs$ jq -r '["Event_Time", "Event_Source", "Event_Name",
"User_Name", "Source_IP"], (.Records[] | [.eventTime, .eventSource,
.eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) |
@tsv' cloudtrail_log.json | column -t -s $'\t' | grep mayor

```

Which got me MM's IP Address. I used the same command but this time `grep`ing for 'mcskidy' and I got the IP addresses under her user.

Then I ran, `grep INSERT rds.log` and also found MM's bank account number.


Looks like the Mayor is embezzling money lol.
