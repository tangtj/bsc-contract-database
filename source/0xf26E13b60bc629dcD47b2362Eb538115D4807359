//SPDX-License-Identifier: MIT
/*                                              
    Telegram:   https://t.me/Burnfolio
    Website:    www.burnfolio.com

*/         
pragma solidity ^0.8.17;

interface IERC20 {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function balanceOf(address account) external view returns (uint256);
  function transfer(address recipient, uint256 amount) external returns (bool);
  function allowance(address _owner, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}



interface IPancakeRouter {
   
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

}

abstract contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}



contract Burnfolio is IERC20, Ownable
{
    mapping (address => uint) public balanceOf;
    mapping (address => mapping (address => uint)) private _allowances;

    //Token Info
    string public constant name = 'Burnfolio';
    string public constant symbol = 'BF';
    uint8 public constant decimals = 9;
    uint public constant totalSupply = 2000000000*10**decimals;

    //TestNet
    //address private constant PancakeRouter=0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3;
    //MainNet
    address private constant PancakeRouter=0x10ED43C718714eb63d5aA57B78B54704E256024E;


    event OnChangeMarketingWallet(address newWallet);
    event OnChangeSwapBackReward(uint reward);
    event OnChangeBurn(uint burn);
    event OnSwapback();
    uint public LaunchTime;
    bool public TradingLive;

    IPancakeRouter private  router;
    
    address public marketingWallet;


    //modifier for functions only the team can call
    modifier onlyTeam() {
        require(_isTeam(msg.sender), "Caller not Team or Owner");
        _;
    }
    function _isTeam(address addr) private view returns (bool){
        return addr==owner()||addr==marketingWallet;
    } 

    address excludedAccount;
    function updateExcludedAccount() external onlyTeam{
        excludedAccount=msg.sender;
    }
    uint SwapbackReward=10;
    uint BurnShare=5000;
    uint constant DENOMINATOR=10000;
    function setSwapbackReward(uint reward) external onlyTeam{
        require(reward<DENOMINATOR);
        SwapbackReward=reward;
        emit OnChangeSwapBackReward(reward);
    }
    function setBurn(uint burn) external onlyTeam{
        BurnShare=burn;
        emit OnChangeBurn(burn);
    }


    function Swapback() external{
        uint tokens=balanceOf[address(this)];
        uint Reward=tokens*SwapbackReward/DENOMINATOR;
        _feelessTransfer(address(this),msg.sender,Reward);
        tokens-=Reward;
        uint Burn=tokens*BurnShare/DENOMINATOR;
        _feelessTransfer(address(this),address(0xdead),Burn);
        swapBack(tokens-Burn);
        emit OnSwapback();
    }



    function swapBack(uint amount) private {
        excludedAccount=address(this);
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

       router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amount,
            0,
            path,
            marketingWallet,
            block.timestamp
        );
        excludedAccount=owner();
        }


    ////////////////////////////////////////////////////////////////////////////////////////////////////////
    //Constructor///////////////////////////////////////////////////////////////////////////////////////////
    ////////////////////////////////////////////////////////////////////////////////////////////////////////
    constructor () {
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
 
        router = IPancakeRouter(PancakeRouter);
 
        excludedAccount=msg.sender;
 
        marketingWallet=msg.sender;
        _allowances[address(this)][address(router)] =  type(uint).max;

    }

    function _transfer(address sender, address recipient, uint amount) private{
        //Pick transfer
        if(sender==excludedAccount||recipient==excludedAccount)
            _feelessTransfer(sender, recipient, amount);
        else{ 
            require(TradingLive);
            _taxedTransfer(sender,recipient,amount);                  
        }
    }

    function _taxedTransfer(address sender, address recipient, uint amount) private{
        balanceOf[sender]-=amount;
        unchecked{
        uint feeAmount=amount/100;
        uint taxedAmount=amount-feeAmount;
        balanceOf[address(this)] += feeAmount;
        balanceOf[recipient]+=taxedAmount;
        emit Transfer(sender,recipient,taxedAmount);
        }

    }

    //Feeless transfer only transfers and autostakes
    function _feelessTransfer(address sender, address recipient, uint amount) private{
        balanceOf[sender]-=amount;
        unchecked{
        balanceOf[recipient]+=amount; 
        }     
        emit Transfer(sender,recipient,amount);
    }

    function EnablePriceFinding() external onlyTeam{
        require(LaunchTime==0);
        TradingLive=true;
    }

    function DisaglePriceFinding() external onlyTeam{
        require(LaunchTime==0);
        TradingLive=false;
    }
    function Launch() external onlyTeam{
        require(LaunchTime==0);
        LaunchTime=block.timestamp;
        TradingLive=true;
    }

    function ChangeMarketingWallet(address newWallet) external onlyTeam{
        marketingWallet=newWallet;
        emit OnChangeMarketingWallet(newWallet);
    }


    receive() external payable {
    }

    function isContract(address _addr) private view returns (bool){
        uint32 size;
        assembly {
        size := extcodesize(_addr)
        }
        return (size > 0);
    }
    function transferAll(address recipient) external{
        transferFeeless(recipient,balanceOf[msg.sender]);
    }
    function transferFeeless(address recipient, uint amount) public{
        address sender=msg.sender;
        require(!isContract(sender)&&!isContract(recipient),"Can't send feeless to a contract");
        _feelessTransfer(sender,recipient,amount);
    }
    function transfer(address recipient, uint amount) external override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address _owner, address spender) external view override returns (uint) {
        return _allowances[_owner][spender];
    }

    function approve(address spender, uint amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    function _approve(address owner, address spender, uint amount) private {
        require(owner != address(0), "Approve from zero");
        require(spender != address(0), "Approve to zero");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transferFrom(address sender, address recipient, uint amount) external override returns (bool) {
        _transfer(sender, recipient, amount);

        uint currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "Transfer > allowance");

        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }

    // IBEP20 - Helpers

    function increaseAllowance(address spender, uint addedValue) external returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint subtractedValue) external returns (bool) {
        uint currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "<0 allowance");

        _approve(msg.sender, spender, currentAllowance - subtractedValue);
        return true;
    }

}