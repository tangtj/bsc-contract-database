// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval (address indexed owner, address indexed spender, uint256 value);
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

}

contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
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

}

interface IDEXFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IDEXRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

contract TestTOKEN is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string constant _name = unicode"asd";
    string constant _symbol = "asd";
    uint8 constant _decimals = 18;
    uint256 constant _totalSupply = 1_000_000 * ( 10 **_decimals );

    IDEXRouter router;
    address pair;
    bool swapEnabled;

    bool inSwap;
    modifier inSwapFees {
        inSwap = true;
        _;
        inSwap = false;
    }

    uint256 fee = 2;
    uint256 denominator = 100;
    uint256 buyCount = 0;
    uint256 preventSwap = 25;

    uint256 threshold = _totalSupply / 1000;
    uint256 maxFeeSwap = _totalSupply / 100;

    address payable marketingWallet;

    mapping (address => bool) isFeeExempt;

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) _allowances;
        
    constructor () {

        _balances[_msgSender()] = _totalSupply;
        isFeeExempt[owner()] = true;
        isFeeExempt[address(this)] = true;
        isFeeExempt[marketingWallet] = true;

        marketingWallet = payable(_msgSender());

        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

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

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        uint256 taxAmount = 0;

        if (from != owner() && to != owner()) {
            
            taxAmount = amount.mul(fee).div(denominator);

            if (from == pair && to != address(router) && ! isFeeExempt[to] ) { buyCount++; }

            uint256 contractTokenBalance = balanceOf(address(this));

            if (!inSwap && to == pair && swapEnabled && contractTokenBalance>threshold && buyCount>preventSwap) {
                swapTokensForEth(min(amount,min(contractTokenBalance,maxFeeSwap)));
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 0) { sendETHToFee(address(this).balance); }
            }

        }

        if(taxAmount > 0){
          _balances[address(this)] = _balances[address(this)].add(taxAmount);
          emit Transfer(from, address(this),taxAmount);
        }

        _balances[from] =_balances[from].sub(amount);
        _balances[to] =_balances[to].add(amount.sub(taxAmount));

        emit Transfer(from, to, amount.sub(taxAmount));
    }


    function min(uint256 a, uint256 b) private pure returns (uint256){
      return (a > b) ? b:a;
    }

    function swapTokensForEth(uint256 tokenAmount) private inSwapFees {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();
        _approve(address(this), address(router), tokenAmount);
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function sendETHToFee(uint256 amount) private {
        marketingWallet.transfer(amount);
    }

    function openTrading(uint256 tokenAmount) external payable onlyOwner() {
        require(!swapEnabled,"trading is already open");
        router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve(address(this), address(router), _totalSupply);
        pair = IDEXFactory(router.factory()).createPair(address(this), router.WETH());
        router.addLiquidityETH{value: msg.value }(address(this),tokenAmount,0,0,owner(),block.timestamp);
        IERC20(pair).approve(address(router), type(uint).max);
        swapEnabled = true;
    }

    receive() external payable {}

}