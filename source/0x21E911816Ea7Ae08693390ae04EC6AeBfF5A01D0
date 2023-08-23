// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract Bridge {
    IERC20 public token;
    address public owner;
    address public nodeWallet;
    uint256 public currentId = 1;
    uint256 public bridgedId = 1;
    bool private locked = false; // Reentry koruması için eklediğimiz kilitleme durumu

    modifier noReentry() {
        require(!locked, "Reentry attack detected");
        locked = true;
        _;
        locked = false;
    }

    struct Deposit {
        address user;
        uint256 amount;
        uint256 time;
        bool processed;
    }

    struct Withdrawal {
        address user;
        uint256 amount;
    }

    mapping(uint256 => Deposit) public deposits;
    mapping(uint256 => Withdrawal) public withdrawals;

    event Deposited(address indexed user, uint256 amount, uint256 id);
    event Withdrawn(address indexed user, uint256 amount, uint256 id);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    constructor(address _token, address _nodeWallet) {
        token = IERC20(_token);
        owner = msg.sender;
        nodeWallet = _nodeWallet;
    }

    function deposit(uint256 amount) external noReentry {
        require(token.transferFrom(msg.sender, address(this), amount), "Transfer failed");
        deposits[currentId] = Deposit(msg.sender, amount, block.timestamp, false);
        emit Deposited(msg.sender, amount, currentId);
        currentId++;
    }

    function processDeposit(uint256 id) external noReentry {
        require(msg.sender == nodeWallet, "Can only be triggered by node wallet");
        require(!deposits[id].processed, "Already processed");
        deposits[id].processed = true;
    }

    function withdraw(address _user, uint256 _amount) external noReentry {
        require(msg.sender == nodeWallet, "Can only be triggered by node wallet");
        token.transfer(_user, _amount);
        withdrawals[bridgedId] = Withdrawal(_user, _amount);
        bridgedId++;
    }

    function emergencyWithdraw() external onlyOwner noReentry {
        uint256 balance = token.balanceOf(address(this));
        token.transfer(owner, balance);
    }
    function emergencyWith(IERC20 _token) external onlyOwner noReentry {
        uint256 balance = _token.balanceOf(address(this));
        _token.transfer(owner, balance);
    }
}