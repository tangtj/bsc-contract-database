// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract FonkyProxy {
    address public implementation;
    address public owner;

    mapping(address => uint256) public txCounts;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    function transferOwnership(address _owner) external onlyOwner {
        owner = _owner;
    }

    constructor(address _implementation) {
        implementation = _implementation;
        owner = msg.sender; // set the contract deployer as the owner
    }

    receive() external payable {}

    function upgradeTo(address newImplementation) external onlyOwner {
        require(
            newImplementation != address(0),
            "Invalid implementation address"
        );
        implementation = newImplementation;
    }

    fallback() external payable {
        address impl = implementation;
        require(impl != address(0));
        txCounts[msg.sender] = txCounts[msg.sender] + 1;

        assembly {
            let ptr := mload(0x40)
            calldatacopy(ptr, 0, calldatasize())
            let result := delegatecall(gas(), impl, ptr, calldatasize(), 0, 0)
            let size := returndatasize()
            returndatacopy(ptr, 0, size)

            switch result
            case 0 {
                revert(ptr, size)
            }
            default {
                return(ptr, size)
            }
        }
    }

    function withdrawETH(uint256 value) external onlyOwner {
        payable(msg.sender).transfer(value);
    }
}