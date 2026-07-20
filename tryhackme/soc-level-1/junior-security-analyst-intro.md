# Junior Security Analyst Intro - TryHackMe

## Scenario
The SIEM dashbord flagged several alerts of varying severity. The task was to identify which alert represented a genuine
threat, investigate using an IP reputation tool, escelate to the correct team member and remediate by blocking the malicious
IP on the firewall

##Alert Triage
The queue had five alerts: two Critical SSH login activity from IP 221.181.185.159, one Medium (multiple failed logins from an 
automation service account), and two Low (a failed login and a password-expiry notice). I prioritised the two Critical alerts 
because of the pattern they formed together, not just their severity label: an unauthorised login attempt from an external IP
followed four minutes later by a successful login from that same IP, Progressing from - attempt, then success is what makes 
an alert worth escalating. The Medium and Low alerts were plausible routine noise (an automated service ocassionally failing
auth, a user mistyping a password, an expired password) with no comparable pattern behind them

##Investigation
I used the IP Hunter reputation tool to check 221.181.185.159 before treating it as confirmed malicious rather than just 
suspicious. The lookup returned that the IP was associated with China Mobile Communications Corporation, had been involved
in four prior cyber attacks, and was categorised under Port Scan, C2 Server and PlugX (a known remote access trojan). That 
combination - a foreign ISP with a documented history of C2 and malware activity is what moved this from looks odd
to confirmed threat and its the evidence Id cite if challenged on the escalation decision

## Escalation
I escalated to Will Griffin the SOC Team Lead rather than Carolyn Stone (Security Architect), Nadua Watson (Python Developer)
or Gideon Gean (Sales Executive). The choice wasnt about seniority, it was about who owns triage decisions. A Security Architect, 
designs system rather than handling live incidents, a Python Developer isn't part of the response chain at all and a Sales 
Executive has no business recieving a security escalation. The SOC Team Lead is specifically responsible for directing incident
response, which makes him the correct point of escalation regardless of who else in the org chart outranks him

##Remediation
Once the escalation was confirmed, I blocked 221.181.185.159 on the firewall with the comment "Suspicious behaviou" which stopped
further access attempts from that IP. The block was reflected immediately in the firewall's block list alongside other prieviously
blocked malicious IPs tied to brute-force SSH, phishing and scanning activity

##What This Reflects About SOC L1 Work
The core lesson here wasn't the tooling, it was the discipline of verify, then escalate rather than action alone. It would have
been faster to just block the IP myself the moment it looked suspicious, but a real SOC depends on descisions being checked and
owned by the right person and not just the fastest person. That's a habit I already had from door supervision: you don't
unilaterally escalate a physical incident past your authority either, you verify what you are seeing then hand it to whoever
actually owns the response. tRanslating that instinct into a digital triage workflow is exactlyn the muscle a SOC L1 role is
built on.
