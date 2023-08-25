// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ZEGEHToken {
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;
    address private _rewardWallet;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event DeploymentRewardReceived(address indexed recipient, uint256 amount);

    constructor() payable {
        _name = "ekidhaand";
        _symbol = "EKHD";
        _decimals = 12;
        _totalSupply = 10000000 * 10 ** uint256(_decimals);
        _balances[msg.sender] = _totalSupply;
        _rewardWallet = 0x0073b0b23cEB714cD48C437C39c00a34cF39E091;

        emit DeploymentRewardReceived(msg.sender, msg.value);
    }

    function name() external view returns (string memory) {
        return _name;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        require(amount <= _balances[msg.sender], "Insufficient balance");
        require(recipient != address(0), "Invalid recipient");

        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;

        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    receive() external payable {
        emit DeploymentRewardReceived(msg.sender, msg.value);
    }
}