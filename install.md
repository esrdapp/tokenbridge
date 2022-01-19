1. ensure the tokenbridge-contracts were already successfully deployed
2. nvm install 12.22.0
3. nvm use 12.22.0
4. git clone https://github.com/esrdapp/tokenbridge.git
5. cd tokenbridge
6. yarn init
7. yarn install
8. you will have an empty "contracts" folder. We need to populate this folder with the tokenbridge contracts we already deployed:

cp -a ../tokenbridge-contracts ./tokenbridge/contracts/

if this worked and copied correctly, your "tokenbridge/contracts/" folder will now be populated with "contracts", "deploy", "node_modules" folders, as well as a package.json file

9. cd oracle
10. edit the Dockerfile (nano Dockerfile) line 26 by commenting out the line with #
#RUN NOYARNPOSTINSTALL=1 yarn install --frozen-lockfile --production (this avoids the scripting error later on)

11. run the following command, adding in the validator wallet and the private key from deploymentUtils.js that can be located in the file when you originally deployed tokenbridge-contracts (in tokenbridge-contracts/src/deploymentUtils.js)

env ORACLE_VALIDATOR_ADDRESS=<address> env ORACLE_VALIDATOR_ADDRESS_PRIVATE_KEY=<private key>  docker-compose -f docker-compose-build.yml -f docker-compose.yml up -d --build
