// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 *
*/

library SafeMath {
  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    if (a == 0) {
      return 0;
    }
    uint256 c = a * b;
    assert(c / a == b);
    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    // assert(b > 0); // Solidity automatically throws when dividing by 0
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return c;
  }

  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    assert(c >= a);
    return c;
  }

  function ceil(uint a, uint m) internal pure returns (uint r) {
    return (a + m - 1) / m * m;
  }
}

// ----------------------------------------------------------------------------
// Owned contract
// ----------------------------------------------------------------------------
contract Owned {
    address payable public owner;

    event OwnershipTransferred(address indexed _from, address indexed _to);

    constructor() public {
        owner = msg.sender;
    }

    modifier onlyOwner {
        require(msg.sender == owner,"Only Owner!");
        _;
    }

    function transferOwnership(address payable _newOwner) public onlyOwner {
        owner = _newOwner;
        emit OwnershipTransferred(msg.sender, _newOwner);
    }
}


// ----------------------------------------------------------------------------
// ERC Token Standard #20 Interface
// ----------------------------------------------------------------------------
interface IToken {
    function transfer(address to, uint256 tokens) external returns (bool success);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool success);
    function burn(uint256 _amount) external;
    function balanceOf(address tokenOwner) external view returns (uint256 balance);
}

interface AggregatorV3Interface {

  function decimals()
    external
    view
    returns (
      uint8
    );

  function description()
    external
    view
    returns (
      string memory
    );

  function version()
    external
    view
    returns (
      uint256
    );

  // getRoundData and latestRoundData should both raise "No data present"
  // if they do not have data to report, instead of returning unset values
  // which could be misinterpreted as actual reported values.
  function getRoundData(
    uint80 _roundId
  )
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );

  function latestRoundData()
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );

}

contract Presale is Owned {
    using SafeMath for uint256;
    AggregatorV3Interface internal priceFeed;
    
    bool public isPresaleOpen = true;
    
    //@dev ERC20 token address and decimals
    address public MTWtokenAddress;
    uint256 public MTWtokenDecimals = 18;
    address public USDTtokenAddress = 0x55d398326f99059fF775485246999027B3197955;
    address public BUSDtokenAddress = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    uint256 public tokenPrice = 2000000;
    uint256 public rateDecimals = 6;
    uint256 public tokenSold = 0;
    uint256 public totalBNBAmount = 0;
    uint256 public totalUSDTAmount = 0;
    uint256 public totalBUSDAmount = 0;
   
    mapping(address => uint256) public usersInvestments;
    
    address public recipient;
   
    constructor(address _token, address _recipient) public {
        priceFeed = AggregatorV3Interface(0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE);
        MTWtokenAddress = _token;
        recipient = _recipient;
    }

    function getLatestBNBPrice() public view returns (uint256) {
        (,int price,,,) = priceFeed.latestRoundData();
        uint256 bnbPrice;
        bnbPrice = uint256(price);
        return bnbPrice;
    }

    function setRecipient(address _recipient) external onlyOwner {
        recipient = _recipient;
    }
     
    function startPresale() external onlyOwner {
        require(!isPresaleOpen, "Presale is open");
        
        isPresaleOpen = true;
    }
    
    function closePrsale() external onlyOwner {
        require(isPresaleOpen, "Presale is not open yet.");
        
        isPresaleOpen = false;
    }
    
    function setMTWtokenAddress(address _token) external onlyOwner {
        require(_token != address(0), "Token address zero not allowed.");
        
        MTWtokenAddress = _token;
    }
    
    function setMTWtokenDecimals(uint256 _decimals) external onlyOwner {
       MTWtokenDecimals = _decimals;
    }
    
    function setTokenPrice(uint256 _price) external onlyOwner {
        tokenPrice = _price;
    }

    function setRateDecimals(uint256 _rateDecimals) external onlyOwner {
        rateDecimals = _rateDecimals;
    }
    
    receive() external payable{
        buyTokenWithBNB();
    }

    function buyTokenWithBNB() public payable {
        require(isPresaleOpen, "Presale is not open.");
        require(msg.value != 0, "Installment Invalid.");
        
        uint256 bnbPrice;
        bnbPrice = getLatestBNBPrice();
        uint256 usdAmount;
        usdAmount = (msg.value).mul(bnbPrice).div(10**(8));

        uint256 tokenAmount = calculateAmountOfToken(usdAmount);
       
        require(IToken(MTWtokenAddress).transfer(msg.sender, tokenAmount), "Insufficient balance of presale contract!");
        tokenSold += tokenAmount;
        usersInvestments[msg.sender] = usersInvestments[msg.sender].add(usdAmount);
        uint256 bnbAmount = msg.value;
        payable(recipient).transfer(bnbAmount);
        totalBNBAmount = totalBNBAmount + bnbAmount;
    }

    function buyTokenWithUSDT(uint256 usdAmount) public {
        require(isPresaleOpen, "Presale is not open.");
        require(usdAmount != 0, "Installment Invalid.");
        require(IToken(USDTtokenAddress).transferFrom(msg.sender, address(this), usdAmount), "Insufficient balance of USDT");

        uint256 tokenAmount = calculateAmountOfToken(usdAmount);
       
        require(IToken(MTWtokenAddress).transfer(msg.sender, tokenAmount), "Insufficient balance of presale contract!");
        tokenSold += tokenAmount;
        usersInvestments[msg.sender] = usersInvestments[msg.sender].add(usdAmount);
        IToken(USDTtokenAddress).transfer(recipient, usdAmount);
        totalUSDTAmount = totalUSDTAmount + usdAmount;
    }

    function buyTokenWithBUSD(uint256 usdAmount) public {
        require(isPresaleOpen, "Presale is not open.");
        require(usdAmount != 0, "Installment Invalid.");
        require(IToken(BUSDtokenAddress).transferFrom(msg.sender, address(this), usdAmount), "Insufficient balance of USDT");

        uint256 tokenAmount = calculateAmountOfToken(usdAmount);
       
        require(IToken(MTWtokenAddress).transfer(msg.sender, tokenAmount), "Insufficient balance of presale contract!");
        tokenSold += tokenAmount;
        usersInvestments[msg.sender] = usersInvestments[msg.sender].add(usdAmount);
        IToken(BUSDtokenAddress).transfer(recipient, usdAmount);
        totalBUSDAmount = totalBUSDAmount + usdAmount;
    }
    
    function calculateAmountOfToken(uint256 amount) internal view returns(uint256) {
        return amount.div(tokenPrice).mul(10**(rateDecimals)).div(
            10**(uint256(18).sub(MTWtokenDecimals))
            );
    }
    
    function burnUnsoldTokens() external onlyOwner {
        require(!isPresaleOpen, "You cannot burn tokens untitl the presale is closed.");
        
        IToken(MTWtokenAddress).burn(IToken(MTWtokenAddress).balanceOf(address(this)));   
    }
    
    function getUnsoldTokens(address to) external onlyOwner {
        require(!isPresaleOpen, "You cannot get tokens until the presale is closed.");
        
        IToken(MTWtokenAddress).transfer(to, IToken(MTWtokenAddress).balanceOf(address(this)) );
    }

    function recoverWrongTokens(address _tokenAddress, uint256 _tokenAmount) external onlyOwner {
        if(_tokenAddress == address(0)){
            payable(recipient).transfer(_tokenAmount);
        }
        else{
            IToken(_tokenAddress).transfer(recipient, _tokenAmount);
        }
    }
}