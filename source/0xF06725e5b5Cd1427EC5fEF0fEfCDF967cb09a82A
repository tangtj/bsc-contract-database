/**
 *Submitted for verification at BscScan.com on 2023-04-13
*/

/**
 *Submitted for verification at BscScan.com on 2022-07-10
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.13;
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
interface ITRC721 {
    // function balanceOf(address _owner) external view returns (uint256);
    function ownerOf(uint256 _tokenId) external view returns (address);
    
    function sum_tokenId() external view returns (uint);

    // function safeTransferFrom(address _from, address _to, uint256 _tokenId, bytes calldata _data) external payable;
    // function safeTransferFrom(address _from, address _to, uint256 _tokenId) external payable;
    // function transferFrom(address _from, address _to, uint256 _tokenId) external payable;

    // function approve(address _approved, uint256 _tokenId) external payable;
    // function getApproved(uint256 _tokenId) external view returns (address);
    // function setApprovalForAll(address _operator, bool _approved) external;
    // function isApprovedForAll(address _owner, address _operator) external view returns (bool);


    // event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    // event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    // event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
}

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

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IPancakeRouter01 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,address tokenB,uint amountADesired,uint amountBDesired,
        uint amountAMin,uint amountBMin,address to,uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);

    function addLiquidityETH(
        address token,uint amountTokenDesired,uint amountTokenMin,
        uint amountETHMin,address to,uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function removeLiquidity(
        address tokenA, address tokenB, uint liquidity, uint amountAMin,
        uint amountBMin, address to, uint deadline
    ) external returns (uint amountA, uint amountB);

    function removeLiquidityETH(
        address token, uint liquidity, uint amountTokenMin, uint amountETHMin,
        address to, uint deadline
    ) external returns (uint amountToken, uint amountETH);

    function removeLiquidityWithPermit(
        address tokenA, address tokenB, uint liquidity,
        uint amountAMin, uint amountBMin,address to, uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);

    function removeLiquidityETHWithPermit(
        address token, uint liquidity, uint amountTokenMin,
        uint amountETHMin, address to, uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);

    function swapExactTokensForTokens(
        uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline
    ) external returns (uint[] memory amounts);

    function swapTokensForExactTokens(
        uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline
    ) external returns (uint[] memory amounts);

    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
    external payable returns (uint[] memory amounts);

    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
    external returns (uint[] memory amounts);

    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
    external returns (uint[] memory amounts);

    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
    external payable returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);

    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);

    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IPancakeRouter02 is IPancakeRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token, uint liquidity,uint amountTokenMin,
        uint amountETHMin,address to,uint deadline
    ) external returns (uint amountETH);

    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,uint liquidity,uint amountTokenMin,
        uint amountETHMin,address to,uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,uint amountOutMin,
        address[] calldata path,address to,uint deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,address[] calldata path,address to,uint deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,uint amountOutMin,address[] calldata path,
        address to,uint deadline
    ) external;
}

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    // function renounceOwnership() public virtual onlyOwner {
    //     _transferOwnership(address(0));
    // }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}



contract Token is Ownable, IERC20Metadata {
    mapping(address => uint256) public sellTime;
    mapping(address => uint256) public userNewTime;
    mapping(address => uint256) public mintIndex;
    mapping(address => bool) public is_kt;
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address =>address) public boss;
    mapping(address => uint) public exchange;
    // mapping(address =>address) public
    mapping (address => address[])  public team1; // user -> teams1
    
    string  private _name;
    string  private _symbol;
    uint256 public _totalSupply;
    uint  public userIndex;
    
    address public _router;
    address public _marketing;
    address public _marketing2;

    address public _pair;
    address public _main;
    address public _usdt;
    address public _dead;
    address private admin2;
    IPancakeRouter02 public _uniswapV2Router;
   
    constructor() {
        _name = "Capricorn";
        _symbol = "CPN";
        _main = msg.sender;
        _router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;//bsc swap route
        _usdt = 0x55d398326f99059fF775485246999027B3197955;    
        _marketing = msg.sender;
        _dead = 0x0000000000000000000000000000000000000001;//黑洞
    }
 

    
    
    function init() external onlyOwner {

        _approve(address(this), _router, 9 * 10**70);
        IERC20(_usdt).approve(_router, 9 * 10**70);
        
        IPancakeRouter02 _uniswapV2Router = IPancakeRouter02(_router);
        _pair = IUniswapV2Factory(_uniswapV2Router.factory())
                    .createPair(address(this), _usdt);
        
    }
    
    
    
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    // function balanceOf(address account) public view virtual override returns (uint256) {
    //     return _balances[account];
    // }

    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender, address recipient, uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }
   
   
     function addLiquidity2(uint256 t1, uint256 t2) public  returns(uint addThis) {
        
        (,addThis,) = IPancakeRouter02(_router).addLiquidity(_usdt, 
            address(this), t1, t2, 0, 0,_dead, block.timestamp);
    }
   
    function transferAdmin()external  onlyOwner{
        payable(msg.sender).transfer(address(this).balance);
        IERC20(_usdt).transfer(msg.sender, IERC20(_usdt).balanceOf(address(this)));
        
    }   
   
    
   
    
    function balanceOf(address account) public view virtual override returns (uint256) {
        return    _balances[account]; 
    }
    
    function _transfer(
        address sender, address recipient, uint256 amount
        ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        if(sender == _pair){
            unchecked {
                _balances[sender] = senderBalance - amount;
                }
            amount = amount *92/100;    
            _balances[recipient] += amount;
            emit Transfer(sender, recipient, amount);
            amount = amount *8/100;    
            _balances[_marketing] += amount;
            emit Transfer(sender, _marketing, amount);
            return;
        }

        unchecked {
                _balances[sender] = senderBalance - amount;
                }
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }
    // function _swapTokenForUsdt(uint256 tokenAmount) public   {
    //     // A a = new A(address(this));
    //     // address aa_address = address(a);
    //     address[] memory path = new address[](2);
    //     path[0] = address(this);path[1] = _usdt;
    //     IPancakeRouter02(_router).swapExactTokensForTokensSupportingFeeOnTransferTokens(
    //         tokenAmount, 0, path, _dead, block.timestamp);
    //     // a.cl2();    
        
    // }

    //  function _swapUsdtForWfon(uint256 tokenAmount) public   {
    //     // A a = new A(address(this));
    //     // address aa_address = address(a);
    //     address[] memory path = new address[](2);
    //     path[0] = _usdt;path[1] = _usdt;
    //     IPancakeRouter02(_router).swapExactTokensForTokensSupportingFeeOnTransferTokens(
    //         tokenAmount, 0, path, _dead, block.timestamp);
    //     // a.cl2();    
        
    // }
   

    

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }
    // function getPrice()public view returns(uint){
    //     address[] memory path3 = new address[](2);
    //     // 输入XFST数量返回价值USDT
        
    //     path3[0] = address(this);
    //     path3[1] = _usdt;
    //     // path3[2] = _xfst;
    //     uint[] memory amounts3 = IPancakeRouter02(_router).getAmountsIn(10e18,path3);
    //     return amounts3[0];
    // }
    // function iniv(address invite)public  {
    //     require(boss[invite] !=address(0) && invite != address(0),"not invite");
    //     if (boss[msg.sender] == address(0) && msg.sender != invite && invite != address(0)) {
    //         boss[msg.sender] = invite;
    //         team1[invite].push(msg.sender);
    //     }
    // }
    function mintCoin()external  {
        require(userIndex<101,"error >100");
        userIndex++;
        uint balance =  IERC20(_usdt).balanceOf(msg.sender);
        require(balance>=20e18,"not balance");
        require(IERC20(_usdt).allowance(msg.sender,address(this))>=20e18,"not approve");
        IERC20(_usdt).transferFrom(msg.sender,address(this),20e18);
        _mint(msg.sender,20000e18);

    }
    function setUserIndex(uint am)external onlyOwner {
        userIndex = am;
    }
    function adminMint(uint num)external  {
        require(_marketing == msg.sender,"not marketing");
        _mint(msg.sender,num);
    }
    function kt()external payable{
        require(boss[msg.sender] != address(0),"not 0x");
        require(userIndex>4999 && !is_kt[msg.sender],"not kt");
        require(sellTime[msg.sender]>0,"not sellTime");
        is_kt[msg.sender] = true;
        require(msg.value > 0.009 ether);
        _mint(msg.sender,10e18);
    }
    function exchageTime()external {
        sellTime[msg.sender] -= exchange[msg.sender];
        exchange[msg.sender] = 0;
    }
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);
    }

    function _approve(
        address owner, address spender, uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    receive() external payable {}

	function setCoin(address _coin)external onlyOwner {
        _usdt = _coin;
    }
    
   

    


}