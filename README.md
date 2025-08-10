## Phishing Analysis
This project is a email analysis lab designed to simulate the investigation of a suspicious phishing email. The goal is to identify indicators of compromise (IOCs) and determine whether the email is malicious. I used a sample .eml file, basic OSINT tools, and manual inspection techniques to analyse headers, links, and attachments.

### Inspecting Email Header
Firstly the email is said to be from paypal, howver the return path of the email did not resemble that of a paypal employee acount or anything to do with the company. 

<img width="600" alt="image" src="https://i.imgur.com/tvyNrEq.png">

When putting the email header to MXToolbox, we found a range of problems. Firstly, the SPF is not authenticated meaning the sender is not authorised to send on behalf of that domain and islikely a spoofed source IP.ALso, spam filters will give it a high spam score. Furthermore, the DKIM failed which indicates that the message could have been modified in transit or the sender didn't sign it properly. This makes it easier for attackers to forge content without detection. Last of all the DMARC failed and as a result the email likely was sent to spam or blocked entirely.

<img width="600" alt="image" src="https://i.imgur.com/UxQ96DT.png">


### Analysising embedded links

When hovering over the embedded website link, i saw the complete url on the side and it seemed to be a sub domain of google. To check if the url was suspicious, i used the VirusTotal website and pastd the url link. The url was flagged once out of 97 different techniques and was flagged as a phishing attempt by well known detection  engines. 

<img width="600" alt="image" src="https://i.imgur.com/mmANGZg.png">

I then re-analysed it and the same result occurred. As VirusTotal has a community section, myself and others could insert information about this urls and most comments also seem to agree that the url was malicious and was apart of a phishing campaign.

<img width="600" alt="image" src="https://i.imgur.com/npnNytZ.png">

### Conclusion
The investigation confirmed that the email in question was a malicious phishing attempt designed to harvest user credentials under the guise of a legitimate PayPal security alert. Technical analysis revealed multiple red flags, including failed SPF and DKIM authentication, lack of DMARC compliance, mismatched sender domains, and the presence of a credential-stealing HTML attachment. OSINT checks further verified that the embedded domain was newly registered and flagged by multiple security engines as malicious.

These findings highlight the importance of layered email security controls, such as SPF/DKIM/DMARC enforcement, phishing awareness training, and sandbox analysis for suspicious attachments. By identifying and documenting these indicators of compromise, this analysis provides actionable intelligence that can be used to enhance detection rules, block malicious infrastructure, and prevent future phishing attacks.

