# Elastic SIEM Home Lab

Objective

This Home Lab was aimed to setup elastic SIEM on a home lab for practice. The primary focus was to ingest logs from different virtual machine/s(In this writeup, I have used a Kali VM for monitoring, however you can add as many machines as you want to). Likewise, creating a Kibana dashboard for visualizing different events, as well as creating a rule that generates alert has also been done. 


Skills Learned

- Elastic Stack SIEM Configuration and Management
- Security Event Simulation and Analysis
- Virtualization and Alerting in SIEM

Tools Used

- Oracle VirtualBox for creating a safe virtual environment
- Kali Linux VM as an agent for SIEM
- Windows 11 as the host OS
- NMap for network scanning
- Security Information and Event Management (SIEM) for log ingestion and analysis

Steps

Step 1: Create free Elastic Account (14 days free trial used for this project)
  - Create a free Elastic Cloud account if you don't have one
  - Login to the Elastic Cloud Console and start using 14 days free trial
  - Now create an Elasticsearch deployment (Also select your deployment size and region)
  - Install and enable Elastic prebuilt rules


<img width="1440" alt="Screen Shot 2024-03-08 at 3 23 29 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/068a5414-a351-44d2-81c7-63c2190ceee1">

Here's a peek at my Deployment. 

Step 2: Setup Kali Linux VM 
- Download Kali Linux from official page.
- Download and install your preferred virtualization platform (I have used Oracle VirtualBox).
- Complete Kali Linux installation on the VirtualBox

Step 3: Installing Agent for Log Collection

<img width="751" alt="Screen Shot 2024-03-08 at 3 36 49 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/038f5b25-37fd-490b-a55f-7f5f72078904">

Deployed Elastic SIEM: Press Open and Start

<img width="749" alt="Screen Shot 2024-03-08 at 3 39 53 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/d7f2baf8-003f-4be9-a0ec-dbfbb1c06adb">

Press the HamBurger Menu: Then Add Integrations (Blue Button on the bottom left)

<img width="1440" alt="Screen Shot 2024-03-08 at 4 05 59 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/acf4f2ab-5cad-4cdb-a943-1e20344b1630">

Now select Elastic Defend and follow instructions to proceed further

<img width="1440" alt="Screen Shot 2024-03-08 at 4 09 50 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/5633003f-4e9d-4ca7-9853-0c90b4554b02">
Copy the commands to your Kali Terminal to install Elastic Agent on your Kali Host.

- After the Elastic Agent is successfully installed on your host, you'll see it on the Confirm Agent Enrollment just below the command you copied.
- You can also use the following command on your Kali Host to ensure the agent is successfully installed.
- Command: sudo systemctl status elastic-agent.service

<img width="681" alt="Screen Shot 2024-03-08 at 4 15 50 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/8a4bf9ed-8d0b-471d-a852-9c2724c83453">

Verifying Elastic Agent Installation

Step 4: Generating and Checking for Security Events

- Check if the SIEM is receiving Logs

<img width="1440" alt="Screen Shot 2024-03-08 at 4 28 37 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/b9817cba-cdd2-4fd6-a89f-82b80717451c">

Press the HamBurger Menu: Observability > Logs > Stream (We can confirm that the SIEM is receiving Logs from the Kali Linux VM Host)


  
- Use NMap to generate Logs: Install NMap on you Kali Linux VM. Then run a NMap command to scan an IP. I have chosen to scan the IP of my main host OS (i.e Windows 11). Please do not use NMap to scan targets or hosts that you do not have permission to scan or own. 
  
Step 5: Filtering Security Events in Elastic SIEM
- To filter specific security events, we will be using Elastic Search. We will focus on filtering and searching for 2 different type of processes (Ping, and NMap).
- Search Query for Ping
<img width="1440" alt="Screen Shot 2024-03-08 at 4 57 49 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/1232b4a0-8c27-4df2-9f97-3b5b00a0d488">
Search Query: process.args: "ping"

- Search Query for NMap
<img width="1440" alt="Screen Shot 2024-03-08 at 5 00 26 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/7bb32f4f-11ac-4711-a625-b5a8234fc0bb">
Search Query: process.args: "NMap"

<img width="1440" alt="Screen Shot 2024-03-08 at 5 09 09 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/396aa645-e06d-45ac-b269-46a602b0540b">
Hover over any event and press View Details to dig deep into the event. You can see here what command I ran on NMap and other details like host IP, and target IP, timestamp, processid, instance, etc. 

Step 6: Data Visualization
- We have now confirmed that we are getting logs to the SIEM.
- Likewise, we have also filtered out the specific events that we want to investigate (ping and NMap events)
- We will not be creating a simple dashboard that will visualize the count of events over a certaion period of time.

<img width="1440" alt="Screen Shot 2024-03-08 at 5 40 47 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/726e9394-dcc4-4dd6-b593-ebf4f109125c">
Go to HamBurger Menu > Analytics > Dashboards 

<img width="1440" alt="Screen Shot 2024-03-08 at 5 48 32 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/44b056d7-c580-4738-8c9b-bb56bfd39b1f">
Select Create visualization

<img width="1440" alt="Screen Shot 2024-03-08 at 6 00 46 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/28a7183b-55d5-4898-82dd-a2a3601ea594">

Select a visualation display type and then on the right of the screenshot you can see that I have selected timestamp and count for the visualization. Likewise, I have used query process.args: "NMap" to filter only the logs which were generated by NMap processes as this dashboard is created to visualize NMap events. Follow along and create your dashboard. 

I have created 2 seperate dashboards. Below are the two dashboards. 

<img width="1440" alt="Screen Shot 2024-03-08 at 6 13 37 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/ab7acea5-a1d2-4ca0-9834-2c2d557adef6">
A generic dashboard that depicts all the events generated from the Kali Linux VM

<img width="1440" alt="Screen Shot 2024-03-08 at 6 13 12 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/4d038e20-11f8-4a82-adca-585686740a5b">
A specific dashboard that depicts only the NMap events generated from the Kali Linux VM

Step 7: Alert Generation

Now that we have created dashboards for visualization of security events, we will now focus on generating alerts for specific security events (i.e. nmap scans for this case). 

<img width="1440" alt="Screen Shot 2024-03-08 at 6 55 26 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/35d70b38-a692-47ba-8ece-14818cf3b499">

Open the HamBurger Menu > Security > Alerts > "Manage rules" on the top right > Select "Create new rule" on the top right

<img width="1440" alt="Screen Shot 2024-03-08 at 6 57 15 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/a146606b-8539-4ec1-bfbf-97247b75a8b0">
Select "Custom query" > "Rule preview" 

<img width="1440" alt="Screen Shot 2024-03-08 at 7 00 26 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/5dfedd26-df27-47ce-aca4-71d2d31765c8">
On the "Custom query" search bar type "event.action: nmap_scan" since we are creating an alert for nmap scans. 

Severity level can be changed on the "About rule" section. Likewise, Schedule rate can also be changed under the "Schedule rule" section. Click "Continue" after you're done. 

<img width="1440" alt="Screen Shot 2024-03-08 at 7 04 45 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/e9e91069-b78e-4aa7-8545-353f4ebf6b97">
Select "Create & enable rule" after that the rule is created. 

<img width="1440" alt="Screen Shot 2024-03-08 at 7 12 09 pm" src="https://github.com/rifua/Elastic-SIEM-Home-Lab/assets/160899842/33876cf9-4ebc-4fa1-a1e4-7033e0af785d">
Security > Rules > Detection rules (SIEM) [To check if the rule is active]
