1. ensure the tokenbridge-contracts were already successfully deployed
2. nvm install 12.22.0
3. nvm use 12.22.0
4. git clone https://github.com/esrdapp/tokenbridge.git
5. cd tokenbridge
6. yarn init
7. yarn install
8. you will have an empty "contracts" folder. We need to populate this folder with all of the tokenbridge-contracts we've already deployed:

cp -avr ../tokenbridge-contracts/* ./contracts

if this worked and copied correctly, your "tokenbridge/contracts/" folder will now be populated with "contracts", "deploy", "node_modules" folders, as well as a package.json file

9. cd deployment
10. edit hosts.yml to suit the deployment, and add in the private key of the ORACLE_VALIDATOR_ADDRESS (more details on hosts.yml can be found here: https://docs.tokenbridge.net/amb-bridge/arbitrary-message-bridge-deployment/2-tokenbridge-oracle-instance)
11. cd group_vars
12. edit hpb_xdai.yml file to suit - (note the file name "hpb_xdai" is the first line in hosts.yml from the previous step) - once again, refer to https://docs.tokenbridge.net/amb-bridge/arbitrary-message-bridge-deployment/2-tokenbridge-oracle-instance for details on editing the yml file.
13. cd ../../oracle

14. edit the Dockerfile (nano Dockerfile) line 26 by commenting out the line with #
#RUN NOYARNPOSTINSTALL=1 yarn install --frozen-lockfile --production (this avoids the scripting error later on)

15. mv .envexample .env
16. adjust the .env file as necessary, ensuring all values are correct and the validator address private key has been added. More details on the .env fields can be found here (in the "Oracle Configuration" section: https://github.com/esrdapp/tokenbridge/blob/master/CONFIGURATION.md 

17. run the following commands, remembering to add in the validator wallet address and the private key:

- env ORACLE_VALIDATOR_ADDRESS=<address> 
- env ORACLE_VALIDATOR_ADDRESS_PRIVATE_KEY=<private key>  docker-compose -f docker-compose-build.yml -f docker-compose.yml up -d --build
  
18. inside the oracle folder run npm install  
  
19. check the bridge is running with "docker-compose logs"
  
