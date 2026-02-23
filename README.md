# SOAR-EDR-PROJECT

## Objective

The objective of this project was to engineer an automated response pipeline that reduced MTTR by over 90%, slashing response times from minutes to under 15 seconds. By orchestrating LimaCharlie EDR and Tines SOAR, I automated the detection of credential-theft tools using custom YARA rules, achieving a 100% detection rate in simulations. The project culminated in a "Human-in-the-Loop" workflow that enables one-click host isolation via Slack, instantly neutralizing threats and preventing lateral movement.

### Skills Learned

- Reduce Mean Time to Respond (MTTR) (Transitioning from manual incident triage to an automated pipeline that reduces the response window from minutes to seconds) less than 15 seconds.
- Implement Detection Engineering: Creating custom Detection & Response (D&R) rules using YARA and LimaCharlie's telemetry to identify specific adversary behaviors (Credential Theft).
- Orchestrate Security Workflows: Building a seamless "Story" in Tines that handles the logic of alert ingestion, multi-channel notification, and response actions.
- Simulate Adversary Tactics: Testing the pipeline against real-world attack simulations (LaZagne) to validate the end-to-end effectiveness of the defense.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- LimaCharlie (EDR & SecOps Cloud)
- Tines (SOAR - Security Orchestration, Automation, and Response)
- Slack & Email (Communication Channels)
- YARA (Pattern Matching): Serve as detection logic

## Steps

Step 1: I started with draw.io tool to provide a diagram that will showcase the workflow

<img width="959" height="510" alt="Screenshot 2026-02-17 121958" src="https://github.com/user-attachments/assets/1f05b413-fb9d-428c-8361-5d2f97028e03" />


Step 2: I also wrote down a rough draft on my notepad to serve as reference (I do this for all my labs or project or whenever I am learning)

<img width="960" height="491" alt="Screenshot 2026-02-17 114643" src="https://github.com/user-attachments/assets/ca510231-dbb7-4e28-9a7e-f1b42afeb682" />

Step 3: (Install and Configure/setup LimaCharlie) I used the Vultr Cloud, operating system to be Windows (2022 x64). To deploy the server, I selected my location to be singapore, the paln to be 1vCPU, 55GB SSD,2GB memory, 2TB bandwith and my host name (Myproject-SOAR)

<img width="960" height="510" alt="Screenshot 2026-02-17 123211" src="https://github.com/user-attachments/assets/678922f0-0f69-449b-95c8-cb9fa177dca9" />

Step 4: I then created an Organization in LimaCharlie (MyprojectSOAR) and then went back to vultr cloud to setup/update firewall to the server

Step 5: From LimaCharlie, I created an Installation Key (MyProject-SOAR-EDR) Installation key is what is going to be used to enroll the machine to LimaCharlie, Sensors list on the other hand will then contain the machine once it has been successfully enrolled.

<img width="960" height="510" alt="Screenshot 2026-02-17 125512" src="https://github.com/user-attachments/assets/2948aaa5-fd2a-4e12-b5ff-8c95fd608e26" />

Step 6: From the sensors downloads, I copied the EDR Windows 64bit link and paste it in the cloud server(microsoft edge browser) which I created and logged into via (view console) and download. 

<img width="960" height="509" alt="Screenshot 2026-02-17 132422" src="https://github.com/user-attachments/assets/ebd048c3-d029-4e0e-961b-8280684179b8" />

<img width="389" height="449" alt="Screenshot 2026-02-17 132640" src="https://github.com/user-attachments/assets/a474ce25-090c-4c51-be9d-a792dbcbfbbf" />

Step 7: From the Insallation Keys I created, I copied the sensor key

<img width="742" height="236" alt="Screenshot 2026-02-17 132717" src="https://github.com/user-attachments/assets/7272a630-0884-4e70-81ba-5847a5ed4f6f" />

Step 8: Back to the server, I opened powershell, run as administrator and the navigate to "Downloads" directory where I have LimaCharlie downloaded (cd Downloads) & (dir). After that, I installed the agent successfully by entering the command (.\hcp_win_x64_release_4.33.26.exe -i (paste the sensor key)).

<img width="762" height="499" alt="Screenshot 2026-02-17 133230" src="https://github.com/user-attachments/assets/e0bce945-89c1-4f33-aeca-d8f568ba0241" />

Step 9: The agent was successfully installed.

<img width="761" height="502" alt="Screenshot 2026-02-17 133259" src="https://github.com/user-attachments/assets/8669eba0-ff53-4abf-afc1-68005e9c078b" />

Step 10: I head back to my LimaCharlie (Live Feed section) and confirmed that it is already generating events.

<img width="960" height="512" alt="Screenshot 2026-02-17 134106" src="https://github.com/user-attachments/assets/01e6ba9c-9a0d-4888-894b-f6a502778c4a" />

Step 11: Generate Telemetry (LaZagne) whcih will be the password recovery tool I used. I turned off real time protection from windows security settings, and downloaded LaZagne.exe file

<img width="760" height="503" alt="Screenshot 2026-02-17 190409" src="https://github.com/user-attachments/assets/0d49ce34-a58a-46ad-a11f-df543a7da999" />

Step 12: I then opened the LaZagne file on powershell and confirmed if it has been correctly downloaded. Which I confirmed to be successful

<img width="764" height="502" alt="Screenshot 2026-02-17 190549" src="https://github.com/user-attachments/assets/d501790a-3df4-4e16-ba86-439d0f2cfdb3" />

Step 13: My assumption after confriming LaZagne file has been installed and running is that my LimaCharlie should have also picked up the process. I confirmed that it has indeed picked up the process and already generating events (check Timeline tab).

<img width="960" height="509" alt="Screenshot 2026-02-17 190850" src="https://github.com/user-attachments/assets/b28ec2d1-3dc7-41a2-a832-ac64ceab121a" />

Step 14: Looking at the events, I am more interested in the "NEW_PROCESS" event, so I clicked on it so I can see and get more information on the events (information such as; Base_Address, FILE_PATH, COMMAND_LINE, MEMORY USAGE, PARENT, LINK, HASH and more). These are all very important information for generationg or creating our detection rules

<img width="748" height="412" alt="Screenshot 2026-02-17 191343" src="https://github.com/user-attachments/assets/975b3d43-a62f-4a97-a095-2e02bf57043e" />

Step 15: Create new rule by navigating to the created organizations to Automation tab to D&R rules.

<img width="959" height="476" alt="Screenshot 2026-02-23 230029" src="https://github.com/user-attachments/assets/c01005dc-6975-4339-b4b4-7e8f30b4154d" />

<img width="960" height="473" alt="Screenshot 2026-02-23 230057" src="https://github.com/user-attachments/assets/9af8a0c7-10d5-4ac9-906c-080e80e98df2" />

Step 16: After creating the D&R rules, I then headed back to my server's powershell and entered the command (.\LaZagne.exe all)

<img width="757" height="496" alt="Screenshot 2026-02-18 181315" src="https://github.com/user-attachments/assets/7d03cf29-0e1e-43f1-80be-500df3c7a9b0" />

Step 17: After that, I then headed to the detection section to check for detections and I found some detections generted already.

<img width="958" height="511" alt="Screenshot 2026-02-20 162315" src="https://github.com/user-attachments/assets/2b2867f9-877d-4423-bdc0-9dbb4dd70777" />


Step 18: Create Playbook. From my Tines (SOAR tool), I created a new Credential (slack).

<img width="958" height="511" alt="Screenshot 2026-02-18 211534" src="https://github.com/user-attachments/assets/eee757e4-27dd-41ed-9ff0-4a5332c9dd81" />

Step 19: Head to story, templates and then drag and drop webhook and Slack and then connect them.

<img width="959" height="507" alt="Screenshot 2026-02-20 163948" src="https://github.com/user-attachments/assets/c1823171-794e-4df0-aa3f-75d63a17dfe4" />

Step 20: Over to my slack account, I navigated to my alert channel and copied the channel ID and paste it in my tines story to enable connection and commuication between tines and slack.

<img width="957" height="510" alt="Screenshot 2026-02-18 212221" src="https://github.com/user-attachments/assets/b1a40933-7f4d-4896-b86a-5f164c81f715" />

Step 21: After connecting the webhook to slack on tines, I headed back to my alert channel on slack and confirmed that slack is already recieving alerts from tines (the message says: "Hello, you are awesome!"). This was just for trial to make sure the connection is good.

<img width="960" height="509" alt="Screenshot 2026-02-20 164057" src="https://github.com/user-attachments/assets/1f25a6b5-c3fc-4d5e-9b63-972089eded56" />

Step 22: I created a send email tool which I selected from the templates, and connected it to the webhook. I then added the recipient email address (audua806@gmail.com) and sender's name as Aminullahi Tanimu.

<img width="960" height="513" alt="Screenshot 2026-02-20 165403" src="https://github.com/user-attachments/assets/ffa502cf-71cc-4b74-a47f-00399c2cbe63" />

Step 23: I tested this and confirmed the email recipient recieved the message from tines.

<img width="960" height="502" alt="Screenshot 2026-02-20 165453" src="https://github.com/user-attachments/assets/8f12faf6-fe10-4c1c-876a-6cb57a14197f" />

Step 24: To enable the user prompt, I used a tool known as PAGE.

Step 25: I then replaced/changed the message content on my slack tool on tines with all the necessary information of the event (information such as title, source IP, command line, file path etc)

<img width="960" height="193" alt="Screenshot 2026-02-23 235014" src="https://github.com/user-attachments/assets/3c72c263-8703-490c-a199-c0d086344fab" />

Step 26: I then tested it out and then confirmed slack and email has recieved the alert from tines with all the informations and the required link.

<img width="960" height="508" alt="Screenshot 2026-02-20 173036" src="https://github.com/user-attachments/assets/26d23535-8067-4342-88c7-58aa5a967b95" />

<img width="958" height="511" alt="Screenshot 2026-02-20 174035" src="https://github.com/user-attachments/assets/d144c69e-4b1f-40f9-b6f1-4e138cb68b7c" />

Step 27: From the PAGE tool for enabling the User Prompt, I added as success message "Thank you, You can now close this window". 

<img width="960" height="510" alt="Screenshot 2026-02-20 174649" src="https://github.com/user-attachments/assets/88e4e668-9378-4846-8402-e9b3a741bcd0" />

Step 28: Tried out the No button on User Prompt to send a message to Slack that "the computer was not isolated, please investigate". I used a tool called Trigger and connected it to slack so that Slack can receieve the message.

<img width="960" height="508" alt="Screenshot 2026-02-20 175017" src="https://github.com/user-attachments/assets/09176736-4a8d-4887-bcba-40284cdec324" />

<img width="959" height="504" alt="Screenshot 2026-02-20 180003" src="https://github.com/user-attachments/assets/bc609fcd-f9dd-4e2f-ba97-fa366d896c9f" />

Step 29: For the Yes option, I used another trigger tool then created another LimaCharlie template and connected it to the trigger tool. 

<img width="754" height="426" alt="Screenshot 2026-02-20 212528" src="https://github.com/user-attachments/assets/70c25e1a-f6a0-425a-bc88-2771c9af1075" />

Step 30: I then tried the yes option and got to see that the computer has been isolated and confirmed it in LimaCharlie.

<img width="960" height="507" alt="Screenshot 2026-02-20 212833" src="https://github.com/user-attachments/assets/eae2f347-b273-428b-8b75-f046b5eeac35" />

Step 31: After Isolating the machine, my alert channel on slack also recieved the isolation status. I did that by creating another LimaCharlie template (get isolation status) and connecting it to the Isolate Sensor LimaCharlie template. The Isolate sensor LimaCharlie template is then also connected to another slack template which will recieve the isolation status message. 

<img width="960" height="508" alt="Screenshot 2026-02-20 215259" src="https://github.com/user-attachments/assets/ab21fdd6-5219-43de-9464-8c736359d4b5" />

Step 32: I then re-emit the event in User prompt and confirmed that slack has indeed recieved the message that the computer has been Isolated

<img width="960" height="507" alt="Screenshot 2026-02-20 214954" src="https://github.com/user-attachments/assets/26d4b922-38a1-48f7-a5e5-457508d19e68" />


### MITRE ATTACK MAPING

### T1003 (OS Credential Dumping)
Adversaries dump credentials to gain administrative access.

The Lab Action: The YARA rules targeted the signature of credential-theft binaries.

Automation Value: Instead of waiting for a human to see the alert, the SOAR platform instantly ingested the telemetry, ensuring no "dwell time" for the attacker to move to the next phase.

### Response: T1570 (Lateral Movement Prevention)
Once an attacker has credentials, they attempt to hop to other servers.

The Lab Action: By triggering Host Isolation via Tines/LimaCharlie, I severed all network connections except to the EDR console.

Result: This effectively "quarantined" the threat, neutralizing the risk of a full-network breach within seconds.
