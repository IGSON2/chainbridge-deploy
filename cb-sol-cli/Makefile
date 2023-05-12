SOL_URL=https://github.com/ChainSafe/chainbridge-solidity
SOL_VERSION="v1.0.0"

SRC_GATEWAY = https://api.baobab.klaytn.net:8651
# DST_GATEWAY = https://data-seed-prebsc-1-s1.binance.org:8545
DST_GATEWAY = https://rpc-mumbai.maticvigil.com/

SRC_ADDR = 0x2345BF77D1De9eacf66FE81a09a86CfAb212a542
SRC_PK = 56a7a32def4a15c0187e8a076baf4c76db60bbf9ad59b8396d0b2174f9d5533e
DST_ADDR = 0x48179c14aeDf2bfaCf7bF6328A2eDACd6d3DA426
DST_PK = 23e44e5019b54dd4e8c02db9cf4a6e4563c6fe32352bcede43cded05fac05404

RESOURCE_ID = 0x0000000000000000000000000000000000000000000000000000000000000002

SRC_BRIDGE = 0x530f36421c3c11Cd692E542e8B63eFEb2fde14b9
SRC_HANDLER = 0x88b839120982db8785D686F591a56476C8a5aB63
SRC_TOKEN = 0x7d41bD17e9B0132C5cd0B852706083866d6E9e56

DST_BRIDGE = 0x1705F0DE28791e7386Dc335e8d02DCbe1C4a6E2d
DST_HANDLER = 0xE2e58E0e3b5DFA25c7E4D8636a35cECe92Aef3AF
DST_TOKEN = 0xC2Ef08A5b367F7d8731B924Ff917d3061ba41AdD

FEE = 10000
AMOUNT = 10
APPROVE = 10000000000000000000

fetch-contracts:
	@echo " > \033[32mFetching chainbridge-solidity contracts... \033[0m "
	git clone ${SOL_URL} && cd chainbridge-solidity && git checkout ${SOL_VERSION}

compile:
	cd chainbridge-solidity && npm install && npx truffle compile

install: fetch-contracts compile
	@echo " > \033[32mInstalling cb-sol-cli... \033[0m "
	npm link .

clean:
	rm -rf chainbridge-solidity/

deploysrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 deploy \
	--bridge --erc20 --erc20Handler \
	--relayers ${SRC_ADDR} \
	--relayerThreshold 1 \
	--chainId 0 \
	--fee ${FEE}

deploydst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 deploy \
	--bridge --erc20 --erc20Handler \
	--relayers ${DST_ADDR} \
	--relayerThreshold 1 \
	--chainId 1 \
	--fee ${FEE}

registersrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 bridge register-resource \
    --bridge ${SRC_BRIDGE} \
    --handler ${SRC_HANDLER} \
    --resourceId ${RESOURCE_ID} \
    --targetContract ${SRC_TOKEN}

registerdst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 bridge register-resource \
    --bridge ${DST_BRIDGE} \
    --handler ${DST_HANDLER} \
    --resourceId ${RESOURCE_ID} \
    --targetContract ${DST_TOKEN}

burnablesrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 bridge set-burn \
    --bridge ${SRC_BRIDGE} \
    --handler ${SRC_HANDLER} \
    --tokenContract ${SRC_TOKEN}

burnabledst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 bridge set-burn \
    --bridge ${DST_BRIDGE} \
    --handler ${DST_HANDLER} \
    --tokenContract ${DST_TOKEN}

mintablesrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 add-minter \
    --minter ${SRC_HANDLER} \
    --erc20Address ${SRC_TOKEN}

mintabledst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 erc20 add-minter \
    --minter ${DST_HANDLER} \
    --erc20Address ${DST_TOKEN}

approvesrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 approve \
    --amount ${APPROVE} \
    --erc20Address ${SRC_TOKEN} \
    --recipient ${SRC_HANDLER}

approvedst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 erc20 approve \
    --amount ${APPROVE} \
    --erc20Address ${DST_TOKEN} \
    --recipient ${DST_HANDLER}

# deposit method 호출은 스크립트 단에서 Wei를 전달하지 못하는 문제로 Remix에서 직접 수행하였음.

depositsrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 deposit \
	--amount ${AMOUNT} \
	--dest 1 \
	--bridge ${SRC_BRIDGE} \
	--recipient ${DST_ADDR} \
	--resourceId ${RESOURCE_ID}

depositdst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 erc20 deposit \
	--amount ${AMOUNT} \
	--dest 1 \
	--bridge ${DST_BRIDGE} \
	--recipient ${SRC_ADDR} \
	--resourceId ${RESOURCE_ID}

encodeamt:
	 ./index.js erc20 encodeamt  \
	 --amount ${AMOUNT}

encodeaddramtsrc:
	./index.js erc20 encodeaddramt  \
	--recipient ${SRC_ADDR} \
	--amount 100

encodeaddramtdst:
	 ./index.js erc20 encodeaddramt  \
	--recipient ${DST_ADDR} \
	--amount 1000

balancesrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 balance \
	--address ${SRC_ADDR} \
	--erc20Address ${SRC_TOKEN}

balancedst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 30000000000 erc20 balance \
	--address ${DST_ADDR} \
	--erc20Address ${DST_TOKEN}
