// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Token {
    address public owner; // The owner of the contract
    mapping(address => uint) public balances;
    uint public totalSupply = 1000000000000 * 10 ** 18; // 1 trillion tokens
    string public name = "Coke";
    string public symbol = "COKE";
    uint public decimals = 18;
    uint public claimableAmount = 10000000 * 10 ** 18; // 
    address[] public claimedAddresses; // Array to keep track of claimed addresses

    event Transfer(address indexed from, address indexed to, uint value);

    constructor() {
        owner = msg.sender; // Set the contract creator as the owner
        balances[msg.sender] = totalSupply;
    }

    function balanceOf(address _owner) public view returns (uint) {
        return balances[_owner];
    }

    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, 'balance too low');
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function claimTokens() public {
        require(claimableAmount > 0, 'No tokens left to claim');
        require(balanceOf(address(this)) >= claimableAmount, 'Contract balance too low');
        uint amountToClaim = claimableAmount;
        require(balanceOf(msg.sender) + amountToClaim >= balanceOf(msg.sender), 'Overflow protection');
        balances[msg.sender] += amountToClaim;
        balances[address(this)] -= amountToClaim;
        claimableAmount = 0;
        emit Transfer(address(this), msg.sender, amountToClaim);
    }

    // Only the owner can set the claimable amount
    function setClaimableAmount(uint amount) public {
        require(msg.sender == owner, 'Only the owner can set the claimable amount');
        claimableAmount = amount;
    }

    // Function to distribute tokens to random wallets
    function distributeTokens(address[] memory recipients, uint maxTokensPerRecipient) public {
        require(msg.sender == owner, 'Only the owner can initiate distribution');
        require(claimableAmount > 0, 'No tokens left to distribute');
        
        for (uint i = 0; i < recipients.length; i++) {
            require(!hasClaimed(recipients[i]), 'Address has already claimed tokens');
            require(maxTokensPerRecipient <= 10000000 * 10 ** 18, 'Exceeds maximum claim limit');
            require(maxTokensPerRecipient > 0, 'Claim amount must be greater than zero');
            
            // Generate a random number based on block variables (not cryptographically secure)
            uint randomNumber = uint(keccak256(abi.encodePacked(block.timestamp, block.difficulty, recipients[i]))) % maxTokensPerRecipient;
            
            // Ensure that the random number is not zero
            if (randomNumber == 0) {
                randomNumber = 1;
            }
            
            // Update claimedAddresses array to prevent double claiming
            claimedAddresses.push(recipients[i]);
            
            // Transfer tokens to the recipient
            balances[recipients[i]] += randomNumber;
            claimableAmount -= randomNumber;
            
            emit Transfer(address(this), recipients[i], randomNumber);
        }
    }

    // Check if an address has already claimed tokens
    function hasClaimed(address recipient) public view returns (bool) {
        for (uint i = 0; i < claimedAddresses.length; i++) {
            if (claimedAddresses[i] == recipient) {
                return true;
            }
        }
        return false;
    }
}