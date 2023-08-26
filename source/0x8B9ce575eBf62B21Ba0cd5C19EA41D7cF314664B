// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
// import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
interface AggregatorV3Interface {

  function decimals() external view returns (uint8);
  function description() external view returns (string memory);
  function version() external view returns (uint256);

  // getRoundData and latestRoundData should both raise "No data present"
  // if they do not have data to report, instead of returning unset values
  // which could be misinterpreted as actual reported values.
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
interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external;
}
contract Crickto  {
    address public tokenAddress;
    IERC20 token;
     IERC20 busdToken= IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    address public owner;
    uint256 public tokenPerUSD=1;
    string private referalCode;
    uint256 public referalPercent;
     AggregatorV3Interface internal dataFeed;

    /**
     * Network: Binance
     * Aggregator: BNB/USD
     * Address: 0xC5A35FC58EFDC4B88DDCA51AcACd2E8F593504bE

     */

    constructor(address _token, string memory _referalCode , uint256 _referalPercent, uint256 _tokenPerUsd ){
        tokenAddress=_token;
       token=IERC20(_token);
       referalCode=_referalCode;
       referalPercent=_referalPercent;
       tokenPerUSD=_tokenPerUsd;
       owner=msg.sender;
        dataFeed = AggregatorV3Interface(
           0x0567F2323251f0Aab15c8dFb1967E4e8A7D42aeE
        );
          // 0x1A26d803C2e796601794f8C5609549643832702C
    }
     modifier onlyOwner() {  
              require(msg.sender == owner,"you are not owner");  
                    _;    }

    function buyWithBNB(address receiver, uint256 amount, string memory referal) public payable  returns(bool){
         uint price=uint(getLatestData());
         uint256 bnbAmount=uint256((amount*10**8)/(price*tokenPerUSD));
          if (keccak256(abi.encodePacked(referal)) == keccak256(abi.encodePacked(referalCode))) {
            amount+=amount*(referalPercent)/100;
        }
        require(token.allowance(owner,address(this))>amount, "amount is invalid");
        require(msg.value>=bnbAmount, "Insufficient amount");
        bool transfer=token.transferFrom(owner, receiver, amount);
        return transfer;
    }
     function getLatestData() public  view returns (int) {
       // prettier-ignore
        (
            /* uint80 roundID */,
            int answer ,
            // /*uint startedAt*/,
            /*uint timeStamp*/,
            /*uint80 answeredInRound*/,
        ) = dataFeed.latestRoundData();
        return answer;
    }
    function ChangeReferalCode(string memory newCode) public onlyOwner{
        referalCode=newCode;
    }
    function ChangeTokenAmountPerUsd(uint256 _amountPerUsd) public onlyOwner {
        tokenPerUSD=_amountPerUsd;
    }
    function buyWithBUSD(address receiver, uint256 amount, string memory referal) public  returns (bool){
        require(busdToken.allowance(receiver, address(this))>(amount/tokenPerUSD),"allow BUSD to this contract");
        bool transferBUSD=busdToken.transferFrom(receiver, address(this), amount/tokenPerUSD);
        require(transferBUSD,"busd transfer failed");
          if (keccak256(abi.encodePacked(referal)) == keccak256(abi.encodePacked(referalCode))) {
            amount+=amount*(referalPercent)/100;
        }
        require(token.allowance(owner,address(this))>amount, "amount is invalid");
        bool transfer=token.transferFrom(owner, receiver, amount);
        return transfer;
    }
     function withdrawTokens(address _tokenAddress) external {
        require(msg.sender == owner, "Only the owner can withdraw");
        IERC20 otherToken = IERC20(_tokenAddress);
        uint256 balance = otherToken.balanceOf(address(this));
        require(balance > 0, "No balance to withdraw");
        otherToken.transfer(msg.sender, balance);
    }
    function withdrawEth() external {
        require(msg.sender == owner, "Only the owner can withdraw");
        payable(msg.sender).transfer(address(this).balance);
    }
    function changeReferalPercent(uint256 percent) public  {
        require(msg.sender==owner, "You are not the owner of the contract");
        referalPercent=percent;
    }
    
}