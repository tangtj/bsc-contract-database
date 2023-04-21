pragma solidity ^0.5.9;

interface IBFIcoin {
    function balanceOf(address _owner) view external  returns (uint256 balance);
    function transfer(address _to, uint256 _value) external  returns (bool success);
    function transferFrom(address _from, address _to, uint256 _value) external  returns (bool success);
    function approve(address _spender, uint256 _value) external returns (bool success);
    function allowance(address _owner, address _spender) view external  returns (uint256 remaining);
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
}

interface AggregatorV3Interface {

  function decimals() external view returns (uint8);
  function description() external view returns (string memory);
  function version() external view returns (uint256);

  
  function getRoundData(uint80 _roundId)
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

contract BFIBuying{
    
    using SafeMath for uint256;
    AggregatorV3Interface internal priceFeed;
    IBFIcoin token;
    
    uint256 public precision = 100000 ;
    address payable public owner;

    
    bool public buying = true;

    modifier onlyOwner() {
        require(msg.sender==owner);
        _;
    }
    
    modifier toBuy(){
        require(buying == true,"Token buying paused by owner.");
        _;
    }
    
    constructor(address payable _owner, IBFIcoin _token) public{
        owner = _owner;
        token = _token; 
        priceFeed = AggregatorV3Interface(0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE);
    }
    
    function changeOwner(address payable _newOwner) public onlyOwner returns(bool) {
        owner = _newOwner;
        return true;
    }
    
    function _setPrecision(uint256 _precision) external onlyOwner {
        precision = _precision;
    }
    
    function _setBuying(bool _value) external onlyOwner {
        buying = _value;
    }
    
    function () payable external {
        owner.transfer(msg.value);
    }
    
    function getContractBalance() public view returns(uint256) {
        return address(this).balance;
    }
    
    
    function getLatestPrice() public view returns (uint256) {
        (,int price,,,) = priceFeed.latestRoundData();
        
        return uint256(price).div(1e8);
    }
    
    function buyToken(uint256 _amount) public payable toBuy returns(bool){
        require(_amount > 0 , "Amount can not be zero.");
        uint256 price = precision.div(getLatestPrice());
        require(_amount.mul(price) >= msg.value.mul(precision).div(1e18) && _amount <= token.balanceOf(owner), "Incorrect amount.");
        
        token.transferFrom(owner, msg.sender, _amount*1e8);

        return true;
    }
    
    function transferFunds(uint256 _amount) external onlyOwner returns(bool){
        require(_amount <= getContractBalance(),"not enough balance in the contract.");
        owner.transfer(_amount);       
        return true;
        
    }
    
     
}

library SafeMath {

  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a * b;
    assert(a == 0 || c / a == b);
    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a / b;
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

}