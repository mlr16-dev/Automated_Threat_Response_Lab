<h1>SOAR-EDR-Project</h1>

<h2>Description</h2>
Project consists of a security automation pipeline to detect HackTool activity and send detailed alerts via Slack and email. Integrated user prompts to isolate affected machines, triggering LimaCharlie actions based on analyst input. Streamlined incident response with real-time notifications and automated isolation logic.
<br />


<h2>Languages and Utilities Used</h2>

- <b>LimaCharlie</b> 
- <b>Tines</b>
- <b>Slack</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> 
<h2>Program walk-through</h2>

<p align="center">
  <h2>SOAR-EDR Workflow:</h2>
<br /><img width="840" height="629" alt="Screenshot 2026-06-08 at 3 03 44 PM" src="https://github.com/user-attachments/assets/a6cb77f1-9cf7-461f-80f2-a8fd89f379c6" />
<br/>
  As shown in the picture above I first started with creating a playbook or rough draft on how I wanted my information to flow between the tools. LimaCharlie is first used to detect the threat and provide the information surrounding it. After detection the information would be forwarded to Tines which would be the center of the information channel. Tines will then email an administrator about the threat with various details such as the computer name, source IP,
process, etc. This same message will be sent to Slack which is what you will use to respond to the issue. Once this Slack message is received the admin will be prompted to isolate the machine. A final message will deliver explaining whether the machine was isolated or not. If yes is chosen, the isolated machine will be sectioned off from the network with no ability to connect back to it until reunited with the network.
<p align="center">
  <h2>Added computer to LimaCharlie::</h2>
  <img width="792" height="273" alt="Screenshot 2026-06-08 at 3 10 44 PM" src="https://github.com/user-attachments/assets/b1cc3ee9-3f3c-467e-a9b8-f3791afedd68" />

To initiate the project, I added my computer to the LimaCharlie infrastructure. This step was esse ntial to enable monitoring of system activity and to allow for isolation if a threat was detected.
 <br/>

<p align="center">
  <h2>Add threat to be detected:</h2>
<img width="496" height="348" alt="Screenshot 2026-06-08 at 3 12 50 PM" src="https://github.com/user-attachments/assets/03a67837-e2a6-4ae8-a6fc-cc9cebfafbe8" />
<br />
  The threat I chose to target was the Lazagne Project, an open-source tool designed to extract stored passwords from a system. To run Lazagne successfully, I had to temporarily disable the windows firewall. This allowed me to simulate a realistic threat scenario for testing the detection and response pipeline.
<br />
<p align="center">
  <h2>LimaCharlie Timeline:</h2>
<img width="834" height="382" alt="Screenshot 2026-06-08 at 3 15 47 PM" src="https://github.com/user-attachments/assets/9c9c2f9e-6727-4f37-9edf-521d92a23076" />
<br />
  After executing lazagne.exe, I used LimaCharlie’s timeline feature to pinpoint the exact process. This timeline provided all the necessary paths and metadata required to build a collection rule for detection.
<br />
<p align="center">
  <h2>Detection Rules:</h2>
  <img width="833" height="134" alt="Screenshot 2026-06-08 at 3 18 38 PM" src="https://github.com/user-attachments/assets/fb3b8b4a-603e-4de3-a1a5-f793f634800e" />
<img width="830" height="275" alt="Screenshot 2026-06-08 at 3 18 27 PM" src="https://github.com/user-attachments/assets/40f446b2-23fe-4301-bb6e-25d8428888df" />
<br />
Using the paths identified from the timeline, I created a detection and response rule in LimaCharlie. This rule was configured to trigger every time lazagne.exe was executed, enabling consistent monitoring and response.
<br />
<p align="center">
  <h2>Triggered Detection Response</h2>
<br />
  <img width="830" height="309" alt="Screenshot 2026-06-08 at 3 21 59 PM" src="https://github.com/user-attachments/assets/87e5db70-9d96-4cf9-800d-f5e0dc94db6f" />
<br />
This image shows an example of the detection response triggered during multiple test runs of lazagne.exe. Each instance generated alerts and activated the response pipeline, validating the effectiveness of the rule.
<br />
<p align="center">
  <h2>Tines Automation Pipeline: </h2>
<br />
<img width="731" height="349" alt="Screenshot 2026-06-08 at 3 26 19 PM" src="https://github.com/user-attachments/assets/c189e2f9-d719-4a46-bb2b-806c76aba5be" />

<br />
The final phase of the project involved transforming individual steps into a fully automated system. I used Tines to build a pipeline that would automatically prompt isolation of the machine once a detection rule was triggered. Above are the key components of the pipeline.
<br />
<p align="center">
  <h2>Webhook/Connection to LimaCharlie: </h2>
<br />
<img width="829" height="479" alt="Screenshot 2026-06-08 at 3 26 28 PM" src="https://github.com/user-attachments/assets/a2e8ab02-e43b-4c38-9ecf-4e5c26fc271d" />
<br />
I used Tines’s webhook feature to establish a connection with LimaCharlie. This webhook served as the entry point for the automation process, allowing Tines to receive data and initiate actions based on detections.

<br />
<p align="center">
  <h2>Adding a Slack Alert: </h2>
<br />
  <img width="838" height="584" alt="Screenshot 2026-06-08 at 3 29 27 PM" src="https://github.com/user-attachments/assets/68c73050-a5e5-42e1-9e08-0ebb6dce32dd" />

<br />
After setting up a Slack account and creating an alerts channel, I configured Tines to send formatted messages to Slack. These messages included details such as computer name, source IP, file path, and sensor ID. Each message also contained a direct link to the detection in LimaCharlie for quick access.
<br />
<p align="center">
  <h2>Alert by Email: </h2>
<br />
  <img width="819" height="541" alt="Screenshot 2026-06-08 at 3 29 42 PM" src="https://github.com/user-attachments/assets/7fa997d6-4ded-47c0-9792-eb751ecd5700" />
<br />
To ensure redundancy, I added email notifications alongside Slack alerts. This allowed the administration to receive threat information even when away from the workstation. The email included the same details and direct link to the detection.

<br />
<p align="center">
  <h2>Option to Isolate: </h2>
<br />
  <img width="406" height="487" alt="Screenshot 2026-06-08 at 3 30 12 PM" src="https://github.com/user-attachments/assets/653421bb-e4cc-4f14-8061-0d5e144d1ecf" />
<br />
Once the alerts were delivered, the administrator(myself) received a prompt asking whether to isolate the affected machne. If “Yes” was selected, the machine would be disconnected from the network until further evaluation.

<br />
<p align="center">
 <h2>After Boolean Evaluation: </h2>
<br />
<img width="467" height="517" alt="Screenshot 2026-06-08 at 3 34 37 PM" src="https://github.com/user-attachments/assets/47c50b6f-e256-4bb7-8c82-9d47cbb77440" />

<br />
If “No” was selected, a final Slack message was sent indicating that the machine was not isolated and required further investigation. If “Yes” was selected, Tines sent an HTTP request to verify the machine’s status, followed by another request to isolate it. A confirmation message was then sent to Slack stating that the machine had been successfully isolated.
<br />
<p align="center">
 <h2>After Boolean Evaluation: </h2>
<br />
  <img width="514" height="454" alt="Screenshot 2026-06-08 at 3 34 48 PM" src="https://github.com/user-attachments/assets/d09536b1-1ad5-498e-884f-186b23533937" />

<br />
After isolation, the machine was unable to interact with the network until the threat was resolved. This marked the completion of my project and my hands-on learning experience in building an automated security pipeline.
<br />

  
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
