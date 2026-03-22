# log4j SOLVING LAB OF TRY HACK ME

 # Reconnaissance
 1.What service is running on port 8983? (Just the name of the software)
 Answer:Aache solr
 
 *-p- scan all ports*
 
 *-sV -service version*
 <img width="1600" height="495" alt="image" src="https://github.com/user-attachments/assets/05b46937-2040-46e9-971b-abf5c96ed9cb" />
 # Dscover
 1.Take a close look at the first page visible when navigating to http://10.65.142.0:8983 (opens in new tab). You should be able to see clear indicators that log4j is in use within the application for logging activity. What is the -Dsolr.log.dir argument set to, displayed on the front page?

<img width="627" height="317" alt="image" src="https://github.com/user-attachments/assets/90340c9c-27a8-48b2-a020-a47b82524ff4" />

Answer: /var/solr/logs

2. One file has a significant number of INFO entries showing repeated requests to one specific URL endpoint. Which file includes contains this repeated entry?

   YOU CAN FIND MORE REQUESTS ARE IN GIVEN FILES (MENTION THE FILE NAME)

Answer: solr.log

3.  What "path" or URL endpoint is indicated in these repeated entries?
  
<img width="1140" height="112" alt="image" src="https://github.com/user-attachments/assets/51c6b290-96f0-45a7-a048-de7c098978ff" />

IN SAME FILE SOLR.LOG HAS THE PATH

Answer:/admin/cores

4) Viewing these log entries, what field name indicates some data entrypoint that you as a user could control?

FINDING SOLR.LOG WE SEE MANY REQUESTS BUT NOT PARAMETER ARE NOT PASSED SO USER CAN DONE BUT ITS EMPTY params{}.

Answer:params
# Proof Of Concept

