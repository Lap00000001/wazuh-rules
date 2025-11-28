_______________________________________READ DESTINATIONdotconf.wd_______________________________________________
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

There are some rules for a wazuh monitoring and EDR installation in your network, i have been help by ChatGPT, i hope it will be usefull

the rules needs to be add in the 

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
there are some commands to install wazuh if you running on a ubuntu/debian :
(create the repo)

sudo tee /etc/apt/sources.list.d/wazuh.list > /dev/null << EOF
deb https://packages.wazuh.com/4.x/apt/ stable main
EOF

(import wazuh key)
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor -o /usr/share/keyrings/wazuh.gpg

(Tell apt to trust the key)
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" \
| sudo tee /etc/apt/sources.list.d/wazuh.list > /dev/null

(update & install the agent)
sudo apt update
sudo apt install wazuh-agent

(Connect the Agent to Your Manager)
sudo nano /var/ossec/etc/ossec.conf
 (Find this section and replace <MANAGER_IP>)
<server>
  <address>MANAGER_IP</address>
</server>

(restart the agent)
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

(check the agent)
sudo systemctl status wazuh-agent
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Alternativly you can try to follow the tutorial on the wazuh dashboard :
Follow these steps to deploy the Wazuh agent on your Linux endpoint.

Select your package manager and run the command below. Replace the WAZUH_MANAGER value with your Wazuh manager IP address or hostname:

APTYumDNFZYpp

WAZUH_MANAGER="10.0.0.2" apt-get install wazuh-agent
For additional deployment options such as agent name, agent group, and enrollment password, see the Deployment variables for Linux section.

obviously the IP and the key are depending of your configuration and should be upgraded !

--------------------------------------------------------------------------------------------------------------------------------------------------------------------
Petit rappel : 
pas d'<active responses> dans l'agent a moins qu'en local
l'agent doit determiner les cible ou collecter les logs
C’est uniquement dans le manager que tu mets les <active-response>
dans le fichier MANAGER ne pas oublier d'affiner :
Port scan → règles 200011, 200021, 200081
Brute-force MySQL → règles 400011, 400012, 400014
Brute-force SSH / système → règles 300011, 300013

ne pas oublier de 
sudo systemctl restart wazuh-agent
ou                     wazuh manarder
