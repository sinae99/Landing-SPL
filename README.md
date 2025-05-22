### "Landing (AI-Structured)"



#### Splunk Greeting (init recon of SIEM)




----------------------------------------------------------------------------------------------------------------------------------------



#### 1 ---> Index Information

"what indexes exist"

- List available indexes

      | eventcount summarize=true index=* | dedup index | table index

- Count of events per index / Top indexes by volume

      | tstats count where index=* by index

----------------------------------------------------------------------------------------------------------------------------------------

#### 2 ---> Sourcetypes Information

"which type of data sources exist"

- Find all available sourcetypes

      | metadata type=sourcetypes | table sourcetype

- Log types received from specific indexes (like firewall)

      index=firewall | stats count by sourcetype
----------------------------------------------------------------------------------------------------------------------------------------

#### 3 ---> Host, Source, and Sourcetypes

"which hosts, sources, and sourcetypes exist"

- List all hosts or sourcetypes per index with event counts

      index=* | stats count by host, sourcetype, source | sort -count
----------------------------------------------------------------------------------------------------------------------------------------

#### 4 ---> Security Events Baseline Test 

"like authentication failures, suspicious login activity"

- Find failed login attempts (count by user, src, host)

      index=* sourcetype=*authentication* (failure OR "failed login") | stats count by user, src, host

- High failed login counts (users or IPs with >10 fails)

      index=* sourcetype=*authentication* "failed login" | stats count by user, src | where count > 10
      
----------------------------------------------------------------------------------------------------------------------------------------

#### 5 ---> Field and Data Structure Information

"fields available"

- See all fields available in an index (fieldsummary)

      index=my_index | fieldsummary

- Compare fieldsummary with raw field extraction

      index=my_index | head 100 | fieldsummary
----------------------------------------------------------------------------------------------------------------------------------------

#### 6 ---> Data Models

"Common Data Models"

Common data models list (Network_Traffic, Authentication, Intrusion_Detection, Endpoint, Change, Web)


    | tstats count from datamodel=Authentication by Authentication.user

or
    
    | datamodel Network_Traffic All_Traffic search | stats count by nodename
----------------------------------------------------------------------------------------------------------------------------------------

#### 7 ---> Notable Events

"Notable events, and correlation searches"

- View correlation searches
      Enterprise Security > Configure > Content Management

      | tstats from notable index by rule_name, urgency, status, mitre_technique

      index=notable | stats count by rule_name, urgency, status
      | stats count by mitre_technique
----------------------------------------------------------------------------------------------------------------------------------------

#### 8 ---> Saved Searches and Reports

"Management of saved searches and reports"

  Settings > Searches, reports, and alerts
