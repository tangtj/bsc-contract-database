// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.0;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, 'SafeMath: addition overflow');
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, 'SafeMath: subtraction overflow');
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, 'SafeMath: multiplication overflow');
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, 'SafeMath: division by zero');
        uint256 c = a / b;
        return c;
    }
}

interface ERC720 {
    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function balanceOf(
        address tokenOwner
    ) external view returns (uint256 balance);

    function allowance(
        address _owner,
        address spender
    ) external view returns (uint256);

    function decimals() external view returns (uint8);
}

contract Wallet {
    using SafeMath for uint256;

    ERC720 public token;
    address public managerAdress;
    address public ownerAddress;
    bool public isSelfAllow = false;
    mapping(address => uint256) public holderBalance;
    mapping(string => bool) public withdrawRequests;

    event Deposit(address _address, uint256 _amount, uint256 _time);
    event Withdraw(address _address, uint256 _amount, string id, uint256 _time);

    constructor(address _managerAdress, address _token) {
        managerAdress = _managerAdress;
        ownerAddress = msg.sender;
        token = ERC720(_token);
    }

    modifier onlyOwner() {
        require(msg.sender == ownerAddress, 'Only owner');
        _;
    }

    modifier onlyManager() {
        require(msg.sender == managerAdress, 'Only Manager');
        _;
    }

    function deposit(uint256 amount) public payable {
        uint256 balance = token.balanceOf(msg.sender);
        uint256 allowance = token.allowance(msg.sender, address(this));

        require(balance >= amount, 'Error: Insufficient Balance');
        require(allowance >= amount, 'Error: Allowance less than spending');

        token.transferFrom(msg.sender, address(this), amount);
        holderBalance[msg.sender] = holderBalance[msg.sender].add(amount);
        emit Deposit(msg.sender, amount, block.timestamp);
    }

    function withdrawMultiple(
        address[] calldata _address,
        uint256[] calldata _amount,
        string[] calldata id
    ) public payable onlyManager {
        for (uint256 i = 0; i < _address.length; i++) {
            withdraw(_address[i], _amount[i], id[i]);
        }
    }

    function withdraw(
        address _address,
        uint256 _amount,
        string memory id
    ) public payable onlyManager {
        require(
            withdrawRequests[id] == false,
            'Withrdraw request already approved'
        );
        token.transfer(_address, _amount);
        emit Withdraw(_address, _amount, id, block.timestamp);
    }

    function withdrawSelf(
        address _address,
        uint256 _amount,
        string memory id
    ) public payable onlyManager {
        require(isSelfAllow, 'Self withdraw not allowed');
        _withdraw(_address, _amount, id);
    }

    function _withdraw(
        address _address,
        uint256 _amount,
        string memory id
    ) internal {
        require(
            holderBalance[_address] >= _amount,
            'Error: Insufficient Balance'
        );
        token.transfer(_address, _amount);
        holderBalance[_address] = holderBalance[_address].sub(_amount);
        emit Withdraw(_address, _amount, id, block.timestamp);
    }

    function handleTransaction(
        address _address,
        uint256 _amount,
        string memory id,
        uint256 operation
    ) public onlyManager {
        if (operation == 0) {
            holderBalance[_address] = holderBalance[_address].add(_amount);
            emit Deposit(_address, _amount, block.timestamp);
        }
        if (operation == 1) {
            holderBalance[_address] = holderBalance[_address].sub(_amount);
            emit Withdraw(_address, _amount, id, block.timestamp);
        }
    }

    function ChangeSelfAllowed(bool _isSelfAllow) public payable onlyManager {
        isSelfAllow = _isSelfAllow;
    }

    function changeManager(address _address) public onlyOwner {
        managerAdress = _address;
    }

    function changeOwner(address _address) public onlyOwner {
        ownerAddress = _address;
    }
}