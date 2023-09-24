// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function balanceOf(address account) external view returns (uint256);
}

contract POF {

    IBEP20 public USDT;
    IBEP20 public BTC;

    mapping(bytes32 => address) private proofs;
    mapping(address => bytes32) private proofs_;
    mapping(address => uint256) private balances;
    mapping(address => address) private tokenInfos;

    constructor(address _USDT, address _BTC) {
        USDT = IBEP20(_USDT);
        BTC = IBEP20(_BTC);
    }

    // Generates a hash using the balance and the address of the holder
    function generateHash(address holder, address token) internal returns (bytes32) {
        uint256 balance;
        if (token == address(USDT)) {
            balance = USDT.balanceOf(holder);
            balances[holder] = balance;
            tokenInfos[holder] = token;
        } else if (token == address(BTC)) {
            balance = BTC.balanceOf(holder);
            balances[holder] = balance;
            tokenInfos[holder] = token;
        } else {
            revert("Invalid token address");
        }
        return keccak256(abi.encodePacked(holder, balance));
    }

    // Submits the generated hash for the holder and token
    function submitProof(address token) external {
        bytes32 proof = generateHash(msg.sender, token);
        proofs[proof] = msg.sender;
        proofs_[msg.sender] = proof;
    }

    // Requests the generated hash for the holder
    function requestProof() external view returns (bytes32) {
        return proofs_[msg.sender];
    }

    // Verifies if the hash for a holder and a token matches the one stored in the contract
    function verifyProof(bytes32 proof) external view returns (address, uint256) {
        address tokeninfo = tokenInfos[proofs[proof]];
        uint256 balance = balances[proofs[proof]];
        require(balance == IBEP20(tokeninfo).balanceOf(proofs[proof]), "Incorrect balance");
        return (tokeninfo, balance);
    }
}