SOL_URL=https://github.com/ChainSafe/chainbridge-solidity
SOL_VERSION="v1.0.0"

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

# cb-sol-cli commands
SRC_GATEWAY = https://api.baobab.klaytn.net:8651
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

# Example
FEE = 100
AMOUNT = 10000
APPROVE = 10000000000000000000

# 소스 체인에 Bridge, ERC20, ERC20 Handler Contract 배포
deploysrc:
	cb-sol-cli --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 deploy \
	--bridge --erc20 --erc20Handler \
	--relayers ${SRC_ADDR} \
	--relayerThreshold 1 \
	--chainId 0 \
	--fee ${FEE}

# 목적 체인에 Bridge, ERC20, ERC20 Handler Contract 배포
deploydst:
	cb-sol-cli --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 deploy \
	--bridge --erc20 --erc20Handler \
	--relayers ${DST_ADDR} \
	--relayerThreshold 1 \
	--chainId 1 \
	--fee ${FEE}

# 소스 체인의 Bridge에 Resource ID를 키로 Handler를, Handler에 Token을 등록한다.
registersrc:
	cb-sol-cli --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 bridge register-resource \
    --bridge ${SRC_BRIDGE} \
    --handler ${SRC_HANDLER} \
    --resourceId ${RESOURCE_ID} \
    --targetContract ${SRC_TOKEN}

# 목적 체인에 Bridge에 Resource ID를 키로 Handler를, Handler에 Token을 등록한다.
registerdst:
	cb-sol-cli --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 bridge register-resource \
    --bridge ${DST_BRIDGE} \
    --handler ${DST_HANDLER} \
    --resourceId ${RESOURCE_ID} \
    --targetContract ${DST_TOKEN}

# 소스 체인의 Bridge에 등록된 Handler에게 Token을 소각할 권리를 부여한다.
burnablesrc:
	cb-sol-cli --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 bridge set-burn \
    --bridge ${SRC_BRIDGE} \
    --handler ${SRC_HANDLER} \
    --tokenContract ${SRC_TOKEN}

# 목적 체인의 Bridge에 등록된 Handler에게 Token을 소각할 권리를 부여한다.
burnabledst:
	cb-sol-cli --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 bridge set-burn \
    --bridge ${DST_BRIDGE} \
    --handler ${DST_HANDLER} \
    --tokenContract ${DST_TOKEN}

# 소스 체인의 Bridge에 등록된 Handler에게 Token을 민팅할 권리를 부여한다.
mintablesrc:
	cb-sol-cli --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 add-minter \
    --minter ${SRC_HANDLER} \
    --erc20Address ${SRC_TOKEN}

# 대상 체인의 Bridge에 등록된 Handler에게 Token을 민팅할 권리를 부여한다.
mintabledst:
	cb-sol-cli --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 erc20 add-minter \
    --minter ${DST_HANDLER} \
    --erc20Address ${DST_TOKEN}

# 소스 체인의 Handler가 민팅할 수 있는 최대 수량을 지정한다.
approvesrc:
	cb-sol-cli --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 approve \
    --amount ${APPROVE} \
    --erc20Address ${SRC_TOKEN} \
    --recipient ${SRC_HANDLER}

# 대상 체인의 Handler가 민팅할 수 있는 최대 수량을 지정한다.
approvedst:
	cb-sol-cli --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 erc20 approve \
    --amount ${APPROVE} \
    --erc20Address ${DST_TOKEN} \
    --recipient ${DST_HANDLER}


# deposit method 호출은 스크립트 단에서 Wei를 전달하지 못하는 문제로 Remix에서 직접 수행하였음.
depositsrc:
	cb-sol-cli --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 deposit \
	--amount ${AMOUNT} \
	--dest 1 \
	--bridge ${SRC_BRIDGE} \
	--recipient ${DST_ADDR} \
	--resourceId ${RESOURCE_ID}

depositdst:
	cb-sol-cli --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 10000000000 erc20 deposit \
	--amount ${AMOUNT} \
	--dest 1 \
	--bridge ${DST_BRIDGE} \
	--recipient ${SRC_ADDR} \
	--resourceId ${RESOURCE_ID}

# 소스 체인의 전송 주소와 동일한 목적 체인의 주소로 토큰을 민팅시키기 위해 데이터를 ABI Encoding 한다. 이 데이터는 Deposit 컨트랙트를 호출할 때 전달한다.
encodeaddramtsrc:
	cb-sol-cli erc20 encodeaddramt  \
	--recipient ${SRC_ADDR} \
	--amount ${AMOUNT}

# 목적 체인의 전송 주소와 동일한 소스 체인의 주소로 토큰을 민팅시키기 위해 데이터를 ABI Encoding 한다. 이 데이터는 Deposit 컨트랙트를 호출할 때 전달한다.
encodeaddramtdst:
	 cb-sol-cli erc20 encodeaddramt  \
	--recipient ${DST_ADDR} \
	--amount ${AMOUNT}

# 소스 체인의 전송자가 보유한 소스 체인의 토큰 수량을 조회한다.
balancesrc:
	cb-sol-cli --url ${SRC_GATEWAY} --privateKey ${SRC_PK} --gasPrice 30000000000 erc20 balance \
	--address ${SRC_ADDR} \
	--erc20Address ${SRC_TOKEN}

# 목적 체인의 전송자가 보유한 목적 체인의 토큰 수량을 조회한다.
balancedst:
	cb-sol-cli --url ${DST_GATEWAY} --privateKey ${DST_PK} --gasPrice 30000000000 erc20 balance \
	--address ${DST_ADDR} \
	--erc20Address ${DST_TOKEN}
