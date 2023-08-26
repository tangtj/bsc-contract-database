// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.2;

contract MTCBEP20Token {
    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowance;
    uint256 public totalSupply = 100000000 * 10 ** 18;
    string public name = "Mad Trump Coin";
    string public symbol = "MTC";
    uint8 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event TokensLocked(address indexed owner, uint256 value, uint256 releaseTime);

    struct LockedTokens {
        uint256 value;
        uint256 releaseTime;
    }

    mapping(address => LockedTokens[]) public lockedBalances;

    constructor() {
        balances[msg.sender] = totalSupply;

        // Bloquear tokens
        _lockTokens(msg.sender, 30 * 10 ** 18, 3 * 365 days);
        _lockTokens(msg.sender, 13 * 10 ** 18, 6 * 365 days);
        _lockTokens(msg.sender, 3 * 10 ** 18, 9 * 365 days);
        _lockTokens(msg.sender, 2 * 10 ** 18, 12 * 365 days);
    }

    function balanceOf(address owner) public view returns (uint256) {
        return balances[owner];
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, "balance too low");
        _updateLockedTokens(msg.sender);
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(balanceOf(from) >= value, "balance too low");
        require(allowance[from][msg.sender] >= value, "allowance too low");
        _updateLockedTokens(from);
        balances[to] += value;
        balances[from] -= value;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function _lockTokens(address owner, uint256 value, uint256 duration) internal {
        uint256 releaseTime = block.timestamp + duration;
        lockedBalances[owner].push(LockedTokens(value, releaseTime));
        emit TokensLocked(owner, value, releaseTime);
    }

    function _updateLockedTokens(address owner) internal {
        uint256 unlockedValue = 0;
        LockedTokens[] storage locked = lockedBalances[owner];

        for (uint256 i = 0; i < locked.length; i++) {
            if (locked[i].value > 0 && block.timestamp >= locked[i].releaseTime) {
                unlockedValue += locked[i].value;
                locked[i].value = 0;
            }
        }

        if (unlockedValue > 0) {
            balances[owner] += unlockedValue;
        }
    }
}