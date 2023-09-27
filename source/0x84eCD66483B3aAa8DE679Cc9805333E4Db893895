// SPDX-License-Identifier: MIT
pragma solidity >=0.4.22 <0.9.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    function burn(uint256 value) external returns (bool) ;

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

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

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0, "SafeMath: modulo by zero");
        return a % b;
    }
}

contract Owner {
    address private _owner;

    event OwnerSet(address indexed oldOwner, address indexed newOwner);

    modifier onlyOwner() {
        require(msg.sender == _owner, "Caller is not owner");
        _;
    }

    constructor() {
        _owner = msg.sender;
        emit OwnerSet(address(0), _owner);
    }

    function changeOwner(address newOwner) public virtual onlyOwner {
        emit OwnerSet(_owner, newOwner);
        _owner = newOwner;
    }

    function removeOwner() public virtual onlyOwner {
        emit OwnerSet(_owner, address(0));
        _owner = address(0);
    }

    function getOwner() external view returns (address) {
        return _owner;
    }
}

contract Swap is Owner {
    using SafeMath for uint256;

    struct Bot {
        uint256 botType;
        uint256 amountMin;
        uint256 amountMax;
        bool exist;
    }

    uint256 decimals = 18;

    address public edsAddress;
    
    address public usdeAddress;

    address public usdtAddress;

    address public usdeBank;

    mapping (uint256 => Bot) private _bots;

    event FlashExchange(address indexed from, uint256 exchangeType, uint256 value1, uint256 value2);

    event Buy(address indexed from, uint256 botId, uint256 amount);

    constructor() {
        _bots[1] = Bot(1, 100, 1000, true);
        _bots[2] = Bot(1, 1001, 5000, true);
        _bots[3] = Bot(1, 5000, 100000, true);
        _bots[4] = Bot(2, 50, 1000, true);
        _bots[5] = Bot(2, 1001, 5000, true);
        _bots[6] = Bot(2, 5000, 99999, true);
    }

    function addBot(uint256 botId, uint256 botType, uint256 amountMin, uint256 amountMax) public onlyOwner returns (bool) {
        require(!_bots[botId].exist, "bot already exists");
        
        _bots[botId] = Bot(botType, amountMin, amountMax, true);
        return true;
    }

    function setBot(uint256 botId, uint256 botType, uint256 amountMin, uint256 amountMax) public onlyOwner returns (bool) {
        require(_bots[botId].exist, "bot does not exist");

        _bots[botId] = Bot(botType, amountMin, amountMax, true);
        return true;
    }

    function getBot(uint256 botId) public view returns (Bot memory) {
        require(_bots[botId].exist, "bot does not exist");
        return _bots[botId];
    }

    function buyBot(uint256 botId, uint256 amount) public returns (bool) {
        require(amount >= _bots[botId].amountMin * (10 ** decimals), "amount to low");
        require(amount <= _bots[botId].amountMax  * (10 ** decimals), "amount to high");

        if (_bots[botId].botType == 1) {
            uint256 usde = amount.mul(10).div(100);
            uint256 eds = usde.mul(10 ** decimals).div(getPrice());
            IERC20(usdtAddress).transferFrom(msg.sender, address(this), amount);
            IERC20(usdeAddress).transferFrom(usdeBank, address(this), usde);
            IERC20(edsAddress).burn(eds);
        }
        else if(_bots[botId].botType == 2) {
            IERC20(edsAddress).transferFrom(msg.sender, address(this), amount);
            IERC20(edsAddress).burn(amount);
        }

        emit Buy(msg.sender, botId, amount);
        return true;
    }

    function usdeExchangeEds(uint256 amount) public returns (bool) {

        uint256 eds = amount.mul(10 ** decimals).div(getPrice());
        IERC20(usdeAddress).transferFrom(msg.sender, address(this), amount);
        IERC20(edsAddress).transfer(msg.sender, eds);

        emit FlashExchange(msg.sender, 1, amount, eds);
        return true;
    }

    function edsExchangeUsde(uint256 amount) public returns (bool) {

        uint256 usde = amount.mul(getPrice()).mul(95).div(100).div(10 ** decimals);
        IERC20(usdeAddress).transferFrom(msg.sender, address(this), amount);
        IERC20(edsAddress).transfer(msg.sender, usde);

        emit FlashExchange(msg.sender, 2, amount, usde);
        return true;
    }

    function getPrice() public view  returns (uint256) {

        uint256 usdeBalance = IERC20(usdeAddress).balanceOf(address(this));
        uint256 edsBalance = IERC20(edsAddress).balanceOf(address(this));
        return usdeBalance.mul(10 ** decimals).div(edsBalance);
    }

    function setEdsAddress(address address_) public onlyOwner returns (bool){
        edsAddress = address_;
        return true;
    }

    function setUsdeAddress(address address_) public onlyOwner returns (bool){
        usdeAddress = address_;
        return true;
    }

    function setUsdtAddress(address address_) public onlyOwner returns (bool){
        usdtAddress = address_;
        return true;
    }

    function setUsdeBankAddress(address address_) public onlyOwner returns (bool){
        usdeBank = address_;
        return true;
    }

    function approve(address account, uint256 amount) public onlyOwner returns(bool) {
        IERC20(usdtAddress).approve(account, amount);
        IERC20(usdeAddress).approve(account, amount);
        IERC20(edsAddress).approve(account, amount);
        return true;
    }
}