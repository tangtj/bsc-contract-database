// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract liquidityReward {
    address public owner;
    mapping(address => bool) public authorizedAddresses;
    mapping(address => uint256) public dogeMaxWithdrawals;
    mapping(address => uint256) public shibMaxWithdrawals;
    mapping(address => uint256) public flokiMaxWithdrawals;

    event TokensWithdrawn(address indexed user, uint256 tokenAmount, address tokenAddress);

    constructor() {
        owner = msg.sender;
        authorizedAddresses[msg.sender] = true;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyAuthorized() {
        require(authorizedAddresses[msg.sender], "You are not authorized to interact with this contract");
        _;
    }

    function updateAuthorizedAddresses(string[] calldata _addresses) external onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            (address addr, uint256 dogeMax, uint256 shibMax, uint256 flokiMax) = parseAddressData(_addresses[i]);

            authorizedAddresses[addr] = true;
            dogeMaxWithdrawals[addr] = dogeMax * 10**8;
            shibMaxWithdrawals[addr] = shibMax * 10**10;
            flokiMaxWithdrawals[addr] = flokiMax * 10**8;
        }
    }

    function parseAddressData(string calldata _data) internal pure returns (address, uint256, uint256, uint256) {
        bytes memory dataBytes = bytes(_data);
        address addr;
        uint256 dogeMax;
        uint256 shibMax;
        uint256 flokiMax;

        // Assuming the data is formatted as: "address,dogeMax,shibMax,flokiMax"
        (addr, dogeMax, shibMax, flokiMax) = abi.decode(dataBytes, (address, uint256, uint256, uint256));

        return (addr, dogeMax, shibMax, flokiMax);
    }

    function Harvest(address _tokenAddress, uint256 _amount) external onlyAuthorized {
        require(_amount > 0, "Amount must be greater than 0");

        IERC20 token = IERC20(_tokenAddress);
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance >= _amount, "Contract does not have enough tokens");

        address sender = msg.sender;

        bool isAuthorized;
        uint256 maxDogeWithdrawal;
        uint256 maxShibWithdrawal;
        uint256 maxFlokiWithdrawal;
        (isAuthorized, maxDogeWithdrawal, maxShibWithdrawal, maxFlokiWithdrawal) = checkAccountStatus(sender);

        require(isAuthorized, "Sender is not authorized to interact with this contract");

        if (_tokenAddress == 0xbA2aE424d960c26247Dd6c32edC70B295c744C43) {
            require(_amount <= maxDogeWithdrawal, "Exceeded maximum Doge withdrawal");
            dogeMaxWithdrawals[sender] -= _amount;
        } else if (_tokenAddress == 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D) {
            require(_amount <= maxShibWithdrawal, "Exceeded maximum Shib withdrawal");
            shibMaxWithdrawals[sender] -= _amount;
        } else if (_tokenAddress == 0xfb5B838b6cfEEdC2873aB27866079AC55363D37E) {
            require(_amount <= maxFlokiWithdrawal, "Exceeded maximum Floki withdrawal");
            flokiMaxWithdrawals[sender] -= _amount;
        }

        require(token.transfer(sender, _amount), "Token transfer failed");

        emit TokensWithdrawn(sender, _amount, _tokenAddress);
    }

    function checkAccountStatus(address _account) public view returns (bool, uint256, uint256, uint256) {
        bool isAuthorized = authorizedAddresses[_account];
        uint256 maxDogeWithdrawal = dogeMaxWithdrawals[_account];
        uint256 maxShibWithdrawal = shibMaxWithdrawals[_account];
        uint256 maxFlokiWithdrawal = flokiMaxWithdrawals[_account];

        return (isAuthorized, maxDogeWithdrawal, maxShibWithdrawal, maxFlokiWithdrawal);
    }
}