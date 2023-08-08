// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract liquidityReward {
    address public owner;
    mapping(address => bool) public authorizedAddresses; // List of addresses allowed to access

    event TokensWithdrawn(address indexed user, uint256 tokenAmount, address tokenAddress);

    constructor() {
        owner = msg.sender;
        authorizedAddresses[msg.sender] = true; // Default owner allowed access
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyAuthorized() {
        require(authorizedAddresses[msg.sender], "You are not authorized to interact with this contract");
        _;
    }

    function updateAuthorizedAddresses(address[] calldata _addresses, bool[] calldata _status) external onlyOwner {
        require(_addresses.length == _status.length, "Arrays length mismatch");

        for (uint256 i = 0; i < _addresses.length; i++) {
            authorizedAddresses[_addresses[i]] = _status[i];
        }
    }

    function Harvest(address _tokenAddress, uint256 _amount) external onlyAuthorized {
        require(_amount > 0, "Amount must be greater than 0");

        IERC20 token = IERC20(_tokenAddress);
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance >= _amount, "Contract does not have enough tokens");

        require(token.transfer(msg.sender, _amount), "Token transfer failed");

        emit TokensWithdrawn(msg.sender, _amount, _tokenAddress);
    }
}