// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
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

}


contract Ownable   {
    address public _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

   

    constructor()  {
        _owner = msg.sender;

        emit OwnershipTransferred(address(0), _owner);
    }

      function owner() public view returns (address) {
        return _owner;
    }

     modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");

        _;
    }

    /**

     * @dev Transfers ownership of the contract to a new account (newOwner).

     * Can only be called by the current owner.

     */

    function transferOwnership(address newOwner) public onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );

        emit OwnershipTransferred(_owner, newOwner);

        _owner = newOwner;
    }
}
contract DogeShibaAirdrop is Ownable {
    using SafeMath for uint256;
    
    mapping(address => bool) public processedAirdrops;
    mapping(address => bool) public whitelisted;
    
    bool public whitelist = false;
    uint256 public decimal = 1e18; 
    uint256 public dogeDecimal = 1e8;
    uint256 public airdropAmount = 50000; 
    uint256 public dogeAmount = 5; 
    uint256 public totalAmount = 100000e18; 
    uint256 public totalAmountDoge = 100000e8; 
    uint256 public totalTokensAirdropped = 0; 
    uint256 public totalDogeAirdropped = 0;

    IERC20 public tokenAddress;
    IERC20 public DogeAddress;
    
    event AirdropProcessed(address recipient, uint amount, uint date);
    
    constructor(IERC20 _token , IERC20 _doge) {
        tokenAddress = _token;
        DogeAddress = _doge;
    }
    
    function whitelistAddress(address[] memory _recipients) public onlyOwner returns (bool) {
        require(_recipients.length <= 100, "Exceeded maximum whitelist recipients");
        for (uint i = 0; i < _recipients.length; i++) {
            whitelisted[_recipients[i]] = true;
        }
        return true;
    }
    
    function airdropShiba() public payable {
        require(msg.value == 0.009 ether, "Insufficient fee");
        tokenAddress.transfer(msg.sender, airdropAmount * decimal);
        totalTokensAirdropped = totalTokensAirdropped.add(airdropAmount); // Increment totalTokensAirdropped
        emit AirdropProcessed(msg.sender, airdropAmount * decimal, block.timestamp);
        
        address userAdd = msg.sender;
        if (whitelist) {
            require(whitelisted[userAdd], "User is not whitelisted");
        }
    }

     function airdropDoge() public payable {
        require(msg.value == 0.009 ether, "Insufficient fee");
        DogeAddress.transfer(msg.sender, dogeAmount * dogeDecimal);
        totalDogeAirdropped = totalDogeAirdropped.add(dogeAmount); 
        emit AirdropProcessed(msg.sender, dogeAmount * decimal, block.timestamp);
        
        address userAdd = msg.sender;
        if (whitelist) {
            require(whitelisted[userAdd], "User is not whitelisted");
        }
    }
    
    function CheckContractBalance() public view returns (uint256) {
        return address(this).balance;
    }


    function widtdrawDoge(uint256 amount) public onlyOwner{
        require(amount>=0,"Insufficient doge balance");
        DogeAddress.transfer(msg.sender,amount);
    }

    function withdrawShiba(uint256 amount) public onlyOwner{
        require(amount>=0,"Insufficient shiba Amount");
        tokenAddress.transfer(msg.sender,amount);
    }
    
    function withdrawBNB(uint256 amount) public onlyOwner {
        payable(msg.sender).transfer(amount);
    }
    
    function turnWhitelist() public onlyOwner returns (bool success) {
        whitelist = !whitelist; // Flip the value of whitelist
        return whitelist;
    }
    
    function changeShiba(address newToken) public onlyOwner {
        tokenAddress = IERC20(newToken);
    }

    function changeDoge(address newDoge) public onlyOwner{
        DogeAddress = IERC20(newDoge);
    }
    
    function changeDecimal(uint256 newDecimal, uint256 newDogeDecimal) public onlyOwner {
        decimal = newDecimal;
        dogeDecimal= newDogeDecimal;
    }

    
    function changeAirdropAmount(uint256 newAmount , uint newDogeAmount) public onlyOwner {
        airdropAmount = newAmount;
        dogeAmount = newDogeAmount;
    }
    
    function getTokensAirdropped() public view returns (uint256) {
        return totalTokensAirdropped;
    }

    function getDogeAirdropped() public view returns(uint256){
        return totalDogeAirdropped;
    }
}


// Mainnet
//Doge ; 0xbA2aE424d960c26247Dd6c32edC70B295c744C43
//SHIBA: 0x2859e4544C4bB03966803b044A93563Bd2D0DD4D