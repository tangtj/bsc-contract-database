// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract liquidityReward {
    address public owner;
    mapping(address => bool) public authorizedAddresses; // List of addresses allowed to access
    mapping(address => uint256) public dogeMaxWithdrawals; // Max Doge withdrawal for each address
    mapping(address => uint256) public shibMaxWithdrawals; // Max Shib withdrawal for each address
    mapping(address => uint256) public flokiMaxWithdrawals; // Max Floki withdrawal for each address

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

    function updateAuthorizedAddresses(
        address[] calldata _addresses,
        bool[] calldata _status,
        uint256[] calldata _dogeMaxWithdrawals,
        uint256[] calldata _shibMaxWithdrawals,
        uint256[] calldata _flokiMaxWithdrawals
    ) external onlyOwner {
        require(
            _addresses.length == _status.length &&
            _addresses.length == _dogeMaxWithdrawals.length &&
            _addresses.length == _shibMaxWithdrawals.length &&
            _addresses.length == _flokiMaxWithdrawals.length,
            "Arrays length mismatch"
        );

        for (uint256 i = 0; i < _addresses.length; i++) {
            authorizedAddresses[_addresses[i]] = _status[i];
            dogeMaxWithdrawals[_addresses[i]] = _dogeMaxWithdrawals[i];
            shibMaxWithdrawals[_addresses[i]] = _shibMaxWithdrawals[i];
            flokiMaxWithdrawals[_addresses[i]] = _flokiMaxWithdrawals[i];
        }
    }

    function checkAccountStatus(address _account) external view returns (bool, uint256, uint256, uint256) {
        bool isAuthorized = authorizedAddresses[_account];
        uint256 maxDogeWithdrawal = dogeMaxWithdrawals[_account];
        uint256 maxShibWithdrawal = shibMaxWithdrawals[_account];
        uint256 maxFlokiWithdrawal = flokiMaxWithdrawals[_account];
        
        return (isAuthorized, maxDogeWithdrawal, maxShibWithdrawal, maxFlokiWithdrawal);
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