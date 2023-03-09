pragma solidity ^0.5.1;

library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
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
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract Ownable{
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () internal {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), msg.sender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(isOwner(), "Ownable: caller is not the owner");
        _;
    }

    function isOwner() public view returns (bool) {
        return msg.sender == _owner;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract ERC20 is Ownable {
    using SafeMath for uint256;

    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    string public constant name = "unidexbot.com";
    string public constant symbol = "bUNDB";
    uint public constant decimals = 18;
    uint constant total = 2000;
    uint256 private _totalSupply;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() public {
        _mint(msg.sender, total * 10**decimals);
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    function returnERC20 (address tokenAddress) public onlyOwner {
        ERC20 token = ERC20(tokenAddress); 
        token.transfer(msg.sender, token.balanceOf(address(this)));
    }

}

contract Crowdsale {
    address payable owner;
    address me = address(this);
    uint sat = 1e18;

    // *** Config ***
    uint startIco = 1614016800;
    uint stopIco = startIco + 72 hours;

    uint percentSell = 50;
    uint manualSaleAmount = 0 * sat;
    
    uint countBy1BNB = 25000; // 1BNB = 0.25 bUNDB
    uint maxTokensToOnceHand = 50 * sat; // 50 tokens to hand
    
    // --- Config ---
    uint priceDecimals = 1e5; // realPrice = Price / priceDecimals
    ERC20 token = new ERC20();

    constructor() public {
        owner = msg.sender;
        token.transferOwnership(owner);
        token.transfer(owner, token.totalSupply() / 100 * (100 - percentSell) + manualSaleAmount);
    }

    function() external payable {
        require(startIco < now && now < stopIco, "Period error");
        uint amount = msg.value * countBy1BNB / priceDecimals;
        require(token.balanceOf(msg.sender) + amount <= maxTokensToOnceHand, "The purchase limit of 50 tokens has been exceeded");
        require(amount <= token.balanceOf(address(this)), "Infucient token balance in ICO");
        token.transfer(msg.sender, amount);
    }


    // OWNER ONLY
    function manualGetETH() public payable {
        require(msg.sender == owner, "You is not owner");
        owner.transfer(address(this).balance);
    }

    function getLeftTokens() public {
        require(msg.sender == owner, "You is not owner");
        token.transfer(owner, token.balanceOf(address(this)));
    }
    //--- OWNER ONLY

    // Utils
    function getStartICO() public view returns (uint) {
        return (startIco - now) / 60;
    }
    function getOwner() public view returns (address) {
        return owner;
    }

    function getStopIco() public view returns(uint){
        return (stopIco - now) / 60;
    }
    function tokenAddress() public view returns (address){
        return address(token);
    }
    function IcoDeposit() public view returns(uint){
        return token.balanceOf(address(this)) / sat;
    }
    function myBalancex10() public view returns(uint){
        return token.balanceOf(msg.sender) / 1e17;
    }
}