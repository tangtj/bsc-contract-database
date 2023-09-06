// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

interface IToken {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract A4 {
    IToken public token;
    address public owner;

    mapping(address => bool) public processedRecipients;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not authorized");
        _;
    }

    constructor(address _token) {
        token = IToken(_token);
        owner = msg.sender;
    }

    function bulkSend(address[] calldata recipients, uint256 value) external onlyOwner {
        uint256 valueInWei = value * 10**18; 
        uint256 totalToSend = valueInWei * recipients.length;
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance >= totalToSend, "Not enough tokens in contract for the airdrop");

        for (uint256 i = 0; i < recipients.length; i++) {
            if (!processedRecipients[recipients[i]]) {
                token.transfer(recipients[i], valueInWei);
                processedRecipients[recipients[i]] = true;
            }
        }
    }

    // Function to withdraw any mistakenly sent BNB
    function withdrawBNB() external onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

    // Function to withdraw any mistakenly sent ERC-20 tokens
    function withdrawToken(address _tokenAddress) external onlyOwner {
        IToken _token = IToken(_tokenAddress);
        uint256 balance = _token.balanceOf(address(this));
        _token.transfer(owner, balance);
    }
}