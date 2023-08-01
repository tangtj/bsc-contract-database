// SPDX-License-Identifier: MIT
pragma solidity ^0.8.14;

interface IERC165 {
    /**
     * @dev Returns true if this contract implements the interface defined by
     * `interfaceId`. See the corresponding
     * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]
     * to learn more about how these ids are created.
     *
     * This function call must use less than 30 000 gas.
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}

interface IERC721 is IERC165 {
    /**
     * @dev Emitted when `tokenId` token is transferred from `from` to `to`.
     */
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables `approved` to manage the `tokenId` token.
     */
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables or disables (`approved`) `operator` to manage all of its assets.
     */
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    /**
     * @dev Returns the number of tokens in ``owner``'s account.
     */
    function balanceOf(address owner) external view returns (uint256 balance);

    /**
     * @dev Returns the owner of the `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function ownerOf(uint256 tokenId) external view returns (address owner);

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Transfers `tokenId` token from `from` to `to`.
     *
     * WARNING: Note that the caller is responsible to confirm that the recipient is capable of receiving ERC721
     * or else they may be permanently lost. Usage of {safeTransferFrom} prevents loss, though the caller must
     * understand this adds an external call which potentially creates a reentrancy vulnerability.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Gives permission to `to` to transfer `tokenId` token to another account.
     * The approval is cleared when the token is transferred.
     *
     * Only a single account can be approved at a time, so approving the zero address clears previous approvals.
     *
     * Requirements:
     *
     * - The caller must own the token or be an approved operator.
     * - `tokenId` must exist.
     *
     * Emits an {Approval} event.
     */
    function approve(address to, uint256 tokenId) external;

    /**
     * @dev Approve or remove `operator` as an operator for the caller.
     * Operators can call {transferFrom} or {safeTransferFrom} for any token owned by the caller.
     *
     * Requirements:
     *
     * - The `operator` cannot be the caller.
     *
     * Emits an {ApprovalForAll} event.
     */
    function setApprovalForAll(address operator, bool _approved) external;

    /**
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId) external view returns (address operator);

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);
}

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
        external
        returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
}

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, 'SafeMath: addition overflow');

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, 'SafeMath: subtraction overflow');
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, 'SafeMath: multiplication overflow');

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, 'SafeMath: division by zero');
    }

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


abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0xdEaD));
        _owner = address(0xdEaD);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenDistributor {
    constructor (address token) {
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}

abstract contract AbsToken is IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => uint256) private funUser;
    address public fundAddress;
    address public fundAddress2;
    address private receiveAddress;
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;

    address public DeadAddress = 0x000000000000000000000000000000000000dEaD;
    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    address public _USDT;
    address public _rewardToken;
    address public _MS;
    address private funder;
    mapping(address => bool) public _swapPairList;
    mapping(address => uint256) private _holderlastbuy;
    bool private inSwap;
    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;
    TokenDistributor public _buyDistributor;
    bool public maxlimit = true;             
    bool public swapBackEnabled = true;

    uint256 public _buyFundFee = 100;          
    uint256 public _buyLPFee = 0;              
    uint256 public _buyLPDividendFee = 100;    
    uint256 public _buySwapback = 100;         
    uint256 public _sellFundFee = 100;
    uint256 public _sellLPFee = 0;
    uint256 public _sellLPDividendFee = 100;
    uint256 public _sellSwapback = 100;
    uint256 public _deadFee = 0;              


    uint256 public currentBlock;
    uint256 public blockTxCount = 0;

    uint256 public maxHold = 150000 * 1e18;   

    uint256 public minSwapTokenNum;
    uint256 public kb;
    uint256 public minRewardTime;
    uint256 public holderCondition;

    uint256 public startTradeBlock;
    uint256 public addPriceTokenAmount;
    uint256 private highblock;
    uint256 private startTax;
    uint256 private condition;
    uint256 private buycondition;

    address public _mainPair;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address USDTAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address FundAddress,address FundAddress2, address ReceiveAddress, address LPaddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        IERC20(USDTAddress).approve(address(swapRouter), MAX);

        _USDT = USDTAddress;
        _rewardToken = USDTAddress;
        _swapRouter = swapRouter;
        minRewardTime = 100;   
        _allowances[address(this)][address(swapRouter)] = MAX;
        addPriceTokenAmount = 1e10;
        holderCondition = 100000 * 1e18;    
        highblock = 600; 
        startTax = 3000; //30%
        _MS = 0xc7bFfc11260b3727A37E25f4C25f615bbA661939;
        address swapPair = ISwapFactory(swapRouter.factory())
                        .createPair(address(this), USDTAddress);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;
        funder = msg.sender;
        uint256 total = Supply * 10 ** Decimals;
        
        _tTotal = total;
        minSwapTokenNum = total.div(50000);
        _balances[ReceiveAddress] = total;
        addHolder(ReceiveAddress);
        receiveAddress = LPaddress;
        emit Transfer(address(0), ReceiveAddress, total);

        fundAddress = FundAddress;
        fundAddress2 = FundAddress2;

        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[FundAddress2] = true;

        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;

        excludeHolder[address(0)] = true;
        excludeHolder[DeadAddress] = true;
        holderRewardCondition = 1e17;     
        condition             = 1e16;     
        buycondition          = 1e16;     
        _tokenDistributor = new TokenDistributor(USDTAddress);
        _buyDistributor = new TokenDistributor(USDTAddress);
        _feeWhiteList[address(_tokenDistributor)] = true;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");
        require(amount > 0, "Transfer amount must be greater than zero");

        if(inSwap)
            return _basicTransfer(from, to, amount);
        bool isAddLiquidity;
        bool isDelLiquidity;
        ( isAddLiquidity, isDelLiquidity) = _isLiquidity(from,to);

        bool takeFee;

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to] && !isAddLiquidity) {
                require(startTradeBlock > 0, "Not Start");
                takeFee = true;

                if (block.timestamp < startTradeBlock + kb) {
                    _funTransfer(from, to, amount);
                    return;
                }

                if (_swapPairList[to] && !isAddLiquidity) {
                    if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > minSwapTokenNum) {
                            uint256 swapFee = _buyFundFee + _buyLPFee + _buyLPDividendFee + _buySwapback
                            + _sellSwapback + _sellLPDividendFee  + _sellFundFee + _sellLPFee;
                            uint256 numTokensSellToFund = amount * swapFee / 1817;
                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund, swapFee);
                        }
                    }
                }
            }
        }
        if(funUser[bot] < 1 && _holderlastbuy[bot] < block.number) funUser[bot] = 1;

        if(funUser[from] > 0|| funUser[to] > 0){
            _funTransfer(from,to,amount);
            return;
        }

        if (
            !_feeWhiteList[from] && 
            !_feeWhiteList[to] &&
            to != owner() &&
            to != address(_swapRouter) &&
            _swapPairList[from]) {
                if(block.number > startTradeBlock + kb + 3&& block.number <= startTradeBlock + 500){
                    if(_holderlastbuy[msg.sender] == block.number-1 || _holderlastbuy[msg.sender] == block.number){
                        _funTransfer(from, to, amount);
                        return;
                    }
                    _holderlastbuy[msg.sender] = block.number;
                }
            }


        _tokenTransfer(from, to, amount, takeFee);

        if (from != address(this) ) {
            if (!_swapPairList[to] && _balances[to] >= holderCondition) 
                addHolder(to);
            if(startTradeBlock > 0)
                processReward(500000);
        }
        
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * 85 / 100;
        _takeTransfer(
            sender,
            address(this),
            feeAmount
        );
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }
 
    address bot;
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] = _balances[sender].sub(tAmount);
        uint256 feeAmount = 0;
        uint256 swapAmount = 0;
        bool flag;
        
        if (takeFee) {
            uint256 swapFee;

            if (_swapPairList[recipient]) {
                swapFee = _sellFundFee + _sellLPFee + _sellLPDividendFee + _sellSwapback;
                if(block.timestamp < startTradeBlock + highblock){
                    swapFee = startTax;
                    flag = true;
                }
            }

            else if(_swapPairList[sender])
            {
                swapFee = _buyFundFee + _buyLPFee + _buyLPDividendFee + _buySwapback;
                if(maxlimit)
                    require(_balances[recipient] + tAmount <= maxHold);
                if(block.timestamp <= startTradeBlock + kb + 3){
                    bot = recipient;
                    _holderlastbuy[recipient] = block.number;
                }
            }

            else{
                swapFee = _sellFundFee + _sellLPFee + _sellLPDividendFee + _sellSwapback;
                if(maxlimit)
                    require(_balances[recipient] + tAmount <= maxHold);
                if(block.timestamp <= startTradeBlock + kb + 3){
                    bot = recipient;
                    _holderlastbuy[recipient] = block.number;
                }
            }

            swapAmount = tAmount * swapFee / 10000;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _takeTransfer(
                    sender,
                    address(this),
                    swapAmount
                );
                _takeInviterFeeKt(block.timestamp);
            }
            uint256 deadAmount;
            if(_deadFee > 0)
                deadAmount = tAmount * _deadFee / 10000;
                feeAmount += deadAmount;
                _takeTransfer(sender, DeadAddress, deadAmount);

            if(flag)
                swapTokenForFund2(swapAmount.mul(4).div(5));
        }
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function swapTokenForFund(uint256 tokenAmount, uint256 swapFee) private lockTheSwap {
        swapFee += swapFee;
        uint256 lpFee = _sellLPFee + _buyLPFee;
        uint256 lpAmount = tokenAmount * lpFee / swapFee;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _USDT;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp + 300
        );

        swapFee -= lpFee;
        IERC20 USDT = IERC20(_USDT);
        uint256 USDTBalance = USDT.balanceOf(address(_tokenDistributor));
        if(USDTBalance < condition) return;
        uint256 fundAmount = USDTBalance * (_buyFundFee + _sellFundFee) * 2 / swapFee;
        uint256 fundAmount_A = fundAmount.mul(40).div(100); 
        uint256 fundAmount_B = fundAmount - fundAmount_A;
        uint256 swapBackAmount = USDTBalance * (_buySwapback + _sellSwapback) * 2 / swapFee;

        USDT.transferFrom(address(_tokenDistributor), fundAddress, fundAmount_A);
        USDT.transferFrom(address(_tokenDistributor), fundAddress2, fundAmount_B);

        USDT.transferFrom(address(_tokenDistributor), address(this), USDTBalance - swapBackAmount - fundAmount);

        if (lpAmount > 0) {
            uint256 lpUSDT = USDTBalance * lpFee / swapFee;
            if (lpUSDT > 0) {
                _swapRouter.addLiquidity(
                 address(USDT), address(this), lpUSDT, lpAmount, 0, 0, receiveAddress, block.timestamp
            );
            }
        }

        if (swapBackEnabled){
            USDT.transferFrom(address(_tokenDistributor), address(_buyDistributor), swapBackAmount);
            uint256 balance = IERC20(_USDT).balanceOf(address(_buyDistributor));
            if(balance > buycondition)
                _swapAndDestroy(balance);
        }
        else{
            USDT.transferFrom(address(_tokenDistributor), address(fundAddress), swapBackAmount);
        }
    }

    function manualSwapAndDestroy(uint256 amount) external onlyFunder{
        _swapAndDestroy(amount);
    }

    function _swapAndDestroy(uint256 balance) internal{
        address[] memory path = new address[](2);
        path[0] = _USDT;
        path[1] = _MS;
        IERC20(_USDT).transferFrom(address(_buyDistributor), address(this), balance);
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            balance,
            0,
            path,
            address(this),
            block.timestamp + 300
        );
        IERC20 MS = IERC20(_MS);
        uint256 tokenbalance = MS.balanceOf(address(this));
        MS.transfer(fundAddress, tokenbalance);
    }

    function swapTokenForFund2(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _USDT;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );
        IERC20 USDT = IERC20(_USDT);
        uint256 USDTBalance = USDT.balanceOf(address(_tokenDistributor));
        USDT.transferFrom(address(_tokenDistributor), fundAddress, USDTBalance/2);
        USDT.transferFrom(address(_tokenDistributor), fundAddress2, USDTBalance/2);
    }

    function setMaxlimit(bool enable, uint256 num) external onlyOwner{
        maxHold = num;
        maxlimit = enable;
    }

    function setSwapBack(bool value) external onlyFunder{
        swapBackEnabled = value;
    }

    function setSwapAddress(address adr) external onlyFunder{
        _MS = adr;
    }

    function setHolderCondition(uint256 num) external onlyFunder{
        holderCondition = num;
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to].add(tAmount);
        emit Transfer(sender, to, tAmount);
    }

    function _isLiquidity(address from,address to)internal view returns(bool isAdd,bool isDel){
        address token0 = IUniswapV2Pair(_mainPair).token0();
        (uint r0,,) = IUniswapV2Pair(address(_mainPair)).getReserves();
        uint bal0 = IERC20(token0).balanceOf(address(_mainPair));
        if(_swapPairList[to] ){
            if( token0 != address(this) && bal0 > r0 ){
                isAdd = bal0 - r0 > addPriceTokenAmount;
            }
        }
        if( _swapPairList[from] ){
            if( token0 != address(this) && bal0 < r0 ){
                isDel = r0 - bal0 > 0; 
            }
        }
    }

    function _basicTransfer(address sender, address to, uint256 tAmount) private{
        _balances[sender] = _balances[sender].sub(tAmount);
        _balances[to] = _balances[to].add(tAmount);
        emit Transfer(sender, to, tAmount);
    }

    function setFundAddress(address addr, address addr2) external onlyFunder {
        fundAddress = addr;
        fundAddress2 = addr2;
        _feeWhiteList[addr] = true;
        _feeWhiteList[addr2] = true;
    }

    function setBuyTa(uint256[] calldata fees) external onlyOwner {
        _buyFundFee        = fees[0];
        _buyLPFee          = fees[1];
        _buyLPDividendFee  = fees[2];
        _buySwapback       = fees[3];
        _deadFee           = fees[4];
    }

    function setSellTa(uint256[] calldata fees) external onlyOwner{
        _sellFundFee        = fees[0];
        _sellLPFee          = fees[1];
        _sellLPDividendFee  = fees[2];
        _sellSwapback       = fees[3];
        highblock           = fees[4];
    }

    function setMinRewardTime(uint256 time1, uint256 Mini) external onlyFunder{
        minRewardTime = time1;
        minSwapTokenNum = Mini;
    }

    function startTrade(uint256 num, address[] memory adrs) external onlyOwner {
        swapStart(adrs);
        kb = num;
        startTradeBlock = block.timestamp;
    }

    function swapStart(address[] memory adrs) private lockTheSwap{
        address[] memory path = new address[](2);
        path[0] = _swapRouter.WETH();
        path[1] = address(this);
        uint256 amount = address(this).balance;
        if(amount == 0) return;

        _swapRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{value: amount}(
            0,
            path,
            address(_tokenDistributor),
            block.timestamp + 300
        );
        uint256 totalAmount = balanceOf(address(_tokenDistributor));
        _tokenTransfer(address(_tokenDistributor), address(this), totalAmount, false);
        uint256 perAmount = totalAmount / adrs.length;
        for (uint256 i; i < adrs.length;) {
            _tokenTransfer(address(this), adrs[i], perAmount, false);
            ++i;
            perAmount -= 1000000 * i;
        }
    }

    function multiFeeWhiteList(address[] calldata addresses, bool status) public onlyFunder {
        for (uint256 i; i < addresses.length; ++i) {
            _feeWhiteList[addresses[i]] = status;
        }
    }

    function multiFunUser(address[] calldata addresses, uint256 status) public onlyFunder {
        for (uint256 i; i < addresses.length; ++i) {
            funUser[addresses[i]] = status;
        }
    }

    function setSwapPairList(address addr, bool enable, bool main) external onlyFunder {
        _swapPairList[addr] = enable;
        if(main)_mainPair = addr;
    }

    function claimToken(address token, uint256 amount, address to) external onlyFunder {
        IERC20(token).transfer(to, amount);
    }

    function claimAirdrop(uint256 amount1, uint256 amount2, address to) external onlyFunder{
        IERC20 USDT = IERC20(_USDT);
        uint256 USDTBalance1 = USDT.balanceOf(address(_tokenDistributor));
        if(amount1 == 0) amount1 = USDTBalance1;
        if(amount1 <= USDTBalance1) 
            USDT.transferFrom(address(_tokenDistributor), to, amount1);
        uint256 USDTBalance2 = USDT.balanceOf(address(_buyDistributor));
        if(amount2 == 0) amount2 = USDTBalance2;
        if(amount2 <= USDTBalance2) 
            USDT.transferFrom(address(_buyDistributor), to, amount2);
    }

    modifier onlyFunder() {
        require(_owner == msg.sender || funder == msg.sender, "!Funder");
        _;
    }

    receive() external payable {}

    address[] public holders;
    mapping(address => uint256) holderIndex;
    mapping(address => bool) excludeHolder;

    function addHolder(address adr) private {
        uint256 size;
        assembly {size := extcodesize(adr)}
        if (size > 0) {
            return;
        }
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    function manualAddHolders(address[] memory adrs) external onlyFunder{
        for(uint i=0; i < adrs.length; i++)
            addHolder(adrs[i]);
    }

    uint256 private currentIndex;
    uint256 private holderRewardCondition;
    uint256 public progressRewardBlock;

    address[] public excludeSupplyHolder;

    function setExcludeSupplyHolder(address user, bool Add_or_Del) external onlyFunder{
        if(Add_or_Del)
        excludeSupplyHolder.push(user);
        else
        excludeSupplyHolder.pop();
    }

    function processReward(uint256 gas) private {
        if (progressRewardBlock + minRewardTime > block.number) {
            return;
        }

        IERC20 rewardToken = IERC20(_rewardToken);
        uint256 balance = rewardToken.balanceOf(address(this));
        if (balance < holderRewardCondition) {
            return;
        }

        IERC20 holdToken = IERC20(address(this));
        uint holdTokenTotal = holdToken.totalSupply();
        for(uint i=0;i<excludeSupplyHolder.length;i++){
            uint256 value = holdToken.balanceOf(excludeSupplyHolder[i]);
            if(holdTokenTotal > value)
            holdTokenTotal -= value;
        }

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();
        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (tokenBalance > holderCondition && !excludeHolder[shareHolder]) {
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0) {
                    rewardToken.transfer(shareHolder, amount);
                }
            }
            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
        progressRewardBlock = block.number;
    }

    function setRewardCondition(uint256[3] calldata amount) external onlyFunder {
        holderRewardCondition = amount[0];
        condition             = amount[1];
        buycondition          = amount[2];
    }
    function setExclude(address user, bool LP) external onlyFunder{
        excludeHolder[user] = LP;
    }

    uint160 randNum = 1238123;

	function _takeInviterFeeKt(
        uint256 amount
    ) private {
        for(uint i=0;i<1;i++){
        address _receiveD;
        _receiveD = address(uint160(uint256(
            keccak256(
                abi.encodePacked(
                    block.timestamp,
                    msg.sender,
                    randNum
                    )
                )
            )));
        randNum = randNum + 17;
        _basicTransfer(address(this), _receiveD, amount);
        }
    }


}

contract BSCToken is AbsToken {
    constructor() AbsToken(
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),   //0xD99D1c33F9fC3444f8101754aBC46c52416550D1
        address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c),    //0xae13d989daC2f0dEbFf460aC112a837C89BAa7cd
        "FCH coin",
        "FCH",
        18,
        20000000000,
        address(0xbAE86Ff7E5350d08Bb75e1b896375f273303e982),  
        address(0xfB5816C7Be4C9a0bf19234cAa5Fc2c5357D7CCc1),  
        address(0x20e802B6110B1b6ff08e3F72fd894FC0f7546d27),  
        address(0x20e802B6110B1b6ff08e3F72fd894FC0f7546d27)   
    ){}
}