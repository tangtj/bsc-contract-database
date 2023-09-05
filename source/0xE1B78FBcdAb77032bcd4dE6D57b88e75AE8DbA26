// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

library SafeMath {
    function add(uint a, uint b) internal pure returns (uint) {
        return a + b;
    }

    function sub(uint a, uint b) internal pure returns (uint) {
        return a - b;
    }

    function mul(uint a, uint b) internal pure returns (uint) {
        return a * b;
    }

    function div(uint a, uint b) internal pure returns (uint) {
        return a / b;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract CadinuAirDrop {
    using SafeMath for uint256;

    IERC20 public token;
    address public owner;

    event TransferredToken(address indexed to, uint256 value);
    event FailedTransfer(address indexed to, uint256 value);

    constructor() {
        owner = msg.sender;
        token = IERC20(0x6e64fCF15Be3eB71C3d42AcF44D85bB119b2D98b);  // Set your token address here
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "New owner cannot be the zero address");
        owner = newOwner;
    }
  
    function cadinuAirDrop(address[] memory dests, uint256 value) external onlyOwner {
        uint256 i = 0;
        uint256 toSend = value.mul(10**18);
        while (i < dests.length) {
            sendInternally(dests[i], toSend, value);
            i++;
        }
    }  

    function sendInternally(address recipient, uint256 tokensToSend, uint256 valueToPresent) internal {
        if (recipient == address(0)) return;

        uint256 tokensAvailable = token.balanceOf(address(this));
        if (tokensAvailable >= tokensToSend) {
            token.transfer(recipient, tokensToSend);
            emit TransferredToken(recipient, valueToPresent);
        } else {
            emit FailedTransfer(recipient, valueToPresent); 
        }
    }

    function destroy() external onlyOwner {
        uint256 balance = token.balanceOf(address(this));
        require(balance > 0, "No tokens available for destruction");
        token.transfer(owner, balance);
        payable(owner).transfer(address(this).balance);
        selfdestruct(payable(owner));
    }
}