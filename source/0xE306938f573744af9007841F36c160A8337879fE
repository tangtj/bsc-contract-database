// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;


contract Context {

    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }
    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}


interface IBEP20 {
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
    function add(uint256 a, uint256 b) internal pure returns (uint256) 
    
    {
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

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor ()  {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}



contract PepeArabPresale is Ownable,ReentrancyGuard {
    using SafeMath for uint256;

    IBEP20 public BUSD;
    IBEP20 public USDT;
    IBEP20 public USDC;
    IBEP20 public Presale_Token;

    uint256 public TokenpricePerBUSD = 400000000000000000000000; //400000 token
    uint256 public TokenpricePerUSDT = 400000000000000000000000; //400000 token
    uint256 public TokenpricePerUSDC = 400000000000000000000000; //400000 token
    uint256 public TokenpricePerBNB  = 100000000000000000000000000; //100.000.000 Token
    uint256 public Token_Sold;
    uint256 public referalpercent=10;
    address public deadAddress=0x000000000000000000000000000000000000dEaD;
    mapping (address=> uint256) public claim_amount;
    bool public presaleStatus;
    bool public ClaimStatus;


    mapping(address => uint256) public deposits;
    event Deposited(address indexed user, uint256 amount);
    event Recovered(address token, uint256 amount);
   

    constructor(IBEP20 _BUSD,IBEP20 _Presale_Token,IBEP20 _USDT,IBEP20 _USDC)  {
        BUSD = _BUSD; 
        USDT=_USDT;
        USDC=_USDC;
        Presale_Token=_Presale_Token;

    }

     receive() external payable {
     
        }

    function BuyTokenWithBNB(address payable referal) public payable nonReentrant
    {

        require(msg.value > 0, "Presale : Unsuitable Amount");
        require(referal!=msg.sender,"Referal Invalid");
        require(presaleStatus == true, "Presale : Presale is finished"); 
        require(msg.sender==tx.origin,"caller is not EOA!");
        uint256 referalreward=msg.value*referalpercent/100;
        uint256 actualValue=msg.value;
        if(referal==deadAddress)
        {
            actualValue=msg.value;
        }
        else
        {
            payable(referal).transfer(referalreward);
            actualValue =msg.value.sub(referalreward);
        }

        uint256 tokenamt=getTokenvalueperBNB(actualValue); 
        claim_amount[msg.sender]+=tokenamt;
      
    }

   

    function BuyTokenWithBUSD(address  referal,uint256 _BUSDAmount) external nonReentrant
    {
 
        require(presaleStatus == true, "Presale : Presale is finished"); 
        require(_BUSDAmount > 0, "Presale : Unsuitable Amount");
        require(referal!=msg.sender,"Referal Invalid");
        require(BUSD.balanceOf(msg.sender)>_BUSDAmount,"not enough BUSD in your wallet");
        require(msg.sender==tx.origin,"caller is not EOA!");
        BUSD.transferFrom(msg.sender, address(this), _BUSDAmount);  
        uint256 referalreward=_BUSDAmount*referalpercent/100;
        uint256 actualValue=_BUSDAmount;
       if(referal==deadAddress)
        {
            actualValue=_BUSDAmount;
        }
        else
        {
            BUSD.transfer(referal,referalreward);
            actualValue =_BUSDAmount.sub(referalreward);
        }
        uint256 tokenamt=getTokenvalueperBUSD(actualValue); 
        claim_amount[msg.sender]+=tokenamt;
    }


       function BuyTokenWithUSDT(address  referal,uint256 _USDTAmount) external nonReentrant
    {
 
        require(presaleStatus == true, "Presale : Presale is finished"); 
        require(_USDTAmount > 0, "Presale : Unsuitable Amount");
        require(referal!=msg.sender,"Referal Invalid");
        require(USDT.balanceOf(msg.sender)>_USDTAmount,"not enough USDT in your wallet");
        require(msg.sender==tx.origin,"caller is not EOA!");
        USDT.transferFrom(msg.sender, address(this), _USDTAmount);  
        uint256 referalreward=_USDTAmount*referalpercent/100;
        uint256 actualValue=_USDTAmount;
          if(referal==deadAddress)
        {
            actualValue=_USDTAmount;
        }
        else
        {
            USDT.transfer(referal,referalreward);
            actualValue =_USDTAmount.sub(referalreward);
        }
        uint256 tokenamt=getTokenvalueperUSDT(actualValue); 
        claim_amount[msg.sender]+=tokenamt;
    }


       function BuyTokenWithUSDC(address  referal,uint256 _USDCAmount) external nonReentrant
    {
 
        require(presaleStatus == true, "Presale : Presale is finished"); 
        require(_USDCAmount > 0, "Presale : Unsuitable Amount");
        require(USDC.balanceOf(msg.sender)>_USDCAmount,"not enough USDC in your wallet");
        require(referal!=msg.sender,"Referal Invalid");
        require(msg.sender==tx.origin,"caller is not EOA!");
        USDC.transferFrom(msg.sender, address(this), _USDCAmount);  
        uint256 referalreward=_USDCAmount*referalpercent/100;
        uint256 actualValue=_USDCAmount;
             if(referal==deadAddress)
        {
            actualValue=_USDCAmount;
        }
        else
        {
            USDC.transfer(referal,referalreward);
            actualValue =_USDCAmount.sub(referalreward);
        }
        uint256 tokenamt=getTokenvalueperUSDC(actualValue); 
        claim_amount[msg.sender]+=tokenamt;
    }



    function Claim() external nonReentrant{
        require(claim_amount[msg.sender]>0,"No Claimables!");
       uint256 claimable= claim_amount[msg.sender];
       Presale_Token.transfer(msg.sender,claimable);
       claim_amount[msg.sender]=0;
    }


     function getTokenvalueperBNB(uint256 value) public view returns(uint256)
    {
        return (TokenpricePerBNB.mul(value)).div(1e18);
    }

    function getTokenvalueperBUSD(uint256 value) public view returns(uint256)
    {
        return (TokenpricePerBUSD.mul(value)).div(1e18);
    }

    function getTokenvalueperUSDT(uint256 value) public view returns(uint256)
    {
        return (TokenpricePerUSDT.mul(value)).div(1e18);
    }

    function getTokenvalueperUSDC(uint256 value) public view returns(uint256)
    {
        return (TokenpricePerUSDC.mul(value)).div(1e18);
    }





    function setTokenPriceperBNB(uint256 _count) external onlyOwner {
        TokenpricePerBNB = _count;
    }

    function setTokenPriceperBUSD(uint256 _count) external onlyOwner {
        TokenpricePerBUSD = _count;
    }

    function setTokenPriceperUSDT(uint256 _count) external onlyOwner {
        TokenpricePerUSDT = _count;
    }
    function setTokenPriceperUSDC(uint256 _count) external onlyOwner {
        TokenpricePerUSDC = _count;
    }

    function stopPresale() external onlyOwner {
        presaleStatus = false;
    }

    function setreferalpercent(uint256 _referalpercent) external onlyOwner{
        referalpercent=_referalpercent;
    }

    function resumePresale() external onlyOwner {
        presaleStatus = true;
    }

    function ToogleClaim(bool _state) external onlyOwner{
        ClaimStatus=_state;
    }

    function recoverBEP20(address tokenAddress, uint256 tokenAmount) external onlyOwner {
        IBEP20(tokenAddress).transfer(msg.sender, tokenAmount);
        emit Recovered(tokenAddress, tokenAmount);
    }

       function releaseFunds() external onlyOwner 
    {
        payable(msg.sender).transfer(address(this).balance);
    }

    function setTokens(IBEP20 _usdc,IBEP20 _usdt,IBEP20 _busd,IBEP20 _presaletoken) external onlyOwner{
        USDC=_usdc;
        USDT=_usdt;
        BUSD=_busd;
        Presale_Token=_presaletoken;


    }

}