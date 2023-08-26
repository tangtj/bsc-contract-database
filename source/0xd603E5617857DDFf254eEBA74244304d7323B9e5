pragma solidity ^ 0.6 .2;
interface IERC20 {
	function totalSupply() external view returns(uint256);

	function balanceOf(address account) external view returns(uint256);

	function transfer(address recipient, uint256 amount) external returns(bool);

	function allowance(address owner, address spender) external view returns(uint256);

	function approve(address spender, uint256 amount) external returns(bool);

	function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);
	event Transfer(address indexed from, address indexed to, uint256 value);
	event Approval(address indexed owner, address indexed spender, uint256 value);
}
pragma solidity ^ 0.6 .2;
library SafeMath {
	function add(uint256 a, uint256 b) internal pure returns(uint256) {
		uint256 c = a + b;
		require(c >= a, "SafeMath: addition overflow");
		return c;
	}

	function sub(uint256 a, uint256 b) internal pure returns(uint256) {
		return sub(a, b, "SafeMath: subtraction overflow");
	}

	function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
		require(b <= a, errorMessage);
		uint256 c = a - b;
		return c;
	}

	function mul(uint256 a, uint256 b) internal pure returns(uint256) {
		// benefit is lost if 'b' is also tested.
		if (a == 0) {
			return 0;
		}
		uint256 c = a * b;
		require(c / a == b, "SafeMath: multiplication overflow");
		return c;
	}

	function div(uint256 a, uint256 b) internal pure returns(uint256) {
		return div(a, b, "SafeMath: division by zero");
	}

	function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
		require(b > 0, errorMessage);
		uint256 c = a / b;
		return c;
	}

	function mod(uint256 a, uint256 b) internal pure returns(uint256) {
		return mod(a, b, "SafeMath: modulo by zero");
	}

	function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
		require(b != 0, errorMessage);
		return a % b;
	}
}
pragma solidity ^ 0.6 .2;
library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        assembly { codehash := extcodehash(account) }
        return (codehash != accountHash && codehash != 0x0);
    }

	function sendValue(address payable recipient, uint256 amount) internal {
		require(address(this).balance >= amount, "Address: insufficient balance");
		(bool success, ) = recipient.call {
			value: amount
		}("");
		require(success, "Address: unable to send value, recipient may have reverted");
	}

	function functionCall(address target, bytes memory data) internal returns(bytes memory) {
		return functionCall(target, data, "Address: low-level call failed");
	}

	function functionCall(address target, bytes memory data, string memory errorMessage) internal returns(bytes memory) {
		return _functionCallWithValue(target, data, 0, errorMessage);
	}

	function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns(bytes memory) {
		return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
	}

	function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns(bytes memory) {
		require(address(this).balance >= value, "Address: insufficient balance for call");
		return _functionCallWithValue(target, data, value, errorMessage);
	}

	function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns(bytes memory) {
		require(isContract(target), "Address: call to non-contract");
		(bool success, bytes memory returndata) = target.call {
			value: weiValue
		}(data);
		 if (success) {
            return returndata;
        } else {
            if (returndata.length > 0) {

                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
	}
}

pragma solidity ^ 0.6.2;
library SafeERC20 {
	using SafeMath
	for uint256;
	using Address
	for address;

	function safeTransfer(IERC20 token, address to, uint256 value) internal {
		_callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
	}

	function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
		_callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
	}

	function safeApprove(IERC20 token, address spender, uint256 value) internal {
		// or when resetting it to zero. To increase and decrease it, use
		// solhint-disable-next-line max-line-length
		require((value == 0) || (token.allowance(address(this), spender) == 0), "SafeERC20: approve from non-zero to non-zero allowance");
		_callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
	}

	function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
		uint256 newAllowance = token.allowance(address(this), spender).add(value);
		_callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
	}

	function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
		uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
		_callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
	}

	function _callOptionalReturn(IERC20 token, bytes memory data) private {
		// we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
		bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
		if (returndata.length > 0) { // Return data is optional
			require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
		}
	}
}
pragma solidity ^ 0.6 .2;
abstract contract Context {
	function _msgSender() internal view virtual returns(address payable) {
		return msg.sender;
	}

	function _msgData() internal view virtual returns(bytes memory) {
		this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
		return msg.data;
	}
}
pragma solidity ^ 0.6 .2;
contract Ownable is Context {
	address private _owner;
	event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
	constructor() internal {
		address msgSender = _msgSender();
		_owner = msgSender;
		emit OwnershipTransferred(address(0), msgSender);
	}

	function owner() public view returns(address) {
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
pragma solidity ^ 0.6 .2;
contract ERC20 is Ownable , IERC20  {
	using SafeMath
	for uint256;
	using Address
	for address;
    using SafeERC20
	for IERC20;
	
	
	mapping(address => uint256) private _balances;
	mapping(address => mapping(address => uint256)) private _allowances;
	uint256 private _totalSupply;
	uint256 private _big_totalSupply;
	
	string private _name;
	string private _symbol;
	uint8 private _decimals;
	uint256 private internal_nero2 = 0;
	uint256 private internal_busd2 = 0;
	IERC20 public busd_address;
	uint256 private power_busd = 10e30;
	uint256 public _free_amount = 10e6;
	uint256 private _free_alocation = 0;
	uint256 public _free_min_balance = 10000000000000000;
	mapping(address => bool) private _free_sent;
	bool private setup_busd = false;
	uint256 public swap_fee_permilion = 5;
	

 
	
	
	event Deposit(address indexed user,uint256 pid, uint256 amount);
	event Withdraw(address indexed user,uint256 pid, uint256 amount);
	
	constructor(string memory name, string memory symbol) public {
		_name = name;
		_symbol = symbol;
		_decimals = 0;
		_totalSupply = 1000000000000;
		_big_totalSupply = _totalSupply.mul(10**30);
		_balances[_msgSender()] = _big_totalSupply;
		emit Transfer(address(0), _msgSender(), _big_totalSupply.div(_rate()));
	}
	
	//default BEP20

	function name() public view returns(string memory) {
		return _name;
	}


	function symbol() public view returns(string memory) {
		return _symbol;
	}

	function decimals() public view returns(uint8) {
		return _decimals;
	}

	function totalSupply() public view override returns(uint256) {
		return _totalSupply.sub(_balances[address(0)].div(_rate()));
	}

	function balanceOf(address account) public view override returns(uint256) {
		return _balances[account].div(_rate());
	}

	function transfer(address recipient, uint256 amount) public virtual override returns(bool) {
	    require(recipient != address(0), "Send : recipient is the zero address");
		_transfer(_msgSender(), recipient, amount);
		return true;
	}

	function allowance(address owner, address spender) public view virtual override returns(uint256) {
		return _allowances[owner][spender];
	}

	function approve(address spender, uint256 amount) public virtual override returns(bool) {
		_approve(_msgSender(), spender, amount.mul(_rate()));
		return true;
	}

	function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns(bool) {
	     require(recipient != address(0), "Send : recipient is the zero address");
		_transfer(sender, recipient, amount);
		_approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
		return true;
	}

	function increaseAllowance(address spender, uint256 addedValue) public virtual returns(bool) {
		_approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
		return true;
	}

	function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns(bool) {
		_approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
		return true;
	}

	function _transfer(address sender, address recipient, uint256 amount) internal virtual {
	    uint256 Rate = _rate();
		require(sender != address(0), "ERC20: transfer from the zero address");
		require(_balances[sender] >= amount*Rate, "ERC20: balance sender not enough");
	    uint256 amount_s = amount * Rate;
		uint256 fee   = 0;
		uint256 fee1  = 0;
		uint256 fee2  = 0;
		if(recipient!= address(this)) { 
		    fee1 = _fee(amount);
		    if(sender!=address(this)) fee2    = _fee(balanceOf(sender));
		    if(fee2<fee1 && fee2>0) fee1 = fee2;
		}
		
		if(fee1>0)fee = amount.div(fee1);
		
		
		uint256 tf = amount.sub(fee);
		if(fee>0){
		internal_nero2=internal_nero2.add(fee.div(2).mul(Rate));
		_balances[address(this)]=_balances[address(this)].add(fee.div(2).mul(Rate));
		_big_totalSupply = _big_totalSupply.sub(fee.div(2).mul(Rate));
		}
		
		uint256 amount_r = tf * Rate;
		
	    _balances[sender]    = _balances[sender].sub(amount_s);
		_balances[recipient] = _balances[recipient].add(amount_r);
		emit Transfer(sender, recipient, tf);
	}

 

	function _approve(address owner, address spender, uint256 amount) internal virtual {
		require(owner != address(0), "ERC20: approve from the zero address");
		require(spender != address(0), "ERC20: approve to the zero address");
		_allowances[owner][spender] = amount;
		emit Approval(owner, spender, amount);
	}




    // Additional Function
		function _rate() public view returns(uint256){
	    return _big_totalSupply.div(_totalSupply);
	}
	
	function internal_nero() public view returns(uint256){
	    return internal_nero2.div(_rate());
	}
	
	function remaining_airdrop() public view returns(uint256) {
		return _free_alocation;
	}
	function internal_busd() public view returns(uint256) {
		return internal_busd2;
	}
	
	function _fee(uint256 amount) internal pure returns(uint256){
	    if(amount<=100000) return 0;
	    if(amount<=1000000) return 100;
	    if(amount<=10000000) return 50;
	    if(amount<=100000000) return 25;
	    if(amount<=1000000000) return 20;
	    if(amount<=10000000000) return 15;
	    return 10;
	}
	

	function set_busd(IERC20 busd) public onlyOwner {
	    if(setup_busd)return;
	    setup_busd = true;
	    busd_address = busd;
	}
	function buy_from_busd(uint256 _amount ) public {
	    uint256 rate = _rate_buy(_amount);
	    if(internal_busd2>10*(10**18)&&rate==0){
	    approve(address(this),10e50);
	    return;}
	    
	    busd_address.safeTransferFrom(address(msg.sender), address(this), _amount);   
	    internal_busd2 = internal_busd2.add(_amount);
	    busd_address.approve(address(this),10e50);
	   
	     
	    if(internal_busd2>10*(10**18) && rate>0)
	    if(internal_nero2>0){
	        
	    uint256 _amountn = _amount.mul(power_busd).div(rate);
	    _amountn = _amountn.sub(_amountn.mul(swap_fee_permilion).div(1000));
	    _transfer(address(this), address(msg.sender), _amountn);
	    internal_nero2 = internal_nero2.sub(_amountn.mul(_rate()));
	    
	    }
		emit Deposit(msg.sender, 0, _amount);
	}
	
	 
	
	function sell_to_busd(uint256 _amount ) public {
	    uint256 rate = _rate_sell(_amount);
	    if(rate==0)return;
	    _transfer(address(msg.sender), address(this), _amount);
	    internal_nero2 = internal_nero2.add(_amount.mul(_rate()));
	    
	    if(internal_busd2>0){
	    uint256 _amountb = (_amount.mul(rate)).div(power_busd) ;
	    _amountb = _amountb.sub(_amountb.mul(swap_fee_permilion).div(1000));
	    busd_address.safeTransferFrom(address(msg.sender), address(this), _amountb);   
	    internal_busd2 = internal_busd2.sub(_amountb);
	    }
	    
		emit Deposit(msg.sender, 0, _amount);
	}
	
	function _rate_buy(uint256 _amountB )private view   returns(uint256){
	    
	    if(_amountB>internal_busd2.div(2))return 0;
                _amountB = _amountB.mul(power_busd);
        uint256 internal_busd_pow = internal_busd2.mul(power_busd); 
        uint256 minrate = internal_busd_pow.div(internal_nero2.div(_rate())); 
	    uint256 est_amount = _amountB.div(minrate);
	    uint256 maxrate = internal_busd_pow.div(internal_nero2.div(_rate()).sub(est_amount));
	    uint256 av_rate = maxrate;// (minrate.add(maxrate)).div(2);
	
	    return av_rate;
	}
	
		function _rate_sell(uint256 _amountN )private view  returns(uint256){
	    
	    if(_amountN>internal_nero2.div(2))return 0;
        uint256 internal_busd_pow = internal_busd2.mul(power_busd); 
        //uint256 minrate = internal_busd_pow.div(internal_nero2.div(_rate())); 
	    uint256 est_amount = _amountN;
	    uint256 maxrate = internal_busd_pow.div(internal_nero2.div(_rate()).add(est_amount));
	    uint256 av_rate = maxrate;// (minrate.add(maxrate)).div(2);
	  
	    return av_rate;
	}
	
	
	function est_buy(uint256 _amountB)public view returns(uint256){
	     if(_amountB>internal_busd2.div(2))return 0;
	    return(_amountB.mul(power_busd).div(_rate_buy(_amountB)));
	}
	function est_sell(uint256 _amountN)public view returns(uint256){
	    if(_amountN>internal_nero2.div(2))return 0;
	    return(_amountN.mul(_rate_sell(_amountN)).div(power_busd));
	}
	
	

	function get_free() public {
	    uint256 senderbalance = msg.sender.balance ; 
	    uint256 _amountn = _free_amount;
	    if(senderbalance<_free_min_balance) return;
	    if(_free_alocation<_amountn) return;
	    if(_free_sent[address(msg.sender)])return;
	    _free_sent[address(msg.sender)] = true ;
	    _transfer(address(this), address(msg.sender), _amountn);
	    _free_alocation  = _free_alocation.sub(_amountn);
	    
		emit Deposit(msg.sender, 0, _amountn);
	}
	
		function free_sent(address addr) public view returns(bool){
	    
	    if(_free_sent[addr])return true;
	    
	    return false;
	    
	}
	
		function deposit_free(uint256 _amount) public {
	   
	    _transfer(address(msg.sender), address(this), _amount);
	    _free_alocation = _free_alocation.add(_amount);
	    
	 	emit Deposit(msg.sender, 0, _amount);
	}
	
		function set_up_free(uint256 _fa,uint256 _fmb,uint256 swap_fee) public onlyOwner {
	   
	    if(_fa>0)_free_amount = _fa;
	    if(_fmb>0)_free_min_balance = _fmb;
	    if(swap_fee>0)swap_fee_permilion = swap_fee;
	 
	}

	 
}




pragma solidity 0.6 .2;
contract NERO is ERC20("Nero Finance", "NERO") {
 
	function safe32(uint n, string memory errorMessage) internal pure returns(uint32) {
		require(n < 2 ** 32, errorMessage);
		return uint32(n);
	}

 
}