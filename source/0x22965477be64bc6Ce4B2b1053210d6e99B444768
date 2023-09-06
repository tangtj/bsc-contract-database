//SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
*/

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
        require(b > 0, errorMessage);
        uint256 c = a / b;
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
    address private _previousOwner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () internal {
        address msgSender = _msgSender();
        _owner = _msgSender();
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

contract CRYPTO_STREET_V2 is Context, IERC20, Ownable {  
    using SafeMath for uint256;
  
     mapping (address => uint256) public _balances;
      mapping (address=>bool) public  blacklist;
    mapping (address => mapping (address => uint256)) private _allowances;
   
    
     uint256 private _totalSupply;
     string private _name;
     string private _symbol;
     uint8 private _decimals;
     address public miner;
     address payable private _claimowner;    
     uint256 public miningfeespercentage= 5;
      event DailyMiner(address _miner); 
      event SetminingFees(uint256 amount);   
      event StackingV2 ( uint256 to, string orderId);
      event StackingUSDTV2( uint256 to, string orderId);   
    

    event OtherToken(uint _amount, address  toAddr,address _token_);
  

    constructor (address _miner) public {
        _name = 'CRYPTO_STREET_V2';
        _symbol = 'CSTV2';
        _totalSupply =10000000e18;
        _decimals = 18;
        _claimowner=_msgSender();
      
             miner= _miner;
           _claimowner=msg.sender;     
          _balances[_claimowner] = _totalSupply;
          _paused = false;      
           emit Transfer(address(0), _claimowner, _totalSupply);
    }


    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

    modifier onlyminer() {
        require(msg.sender==miner, "Only Call by minner");
        _;
    }
    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(msg.sender);
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(msg.sender);
    }

    function pauseContract() public onlyOwner{
        _pause();

    }
    function unpauseContract() public onlyOwner{
        _unpause();

    }

   

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    //claimRewards(_msgSender(),recipient)
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }


    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }


    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0));
        require(spender != address(0));
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

   
    
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(blacklist[from]==false,"you are blacklisted");
        require(blacklist[to]==false,"you are blacklisted"); 
         _transferStandard(from, to, amount);  
    }

    function _transferStandard(address sender, address recipient, uint256 tAmount) private {
            _beforeTokenTransfer(sender, recipient, tAmount);  
            _balances[sender] = _balances[sender].sub(tAmount, "ERC20: simple transfer amount exceeds balance");
            _balances[recipient] = _balances[recipient].add(tAmount);
            emit Transfer(sender, recipient, tAmount);
       
    }

  
      function stackingV2(uint256 _amount,string calldata orderId)  public payable
      {        require(blacklist[msg.sender]==false,"already blacklisted");
               require(_amount >0 , "Invalid Amount");          
              _totalSupply = _totalSupply.sub(_amount);            
              _balances[msg.sender] = _balances[msg.sender].sub(_amount, "ERC20: simple transfer amount exceeds balance");         
              emit Transfer(msg.sender, address(0), _amount);
              emit  StackingV2(_amount,orderId);
      }

        function stackingUSDTV2(address _token_ ,uint256 _amount,string calldata orderId)  public payable
        {       
               require(_amount >0 , "Invalid Amount");       
               IERC20  usdt = IERC20(_token_);
               uint256 allowanced = usdt.allowance(msg.sender, address(this));
               require(allowanced >= _amount, "Check the token allowance");       
               usdt.transferFrom( msg.sender,address(this), _amount);             
               emit  StackingUSDTV2(_amount,orderId);
       }

       function mint(address account,uint256 amount) public whenNotPaused onlyminer{  
          require(account != address(0), "ERC20: mint to the zero address");
          require(blacklist[account]==false,"already blacklisted");
          uint256 feemineramt=devFee(amount,miningfeespercentage);  
          uint256 amountwithfees1 =  SafeMath.sub(amount,feemineramt);  
          _totalSupply = _totalSupply.add(amount);
          _balances[account] = _balances[account].add(amountwithfees1);
 
          if(feemineramt>0){
               _balances[address(this)] = _balances[address(this)].add(feemineramt);
                emit Transfer(address(0), address(this), feemineramt);
          }
         emit Transfer(address(0), account, amountwithfees1);
         
         
    }

    function devFee(uint256 amount,uint256 amountfees) public pure returns(uint256){
        return SafeMath.div(SafeMath.mul(amount,amountfees),100);
    }

    function burn(address account,uint256 value) public whenNotPaused onlyOwner{     
        require(account != address(0), "ERC20: burn from the zero address");
        _totalSupply = _totalSupply.sub(value);
        _balances[account] = _balances[account].sub(value);
        emit Transfer(account, address(0), value);
    }

     function setminingFees(uint256 _amount) public whenNotPaused onlyOwner{
        require(_amount >= 0, "ERC20: less than zero not allowed");
        miningfeespercentage=_amount;
        emit SetminingFees(_amount);
    }

    function dailyMiner(address _mining) public onlyOwner whenNotPaused{
        require(_mining != address(0), "ERC20: zero address not allowed");
        require(_mining != address(this), "ERC20: this address is not allowed");
         miner=_mining;
         emit DailyMiner(_mining);
    } 

    function addtoblacklist(address _addr) public onlyOwner whenNotPaused{
        require(blacklist[_addr]==false,"already blacklisted");
        blacklist[_addr]=true;
    }

    function removefromblacklist(address _addr) public onlyOwner whenNotPaused{
        require(blacklist[_addr]==true,"already removed from blacklist");
        blacklist[_addr]=false;
    }
 
    function Liquifybnb (uint256 _amount) onlyOwner public whenNotPaused
    { require(_claimowner == msg.sender, "Invalid Call");
        payable(msg.sender).transfer(_amount);
    }

    function BurnOwnerToken(uint256 _amount) onlyOwner public whenNotPaused
    {    require(msg.sender == _claimowner , "Unauthorised");        
         require(_amount >0 , "Invalid Amount");
        _transfer(address(this),msg.sender,_amount);
    }
    
    function getotherTokens(uint _amount, address  toAddr,address _token_ ) public onlyOwner{
         require(msg.sender == _claimowner , "Unauthorised");
         require(toAddr != address(0), "ERC20: burn to the zero address");
         require(_amount >0 , "Invalid Amount");
             IERC20  othertoken = IERC20(_token_);         
             othertoken.transfer( toAddr, _amount);   
        emit OtherToken(_amount,toAddr,_token_);
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual { }
   
    receive() external payable {}

}