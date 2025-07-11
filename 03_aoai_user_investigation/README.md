# AOAI Anomalous User Investigation

### Why use AOAI?

Azure OpenAI (AOAI) is leveraged in this project to simulate the reasoning of a cybersecurity analyst. 
By using AOAI, we can analyze complex user behaviors and generate detailed reports that provide insights into potential insider threats. 
AOAI combines user background information, engineered features, and raw event logs to produce structured analysis summaries, including risk levels, 
anomalous activities, and recommendations. This approach enhances the detection and investigation process by providing a comprehensive understanding 
of user behavior and potential risks.

**Additional Benefits for Cybersecurity Specialists:**

* **Expedited Workflows:** AOAI can significantly speed up the analysis process by quickly processing large volumes of data and generating insights, allowing cybersecurity specialists to focus on critical tasks and decision-making.
* **Second Review:** AOAI provides an additional layer of analysis, acting as a second review to validate findings and ensure no potential threats are overlooked.
* **Scalability:** AOAI can handle large datasets and complex scenarios, making it suitable for organizations with extensive and diverse data sources.
* **Enhanced Detection:** By leveraging advanced AI capabilities, AOAI can detect subtle and complex patterns that might be missed by traditional methods, improving the overall effectiveness of threat detection.

---

### Investigate Anomalies ([aoai_investigate_anomalies.ipynb](aoai_investigate_anomalies.ipynb))
This notebook leverages Azure OpenAI (AOAI) to simulate the reasoning of a cybersecurity analyst.
Once a users has been nominated as anomalous from the isolation forest model, this notebook can be used to further explain the users behavior.
AOAI analyzes the users most recent logs, background employee information, and engineered features to create an analysis report of the user's anomalous activity, anomalous activity timelines, and recommendations for next steps.

**AOAI prompt:**
- Combines user background, engineered features, and raw event logs.
- Encourages temporal and semantic reasoning to detect anomalies.
- Produces a structured analysis summary including risk level, anomalous activities, and recommendations.

**Example AOAI output:**
```
## Insider Threat Analysis Summary (Example Output from AOAI)

**User Summary**  
User: Anonymous Employee (XXXXX-ID) — Senior IT Administrator in the Electronic Security team, with broad access and technical privileges.

**Behavior Summary**  
In the recent period, the user exhibited a moderate decrease in overall activity compared to baseline, but with a significant proportion of after-hours logons, increased interaction with external parties, and evidence of risky behaviors. Notably, there is a spike in job search-related web activity, external communications, and the presence of a suspicious executable file associated with keylogging/malware.

**Anomalous Activities**  
1. Execution of Undetectable Keylogger/Surveillance Malware: On 2010-12-09, the user executed [REDACTED_FILENAME].exe on [REDACTED_DEVICE_ID], a file described as "undetectable username malware" with keylogging and covert surveillance capabilities.  
2. High Volume of Job Search and External Communications: There is a marked increase in visits to job search and recruitment websites (e.g., CareerBuilder, LinkedIn, Indeed, Monster, SimplyHired, Craigslist, Yahoo HotJobs) and a spike in emails sent to external addresses, including personal and non-corporate domains.  
3. Elevated After-Hours Activity and Use of Multiple Devices: 54% of recent logons occurred after hours (26 out of 48), and the user accessed four different devices recently, with device connect/disconnect events clustered in short intervals.

**Anomalous Timeline of Events**  
- 2010-12-06 to 2010-12-10 — Surge in job search web activity (multiple job boards, LinkedIn, etc.), repeated access to file-sharing and personal email services, and increased external email traffic.  
- 2010-12-09 — Execution of a keylogger/surveillance tool on the user's primary workstation ([REDACTED_DEVICE_ID]), followed by continued after-hours activity and further external communications.  
- 2010-12-09 to 2010-12-10 — Continued high frequency of after-hours logons, persistent access to job search and file-sharing sites, and ongoing external email correspondence, including to personal and non-corporate addresses.

**Risk Assessment**  
- Risk Level: High  
- Justification: The combination of malware/keylogger execution, increased external job search and communication, high after-hours access, and deviation from baseline in both device usage and external interactions strongly indicate potential insider threat activity, possibly involving data exfiltration or credential harvesting.

**Recommendations**  
- Immediately escalate to security incident response for forensic investigation of the affected workstation(s).  
- Temporarily suspend or restrict the user's privileged access pending investigation.  
- Review outbound data transfers and email attachments for possible exfiltration.  
- Conduct an interview with the user to assess intent and clarify anomalous behaviors.  
- Increase monitoring of related accounts and endpoints for lateral movement or additional compromise.
```

**Before running script:**
- update "api_key" with your AOAI API Key
- update "user" variable with the username of the user you would like to analyze
- update "max_logs" variable with the max number of logs your deployment of AOAI can use (experiment with different values)
