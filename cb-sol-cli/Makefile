SOL_URL=https://github.com/ChainSafe/chainbridge-solidity
SOL_VERSION="v1.0.0"

SRC_GATEWAY = https://api.baobab.klaytn.net:8651
# DST_GATEWAY = https://data-seed-prebsc-1-s1.binance.org:8545
DST_GATEWAY = https://rpc-mumbai.maticvigil.com/

SRC_ADDR = 0x2345BF77D1De9eacf66FE81a09a86CfAb212a542
SRC_PK = 56a7a32def4a15c0187e8a076baf4c76db60bbf9ad59b8396d0b2174f9d5533e
DST_ADDR = 0x48179c14aeDf2bfaCf7bF6328A2eDACd6d3DA426
DST_PK = 23e44e5019b54dd4e8c02db9cf4a6e4563c6fe32352bcede43cded05fac05404

RESOURCE_ID = 0x0000000000000000000000000000000000000000000000000000000000000001

SRC_BRIDGE = 0xa7D0e7fBc0163bd7c9aA5a7BBc218ccf5DFF07eE
SRC_HANDLER = 0x7731cB49EE28bea697e4835c849abBD10468Adfa
SRC_TOKEN = 0x6A580CfEa6D6eE2e19a2AaD4b7fD8D86e3B7181B

DST_BRIDGE = 0x07a1F0E0fa69aefE06E3C4C5f1F14017CFF0FAE5
DST_HANDLER = 0x596375775B46ddfaB95986Ed999e99D88Afbadac
DST_TOKEN = 0xCA6332E7A41c7B6E2D8EBBA13296257a256ca2B9

FEE = 0.1
AMOUNT = 10000000000000000
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

registergenericsrc:
	./index.js --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 bridge register-generic-resource \
    --bridge ${SRC_BRIDGE} \
    --handler ${SRC_HANDLER} \
    --resourceId ${RESOURCE_ID} \
	--execute \
    --targetContract ${SRC_TOKEN}

registergenericdst:
	./index.js --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 bridge register-generic-resource \
    --bridge ${DST_BRIDGE} \
    --handler ${DST_HANDLER} \
    --resourceId ${RESOURCE_ID} \
	--execute \
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

datasrc:
	 ./index.js erc20 printdata  \
	 --recipient ${DST_ADDR} \
	 --amount ${AMOUNT}

datadst:
	 ./index.js erc20 printdata  \
	 --recipient ${SRC_ADDR} \
	 --amount ${AMOUNT}