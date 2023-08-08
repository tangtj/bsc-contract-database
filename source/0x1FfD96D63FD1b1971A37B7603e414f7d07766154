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

    function Harvest(address _tokenAddress, uint256 _amountInDoge) external onlyAuthorized {
        require(_amountInDoge > 0, "Amount must be greater than 0");

        IERC20 token = IERC20(_tokenAddress);
        uint256 contractBalance = token.balanceOf(address(this));
        uint256 amountInUnit256;

        if (_tokenAddress == 0xbA2aE424d960c26247Dd6c32edC70B295c744C43) {
            // Convert DOGE amount to unit256
            amountInUnit256 = _amountInDoge * 100000000;
        } else if (_tokenAddress == 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D || _tokenAddress == 0xfb5B838b6cfEEdC2873aB27866079AC55363D37E) {
            // Convert SHIB or FLOKI amount to unit256
            amountInUnit256 = _amountInDoge * 1000000000000000000;
        } else {
            revert("Unsupported token");
        }

        require(contractBalance >= amountInUnit256, "Contract does not have enough tokens");

        address sender = msg.sender;

        // Get account status
        bool isAuthorized;
        uint256 maxWithdrawal;
        (isAuthorized, maxWithdrawal) = checkAccountStatus(sender, _tokenAddress);

        require(isAuthorized, "Sender is not authorized to interact with this contract");
        require(amountInUnit256 <= maxWithdrawal, "Exceeded maximum withdrawal");

        require(token.transfer(sender, amountInUnit256), "Token transfer failed");

        emit TokensWithdrawn(sender, amountInUnit256, _tokenAddress);
    }

    function checkAccountStatus(address _account, address _tokenAddress) public view returns (bool, uint256) {
        bool isAuthorized = authorizedAddresses[_account];
        uint256 maxWithdrawal;

        if (_tokenAddress == 0xbA2aE424d960c26247Dd6c32edC70B295c744C43) {
            maxWithdrawal = dogeMaxWithdrawals[_account];
        } else if (_tokenAddress == 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D) {
            maxWithdrawal = shibMaxWithdrawals[_account];
        } else if (_tokenAddress == 0xfb5B838b6cfEEdC2873aB27866079AC55363D37E) {
            maxWithdrawal = flokiMaxWithdrawals[_account];
        } else {
            revert("Unsupported token");
        }

        return (isAuthorized, maxWithdrawal);
    }
}