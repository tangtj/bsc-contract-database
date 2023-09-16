// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;
        return c;
    }
}

contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), _owner);
    }

    modifier onlyOwner() {
        require(isOwner(), "Ownable: caller is not the owner");
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    function isOwner() public view returns (bool) {
        return msg.sender == _owner;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
}

contract ReentrancyGuard {
    uint256 private _guardCounter;

    constructor() {
        _guardCounter = 1;
    }

    modifier nonReentrant() {
        _guardCounter += 1;
        uint256 localCounter = _guardCounter;
        _;
        require(localCounter == _guardCounter, "ReentrancyGuard: reentrant call");
    }
}

contract Test is Ownable, ReentrancyGuard {
    using SafeMath for uint256;

    string public name = "test";
    string public symbol = "test";
    uint256 public totalSupply;
    uint8 public decimals = 18;

    uint256 public outTaxPercent = 3;
    address public marketingWallet;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        totalSupply = 40000000000000000000000000000000;
        balanceOf[msg.sender] = 40000000000000000000000000000000;
        marketingWallet = msg.sender;
    }

    function transfer(address _to, uint256 _value) external nonReentrant returns (bool) {
        require(_to != address(0), "Invalid recipient");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");

        uint256 outTaxAmount = _value.mul(outTaxPercent).div(100);
        uint256 netTransferAmount = _value.sub(outTaxAmount);

        balanceOf[msg.sender] = balanceOf[msg.sender].sub(_value);
        balanceOf[_to] = balanceOf[_to].add(netTransferAmount);
        balanceOf[marketingWallet] = balanceOf[marketingWallet].add(outTaxAmount);

        emit Transfer(msg.sender, _to, netTransferAmount);
        emit Transfer(msg.sender, marketingWallet, outTaxAmount);
        return true;
    }

    function approve(address _spender, uint256 _value) external nonReentrant returns (bool) {
        require(_spender != address(0), "Invalid spender address");

        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) external nonReentrant returns (bool) {
        require(_from != address(0), "Invalid sender address");
        require(_to != address(0), "Invalid recipient address");
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Insufficient allowance");

        uint256 outTaxAmount = _value.mul(outTaxPercent).div(100);
        uint256 netTransferAmount = _value.sub(outTaxAmount);

        balanceOf[_from] = balanceOf[_from].sub(_value);
        balanceOf[_to] = balanceOf[_to].add(netTransferAmount);
        balanceOf[marketingWallet] = balanceOf[marketingWallet].add(outTaxAmount);

        allowance[_from][msg.sender] = allowance[_from][msg.sender].sub(_value);

        emit Transfer(_from, _to, netTransferAmount);
        emit Transfer(_from, marketingWallet, outTaxAmount);
        return true;
    }

    function setMarketingWallet(address _marketingWallet) external onlyOwner {
        require(_marketingWallet != address(0), "Invalid wallet address");
        marketingWallet = _marketingWallet;
    }

    function setTaxPercentage(uint256 _taxPercentage) external onlyOwner {
        require(_taxPercentage <= 100, "Tax percentage must be less than or equal to 100");
        outTaxPercent = _taxPercentage;
    }


}