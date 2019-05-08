Start Docker Container 
----------------------
 cd artifacts/

 docker-compose -f docker-compose.yml up -d
 
Run Cli Docker Container 
-------------------------
docker exec -it cli bash


Create Channel
---------------

export CHANNEL_NAME=insurance-channel

peer channel create -o orderer.insurance.com:7050 -c $CHANNEL_NAME -f ./channel/channel.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/insurance.com/orderers/orderer.insurance.com/msp/tlscacerts/tlsca.insurance.com-cert.pem

Join Insurer1 to Channel
------------------------
peer channel join -b insurance-channel.block


Join Insurer2 to Channel
------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer2.insurance.com/users/Admin@insurer2.insurance.com/msp CORE_PEER_ADDRESS=peer0.insurer2.insurance.com:7051 CORE_PEER_LOCALMSPID="Insurer2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer2.insurance.com/peers/peer0.insurer2.insurance.com/tls/ca.crt peer channel join -b insurance-channel.block


Join ReInsurer to Channel
-------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/reinsurer.insurance.com/users/Admin@reinsurer.insurance.com/msp CORE_PEER_ADDRESS=peer0.reinsurer.insurance.com:7051 CORE_PEER_LOCALMSPID="ReInsurerMSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/reinsurer.insurance.com/peers/peer0.reinsurer.insurance.com/tls/ca.crt peer channel join -b insurance-channel.block

Join ExternalSystem to Channel
-------------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/externalsystem.insurance.com/users/Admin@externalsystem.insurance.com/msp CORE_PEER_ADDRESS=peer0.externalsystem.insurance.com:7051 CORE_PEER_LOCALMSPID="ExternalSystemMSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/externalsystem.insurance.com/peers/peer0.externalsystem.insurance.com/tls/ca.crt peer channel join -b insurance-channel.block


Update Insurer1 Anchor Peer to Channel
---------------------------------------
peer channel update -o orderer.insurance.com:7050 -c $CHANNEL_NAME -f ./channel/Insurer1MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/insurance.com/orderers/orderer.insurance.com/msp/tlscacerts/tlsca.insurance.com-cert.pem

Update Insurer2 Anchor Peer to Channel
--------------------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer2.insurance.com/users/Admin@insurer2.insurance.com/msp CORE_PEER_ADDRESS=peer0.insurer2.insurance.com:7051 CORE_PEER_LOCALMSPID="Insurer2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer2.insurance.com/peers/peer0.insurer2.insurance.com/tls/ca.crt peer channel update -o orderer.insurance.com:7050 -c $CHANNEL_NAME -f ./channel/Insurer2MSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/insurance.com/orderers/orderer.insurance.com/msp/tlscacerts/tlsca.insurance.com-cert.pem

Update ReInsurer Anchor Peer to Channel
---------------------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/reinsurer.insurance.com/users/Admin@reinsurer.insurance.com/msp CORE_PEER_ADDRESS=peer0.reinsurer.insurance.com:7051 CORE_PEER_LOCALMSPID="ReInsurerMSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/reinsurer.insurance.com/peers/peer0.reinsurer.insurance.com/tls/ca.crt peer channel update -o orderer.insurance.com:7050 -c $CHANNEL_NAME -f ./channel/ReInsurerMSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/insurance.com/orderers/orderer.insurance.com/msp/tlscacerts/tlsca.insurance.com-cert.pem

Update ExternalSystem Anchor Peer to Channel
--------------------------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/externalsystem.insurance.com/users/Admin@externalsystem.insurance.com/msp CORE_PEER_ADDRESS=peer0.externalsystem.insurance.com:7051 CORE_PEER_LOCALMSPID="ExternalSystemMSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/externalsystem.insurance.com/peers/peer0.externalsystem.insurance.com/tls/ca.crt peer channel update -o orderer.insurance.com:7050 -c $CHANNEL_NAME -f ./channel/ExternalSystemMSPanchors.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/insurance.com/orderers/orderer.insurance.com/msp/tlscacerts/tlsca.insurance.com-cert.pem



Package Chaincode
-----------------
peer chaincode package -n mycc -l node -v 1.0 -p /opt/gopath/src/github.com/hyperledger/fabric/peer/chaincode/node -s -S ccpack.out

Install Chaincode in Insurer1
-----------------------------
peer chaincode install ccpack.out

Install Chaincode in Insurer2
-----------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer2.insurance.com/users/Admin@insurer2.insurance.com/msp CORE_PEER_ADDRESS=peer0.insurer2.insurance.com:7051 CORE_PEER_LOCALMSPID="Insurer2MSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer2.insurance.com/peers/peer0.insurer2.insurance.com/tls/ca.crt peer chaincode install ccpack.out

Install Chaincode in ReInsurer
------------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/reinsurer.insurance.com/users/Admin@reinsurer.insurance.com/msp CORE_PEER_ADDRESS=peer0.reinsurer.insurance.com:7051 CORE_PEER_LOCALMSPID="ReInsurerMSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/reinsurer.insurance.com/peers/peer0.reinsurer.insurance.com/tls/ca.crt peer chaincode install ccpack.out

Install Chaincode in External System
-------------------------------------
CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/externalsystem.insurance.com/users/Admin@externalsystem.insurance.com/msp CORE_PEER_ADDRESS=peer0.externalsystem.insurance.com:7051 CORE_PEER_LOCALMSPID="ExternalSystemMSP" CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/externalsystem.insurance.com/peers/peer0.externalsystem.insurance.com/tls/ca.crt peer chaincode install ccpack.out

Instantiate chaincode in any Org peer
--------------------------------------
peer chaincode instantiate -o orderer.insurance.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/insurance.com/orderers/orderer.insurance.com/msp/tlscacerts/tlsca.insurance.com-cert.pem -C $CHANNEL_NAME -n mycc -l node -v 1.0 -c '{"Args":[""]}' -P "AND ('Insurer1MSP.peer','Insurer2MSP.peer','ReInsurerMSP.peer','ExternalSystemMSP.peer')"

Invoke Transaction
-------------------
peer chaincode invoke -o orderer.insurance.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/insurance.com/orderers/orderer.insurance.com/msp/tlscacerts/tlsca.insurance.com-cert.pem -C $CHANNEL_NAME -n mycc --peerAddresses peer0.insurer1.insurance.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer1.insurance.com/peers/peer0.insurer1.insurance.com/tls/ca.crt --peerAddresses peer0.insurer2.insurance.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/insurer2.insurance.com/peers/peer0.insurer2.insurance.com/tls/ca.crt --peerAddresses peer0.reinsurer.insurance.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/reinsurer.insurance.com/peers/peer0.reinsurer.insurance.com/tls/ca.crt --peerAddresses peer0.externalsystem.insurance.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/externalsystem.insurance.com/peers/peer0.externalsystem.insurance.com/tls/ca.crt -c '{"Args":["claimInsurance","CLM01","Ponmudi","1200"]}'

Query Transaction
-----------------
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["findClaim","CLM01"]}'

 
 
