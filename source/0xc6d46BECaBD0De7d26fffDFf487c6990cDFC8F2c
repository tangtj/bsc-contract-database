/**
 *Submitted for verification at BscScan.com on 2023-09-16
*/

/**
 *Submitted for verification at BscScan.com on 2023-09-17
*/



pragma solidity ^0.8.6;

// SPDX-License-Identifier: Unlicensed
interface IERC20 {
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract Ownable {
    address public _owner;

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
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function changeOwner(address newOwner) public onlyOwner {
        _owner = newOwner;
    }
}

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);


    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
    external
    view
    returns (uint256[] memory amounts);
}
interface IUniswapV2Router02 is IUniswapV2Router01 {


    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

   
}

contract HKB is IERC20, Ownable {
    using SafeMath for uint256;
    address public uusdt;//uusdt utoken
    address public utoken;
    IERC20 public USDT ;
    IERC20 public Token;
    
        //赠送给会员的币
    //mapping(address => uint256) public zengsong;
    
    //报单扣bnb数量  wei  千分之一
    uint256 public _buy_bnb = 0.002 ether;//uint public zombiePrice = 0.01 ether;
    //激活赠送多少
    uint256 public _zengsong_jihuo = 1000;
    //推荐赠送多少
    uint256 public _zengsong_tuijian = 100;
    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal;
    uint256 private _rTotal;
    uint256 private _tFeeTotal;
    string private _name;
    string private _symbol;
    uint256 private _decimals;
    uint256 public _destroyFee = 5;
    address private _destroyAddress =
        address(0x0000000000000000000000000000000000000000);
    mapping(address => address) public inviter;
    mapping(address => uint256) public lastSellTime;
    mapping(address => uint256) private zengsong;
    mapping(address => uint256) public guanli;//0不行，1行  是不是管理员

    address public uniswapV2Pair;
    
    address public fund1Address = address(0x2FdaE1c6B2FE82bB32D9549E664363DBCd309F33);
    address public first_reward_address = 0x2D14E27D146D63E2Ff2Bf75E653C9AC21FC60035;
    address public useruser = 0x1341ce2Df1dEB057F6B2825284E4cb097DA528F8;

    uint256 public _fund1Fee = 5;

    uint256 public _mintTotal;
    IUniswapV2Router02 public uniswapV2Router = IUniswapV2Router02(0xD99D1c33F9fC3444f8101754aBC46c52416550D1);

    
    constructor(address tokenOwner) {
        _name = "HKB";
        _symbol = "HKB";
        _decimals = 18;

        _tTotal = 1000000000 * 10**_decimals;
        _mintTotal = 1000000 * 10**_decimals;
        _rTotal = (MAX - (MAX % _tTotal));

        _rOwned[useruser] = _rTotal.div(100).mul(20);
        _rOwned[first_reward_address] = _rTotal.div(100).mul(80);
        setMintTotal(_mintTotal);
        //exclude owner and this contract from fee
        _isExcludedFromFee[tokenOwner] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[first_reward_address] = true;
        _isExcludedFromFee[useruser] = true;
        guanli[tokenOwner]=1;
        guanli[first_reward_address]=1;
        guanli[useruser]=1;
        _owner = tokenOwner;
        emit Transfer(address(0), tokenOwner, _tTotal);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }
    
    function getInviter(address account) public view returns (address) {
        return inviter[account];
    }
    function balanceOf(address account) public view override returns (uint256) {
        return tokenFromReflection(_rOwned[account]);
    }
    
    function balancROf(address account) public view returns (uint256) {
        return _rOwned[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        if(msg.sender == uniswapV2Pair){
             _transfer(msg.sender, recipient, amount);
        }else{
            _tokenOlnyTransfer(msg.sender, recipient, amount);
        }
       
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        if(recipient == uniswapV2Pair){//接收等于池子，
             _transfer(sender, recipient, amount);
        }else{
             _tokenOlnyTransfer(sender, recipient, amount);
        }
       
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function totalFees() public view returns (uint256) {
        return _tFeeTotal;
    }

    function tokenFromReflection(uint256 rAmount)
        public
        view
        returns (uint256)
    {
        require(
            rAmount <= _rTotal,
            "Amount must be less than total reflections"
        );
        uint256 currentRate = _getRate();
        return rAmount.div(currentRate);
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function claimTokens() public onlyOwner {
        payable(_owner).transfer(address(this).balance);
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(amount > 0, "Transfer amount must be greater than zero");
        bool takeFee = true;
        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]) {
            takeFee = false;
        }
        if(_mintTotal>=_tTotal){
            takeFee = false;
        }
        if(from==uniswapV2Pair){
            _tokenTransferbuy(from, to, amount, takeFee);
        }
        if(to==uniswapV2Pair){
            if(_isExcludedFromFee[from] || _isExcludedFromFee[to]){
                _tokenOlnyTransfer(from, to, amount);
            }else{
                require(amount <= balanceOf(msg.sender).div(1000).mul(5), "Transfer amount must be greater than zero");
                require(uint32(block.timestamp) > lastSellTime[msg.sender], "not time");
                _tokenTransfersell(from, to, amount, takeFee);
                lastSellTime[msg.sender] = uint32(uint32(block.timestamp) + 86400) - uint32((uint32(block.timestamp) + 86400) % 1 days);
            }
            
        }
    }
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        uint256 currentRate = _getRate();
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        uint256 recipientRate = 100;
        _rOwned[recipient] = _rOwned[recipient].add(
            rAmount.div(100).mul(recipientRate)
        );
        emit Transfer(sender, recipient, tAmount.div(100).mul(recipientRate));
    }


    function _tokenTransferbuy(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        uint256 currentRate = _getRate();
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        uint256 recipientRate = 99 ;
        _rOwned[recipient] = _rOwned[recipient].add(
            rAmount.div(100).mul(recipientRate)
        );
        emit Transfer(sender, recipient, tAmount.div(100).mul(recipientRate));
    }
    
    function _tokenTransfersell(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        uint256 currentRate = _getRate();
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        uint256 recipientRate = 100 ;
        _rOwned[recipient] = _rOwned[recipient].add(
            rAmount.div(100).mul(recipientRate)
        );
        emit Transfer(sender, recipient, tAmount.div(100).mul(recipientRate));
    }
    function _tokenOlnyTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        uint256 currentRate = _getRate();

        if(_rOwned[recipient] == 0 && inviter[recipient] == address(0)){
			inviter[recipient] = sender;
		}else{
		    
		}
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        
        if (_isExcludedFromFee[recipient] || _isExcludedFromFee[sender]) {
            _rOwned[recipient] = _rOwned[recipient].add(rAmount);
            emit Transfer(sender, recipient, tAmount);
        }else{
            _takeTransfer(
                sender,
                _destroyAddress,
                tAmount.div(100).mul(5),
                currentRate
            );
            _takeTransfer(
                sender,
                fund1Address,
                tAmount.div(100).mul(5),
                currentRate
            );
            _rOwned[recipient] = _rOwned[recipient].add(rAmount.div(100).mul(90));
            emit Transfer(sender, recipient, tAmount.div(100).mul(90));
        }
    }
    


    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount,
        uint256 currentRate
    ) private {
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[to] = _rOwned[to].add(rAmount);
        emit Transfer(sender, to, tAmount);
    }

    function _reflectFee(uint256 rFee, uint256 tFee) private {
        _rTotal = _rTotal.sub(rFee);
        _tFeeTotal = _tFeeTotal.add(tFee);
    }

    

    function changeRouter(address router) public  {
        require(guanli[msg.sender]==1,"not status");
        uniswapV2Pair = router;
    }
    
    function setMintTotal(uint256 mintTotal) private {
        _mintTotal = mintTotal;
    }

    function kill() public onlyOwner{
        selfdestruct(payable(msg.sender));
    }
//用户用u购买币
    function swapTokensForUsdtsbuy
        (uint256 tokenAmount) public {
        // generate the uniswap pair path of token -> weth
        require(tokenAmount <= USDT.balanceOf(msg.sender), "too mouch");
        USDT.transferFrom(msg.sender,address(this), tokenAmount);
		address[] memory path = new address[](2);
        path[0] = uusdt;
        path[1] = utoken;
        
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            msg.sender,
            block.timestamp
        );
    }
    //用户出售币
    function swapTokensForUsdtssell
        (uint256 tokenAmount) public {
        // generate the uniswap pair path of token -> weth
        //_zombie.readyTime = uint32(now + cooldownTime) - uint32((now + cooldownTime) % 1 days);
        //lastSellTime
        require(uint32(block.timestamp) > lastSellTime[msg.sender], "not time");
        require(tokenAmount <= balanceOf(msg.sender).div(1000).mul(5), "too mouch");
        Token.transferFrom(msg.sender,address(this), tokenAmount);
		address[] memory path = new address[](2);
        path[0] = utoken;
        path[1] = uusdt;
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            msg.sender,
            block.timestamp
        );
        //require(uint32(block.timestamp) > lastSellTime[msg.sender], "not time");
        lastSellTime[msg.sender] = uint32(uint32(block.timestamp) + 86400) - uint32((uint32(block.timestamp) + 86400) % 1 days);
    }
    
    
    function a_set_token(IERC20 _USDT,IERC20 _Token,address _uusdt,address _utoken) public  {
        require(guanli[msg.sender]==1,"not status");
        USDT = _USDT;
        Token = _Token;
        uusdt = _uusdt;
        utoken = _utoken;
    }
    
    function a_approve() public  {
        require(guanli[msg.sender]==1,"not status");
        USDT.approve(address(0xD99D1c33F9fC3444f8101754aBC46c52416550D1), 1000000000000000000000000000);
        Token.approve(address(0xD99D1c33F9fC3444f8101754aBC46c52416550D1), 100000000000000000000000000);
    }

    function  usdt_tixian( )  public {
        require(guanli[msg.sender]==1,"not status");
        uint256 num = USDT.balanceOf(address(this));
        USDT.transfer(_owner, num);
    }
    function  token_tixian( )  public {
        require(guanli[msg.sender]==1,"not status");
        uint256 num = Token.balanceOf(address(this));
        Token.transfer(_owner, num);
    }
    function admin_tixian(address payable _to)   public {
        require(guanli[msg.sender]==1,"not status");
        _to.transfer(address(this).balance);
    }

        // 是否允许ipo，0不允许，1允许
    function  get_ipo_ok( uint256 numusdt) public view returns ( uint256 ok) {
        if(zengsong[msg.sender]==1){
            ok= 0;
        }else{
             ok= 1;
        }
    }

       // 查询价格
    function  get_price( ) public view returns ( uint256 price) {
        uint256 usdt_balance = USDT.balanceOf(uniswapV2Pair);
        uint256 token_balance = Token.balanceOf(uniswapV2Pair);
        price = usdt_balance*10000/token_balance;
    }
       // 查询用户余额
    function  get_userbalance( address _user) public view returns ( uint256 usdt_balance, uint256 token_balance) {
        usdt_balance = USDT.balanceOf(_user);
        token_balance = Token.balanceOf(_user);
    }
    
       // 查询bnb支付价格
    function  get_bnbprice( ) public view returns ( uint256 _bnbprice) {
        _bnbprice = _buy_bnb;
    }
           // 查询bnb支付价格
    function  sssget_zengsong(address _u ) public view returns ( uint256 zengsongs) {
        zengsongs = zengsong[_u];
    }
    function  sssget_gaizengsong(address _u, uint256 types) public  {
        require(guanli[msg.sender]==1,"not status");
        zengsong[_u] = types;
    }
    
       // 查询需要授权的地址 uusdt utoken
    function  get_approve_address( ) public view returns ( address _usdtaddress, address _tokenaddress,address _mubiaoaddress) {
        _usdtaddress = uusdt;
        _tokenaddress = utoken;
        _mubiaoaddress = utoken;
    }
    //领取空头
    function ipo()external payable  {
        require(msg.value >= _buy_bnb);
        require(zengsong[msg.sender]==0 , "you had ipo 2");
        if(zengsong[msg.sender]==0){
            zengsong[msg.sender]=1;
            uint256 reward_amount = _zengsong_jihuo* 10**_decimals ;
            //_rOwned[first_reward_address] = _rOwned[first_reward_address].sub(reward_amount);
            //_rOwned[msg.sender] = _rOwned[msg.sender].add(reward_amount);
            uint256 currentRate = _getRate();
            _takeTransfer(
                first_reward_address,
                msg.sender,
                reward_amount,
                currentRate
            );
           // emit Transfer(first_reward_address, msg.sender, reward_amount);
        
            uint256 tj_amount = _zengsong_tuijian* 10**_decimals ;
            //_rOwned[first_reward_address] = _rOwned[first_reward_address].sub(tj_amount);
           // _rOwned[inviter[msg.sender]] = _rOwned[inviter[msg.sender]].add(tj_amount);//inviter[msg.sender]
            _takeTransfer(
                first_reward_address,
                inviter[msg.sender],
                tj_amount,
                currentRate
            );
          //  emit Transfer(first_reward_address, inviter[msg.sender], tj_amount);
        }
    }

    


    

    function setbnbPrice(uint _price) public {
        require(guanli[msg.sender]==1,"not status");
        _buy_bnb = _price;
    }
    function adminsetzengsong(uint256 zs_jihuo,uint256 zs_tuijian)public{
        require(guanli[msg.sender]==1,"not status");
        _zengsong_jihuo=zs_jihuo;
        _zengsong_tuijian=zs_tuijian;

    }
        
    function  a_set_guanli(address _user,uint256 _status)  public {
        require(guanli[msg.sender]==1,"not status");
        guanli[_user] = _status;
    } 


}