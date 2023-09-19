// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address to, uint256 value) external returns (bool);
    function burn(uint256 _amount,address user) external returns(bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes calldata) {
        this; 
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _setOwner(_msgSender());
    }

    function Owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(Owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract TokenSwap is Ownable {
    uint256 public sponsorPercentage = 2;
    uint256 public developerPercentage = 5;
     uint256 private key;
    address public developer;
    address public owner;
    address public tokenAddress;  // Address of your token contract
    address public usdtAddress;   // Address of the USDT contract
    uint256 public exchangeRate;   // Exchange rate between your token and USDT
 


    
    constructor(address _tokenAddress, address _usdtAddress, uint256 Exchangerate) {
        owner = msg.sender;
        tokenAddress = _tokenAddress;
        usdtAddress = _usdtAddress;
        exchangeRate = Exchangerate;
    }
 
    function setExchangeRate(uint256 newRate) external onlyOwner {
        exchangeRate = newRate;
    }
    function setusdtAddress(address _address)external onlyOwner{
        usdtAddress = _address;
    }
       function setTokenAddress(address _address)external onlyOwner{
        tokenAddress = _address;
    }
        function setDeveloperAddress(address developerAddress)external  onlyOwner{
        developer = developerAddress;
    }
    
    function swapTokens(uint256 _amount,address sponsor, uint256 _key) external {
        require(getKey() == _key,"You not call directly");
        require(_amount > 0, "Token amount must be greater than 0");
        IERC20 token = IERC20(tokenAddress);
        IERC20 usdt = IERC20(usdtAddress);
        require(token.transferFrom(_msgSender(), address(this), _amount), "Token transfer failed");
        uint256 usdtAmT = _amount / exchangeRate;
        uint256 sponsorReward = (usdtAmT * sponsorPercentage) / 100;
        uint256 developerReward = (usdtAmT * developerPercentage) / 100;
        uint256 usdtAmount = usdtAmT - sponsorReward - developerReward;

        (token).burn(_amount,address(this));

       
          require(usdt.transfer(_msgSender(), usdtAmount), "USDT transfer failed");
          require(usdt.transfer(developer,developerReward), "USDT transfer failed");
          require(usdt.transfer(sponsor, sponsorReward), "USDT transfer failed");
    }
    
    function setKey(uint256 _key)external onlyOwner{
    key = _key;
     }
     function getKey()private view returns(uint256) {
        return key;
    }
    
     function RescueAmountMHTtoken(address reciver, uint256 amount)external onlyOwner{
        IERC20 token = IERC20(tokenAddress);
        require(amount <= token.balanceOf(address(this)),"please enter valid amount");
        require(token.transfer(reciver, amount),"transaction failed");
    }
       function _RescueAmountusdt(address reciver, uint256 amount)external onlyOwner{
        IERC20 token = IERC20(usdtAddress);
        require(amount <= token.balanceOf(address(this)),"please enter valid amount");
        require(token.transfer(reciver, amount),"transaction failed");
    }


}