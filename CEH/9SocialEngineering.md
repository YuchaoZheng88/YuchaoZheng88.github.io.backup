# 9. Social Engineering

## 1. Perform

### Social-Engineer Toolkit (SET)
  - open-source Python-driven tool
  - [page](https://github.com/trustedsec/social-engineer-toolkit)
  - ``` # ./setoolkit ```
  - choose 1. Social Engineering Attacks
  - then choose 2. Website Attack Vectors
  - then choose 3. Credential Harvester Attack Method.
  - then choose 2. Site Cloner
  - input your IP, your target website URL
  - try to send your IP to target, trick him to click
  - After victim login, he will be redirected to legitimate page.
  - The username and password will be catched.

### ShellPhish
  [page](https://github.com/suljot/shellphish)
  
  Various phishing techniques:
  - Spear Phishing: A targeted attack aimed at specific individuals within an organization.
  - Whaling: An attack that targets high profile executives like CEOs, CFOs, politicians, and celebrities.
  - Pharming: An attack in which web traffic is redirected to a fraudulent website by installing a malicious program.
  - Spimming: A variant of spam that exploits Instant Messaging platforms to flood spam across the network

  ``` chmod +x ./shellphish.sh ```
  ``` ./shellphish.sh ```

## 2. Detect
  
### Netcraft
  - [webpage](https://www.netcraft.com/apps/)
  - Add to Firefox
  - Show Risk Rating, Site Rank, First Seen, Host. about the website in the firefox.
  - Netcraft will block suspected phishing page in the firefox.

### PhishTank
  - [webPage](https://phishtank.org/)
  - Provide open API for developers to integrate anti-phishing data into applications.
  
## 3. Audit

### OhPhish
  - a phishing simulation tool
