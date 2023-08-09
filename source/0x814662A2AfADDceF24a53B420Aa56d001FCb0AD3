// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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

contract MultiTokenEscrow {
    address public owner;

    struct TokenInfo {
        IERC20 tokenContract;
        mapping(address => uint256) balances;
    }

    mapping(address => TokenInfo) public tokenBalances;

    event Deposited(address indexed user, address indexed token, uint256 amount);
    event Withdrawn(address indexed user, address indexed token, uint256 amount);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function deposit(address _tokenAddress, uint256 _amount) external {
        require(_amount > 0, "Amount must be greater than 0");

        TokenInfo storage tokenInfo = tokenBalances[_tokenAddress];
        require(address(tokenInfo.tokenContract) != address(0), "Token is not supported");

        tokenInfo.tokenContract.transferFrom(msg.sender, address(this), _amount);
        tokenInfo.balances[msg.sender] += _amount;

        emit Deposited(msg.sender, _tokenAddress, _amount);
    }

    function withdraw(address _tokenAddress, uint256 _amount) external {
        require(_amount > 0, "Amount must be greater than 0");

        TokenInfo storage tokenInfo = tokenBalances[_tokenAddress];
        require(address(tokenInfo.tokenContract) != address(0), "Token is not supported");
        require(tokenInfo.balances[msg.sender] >= _amount, "Insufficient balance");

        tokenInfo.balances[msg.sender] -= _amount;
        tokenInfo.tokenContract.transfer(msg.sender, _amount);

        emit Withdrawn(msg.sender, _tokenAddress, _amount);
    }

    function addSupportedToken(address _tokenAddress) external onlyOwner {
        require(_tokenAddress != address(0), "Invalid token address");
        require(tokenBalances[_tokenAddress].tokenContract == IERC20(address(0)), "Token already supported");

        tokenBalances[_tokenAddress].tokenContract = IERC20(_tokenAddress);
    }

    function changeOwner(address _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    function getUserBalance(address _user, address _tokenAddress) external view returns (uint256) {
        return tokenBalances[_tokenAddress].balances[_user];
    }
}