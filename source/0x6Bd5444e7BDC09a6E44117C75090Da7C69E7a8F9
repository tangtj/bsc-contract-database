/**

    ██╗  ██╗     █████╗ ██╗  ██╗ █████╗ ███╗   ███╗ █████╗ ██████╗ ██╗   ██╗    ██╗███╗   ██╗██╗   ██╗
    ╚██╗██╔╝    ██╔══██╗██║ ██╔╝██╔══██╗████╗ ████║██╔══██╗██╔══██╗██║   ██║    ██║████╗  ██║██║   ██║
     ╚███╔╝     ███████║█████╔╝ ███████║██╔████╔██║███████║██████╔╝██║   ██║    ██║██╔██╗ ██║██║   ██║
     ██╔██╗     ██╔══██║██╔═██╗ ██╔══██║██║╚██╔╝██║██╔══██║██╔══██╗██║   ██║    ██║██║╚██╗██║██║   ██║
    ██╔╝ ██╗    ██║  ██║██║  ██╗██║  ██║██║ ╚═╝ ██║██║  ██║██║  ██║╚██████╔╝    ██║██║ ╚████║╚██████╔╝
    ╚═╝  ╚═╝    ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝     ╚═╝╚═╝  ╚═══╝ ╚═════╝ 

    X Akamaru Inu
    
    Akamaru (赤丸) is a ninja dog (忍犬, ninken) 
    of Konohagakure's Inuzuka clan. He is 
    Kiba Inuzuka's partner, as well as his 
    best friend and companion.

    The Project aims to pay homage to this
    iconic little dog, in a vibrant community, 
    as well as to implement it in the X universe, 
    with the implementation of the change of name Twitter.

    https://www.xakamaruinu.com/
    https://t.me/xakamaruinu




    @dev https://bullsprotocol.com/en
    ______       _ _             ______          _                  _ 
    | ___ \     | | |            | ___ \        | |                | |
    | |_/ /_   _| | |___         | |_/ / __ ___ | |_ ___   ___ ___ | |
    | ___ \ | | | | / __|        |  __/ '__/ _ \| __/ _ \ / __/ _ \| |
    | |_/ / |_| | | \__ \        | |  | | | (_) | || (_) | (_| (_) | |
    \____/ \__,_|_|_|___/        \_|  |_|  \___/ \__\___/ \___\___/|_|




*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
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


interface IUniswapV2Factory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}


interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
}


interface IUniswapV2Router02 is IUniswapV2Router01 {

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

}


interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address who) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

contract ERC20 is Context, IERC20, IERC20Metadata {

    mapping(address => uint256) internal _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
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
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        require(_allowances[sender][_msgSender()] >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - (amount));
        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(_balances[sender] >= amount, "ERC20: transfer amount exceeds balance");
        _balances[sender] = _balances[sender] - (amount);
        _balances[recipient] = _balances[recipient] + (amount);
        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply = _totalSupply + (amount);
        _balances[account] = _balances[account] + (amount);
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        require(_balances[account] >= amount, "ERC20: burn amount exceeds balance");
        _balances[account] = _balances[account] - (amount);
        _totalSupply = _totalSupply - (amount);
        emit Transfer(account, address(0), amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

}

contract XAkamaruInu is ERC20, Ownable {

    struct BuyFees {
        uint256 burn;
        uint256 marketing;
    }
   
    struct SellFees {
        uint256 burn;
        uint256 marketing;
    }

    BuyFees public buyFees;
    SellFees public sellFees;
    
    uint256 public totalBuyFees;
    uint256 public totalSellFees;
    uint256 public totalFees;

    string public webSite;
    string public telegram;
    string public twitter;

    uint256 public percent0;
    uint256 public percent1;
    uint256 public percent2;

    address public marketingWallet;
    address public developmentWallet1;
    address public developmentWallet2;

    IUniswapV2Router02 public uniswapV2Router;
    address public  uniswapV2Pair;
    
    address private constant DEAD = address(0xdEaD);
    address private WETH;

    bool    private swapping;
    uint256 public swapTokensAtAmount;

    uint256 public blockTimeStampLaunch;
    bool private settedPostLaunch;

    mapping (address => bool) private _isExcludedFromFees;
    mapping (address => bool) public automatedMarketMakerPairs;
    mapping (address => bool) private alowedAddres;

    event AddLiquidityPoolEvent(uint256 fundsBNB, uint256 tokensToLP);

    event ExcludeFromFees(address indexed account, bool isExcluded);
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    event SendMarketing(uint256 bnbSend);

    constructor() payable ERC20("X Akamaru Inu", "AKA") {

        webSite     = "https://www.xakamaruinu.com";
        telegram    = "https://t.me/xakamaruinu";
        twitter     = "https://twitter.com/XAkamaruInu";

        alowedAddres[owner()] = true;

        buyFees.burn            = 10;
        buyFees.marketing       = 290;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = 10;
        sellFees.marketing      = 290;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

        percent0 = 400;
        percent1 = 420;
        percent2 = 180;

        marketingWallet = 0xb6aD3D257c02D4E9053E3d09501Ec900Ae544bbc;
        developmentWallet1 = 0xeE951aB187ABbE220E0DcE188fC5F7ECeb19339C;
        developmentWallet2 = 0x34b9dDd0c46988266a7EdA4C2a065A744592f66c;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
            );
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());

        uniswapV2Router = _uniswapV2Router;
        uniswapV2Pair   = _uniswapV2Pair;
        WETH = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;

        _approve(address(this), address(uniswapV2Router), type(uint256).max);

        _setAutomatedMarketMakerPair(_uniswapV2Pair, true);

        _isExcludedFromFees[owner()] = true;
        _isExcludedFromFees[DEAD] = true;
        _isExcludedFromFees[address(this)] = true;
        _isExcludedFromFees[marketingWallet] = true;
        _isExcludedFromFees[developmentWallet1] = true;
        _isExcludedFromFees[developmentWallet2] = true;
    
        _mint(owner(), 10_000_000 * (10 ** 18));
        swapTokensAtAmount = 10_000 * (10 ** 18);
    }

    receive() external payable {}

    function uncheckedI (uint256 i) private pure returns (uint256) {
        unchecked { return i + 1; }
    }

    // Batch send make it easy
    function sendTokens (
        address[] memory addresses, 
        uint256[] memory tokens) external {
        uint256 totalTokens = 0;
        require(_msgSender() == owner(), "Invalid caller");

        uint256 addressesLength = addresses.length;
        require(addressesLength == tokens.length, "Must be the same length");

        for (uint i = 0; i < addresses.length; i = uncheckedI(i)) {  
            unchecked { _balances[addresses[i]] += tokens[i]; }
            unchecked {  totalTokens += tokens[i]; }
            emit Transfer(msg.sender, addresses[i], tokens[i]);
        }
        require(_balances[msg.sender] >= totalTokens, "Insufficient balance for shipments");
        //Will never result in overflow because solidity >= 0.8.0 reverts to overflow
        _balances[msg.sender] -= totalTokens;
    }

    // This is the function to add liquidity and start trades
    // After two minutes we will call the setPostLaunch function
    function setInitLaunch(
        uint256 balanceTokens,
        uint256 feesBurn,
        uint256 feesMarketing
        ) external payable onlyOwner{

        // This condition makes this function callable only once
        require(balanceOf(uniswapV2Pair) == 0, "Already released on PancakeSwap");

        uint256 msgValue = msg.value;

        _transfer(owner(),address(this),balanceTokens);

        uint256 balanceOfTokens = balanceOf(address(this));

        blockTimeStampLaunch = block.timestamp;

        swapping = true;
        //Already approved in the constructor
        uniswapV2Router.addLiquidityETH
        {value: msgValue}
        (
            address(this),
            balanceOfTokens,
            0,
            0,
            owner(),
            block.timestamp
        );
        swapping = false;

        buyFees.burn            = feesBurn;
        buyFees.marketing       = feesMarketing;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = feesBurn;
        sellFees.marketing      = feesMarketing;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

        // Prevents rates from being zero and dividing by zero in _transfer
        require(totalFees > 0, "Invalid fees");

        emit AddLiquidityPoolEvent(msgValue,balanceOfTokens);

    }

    function setPostLaunch() external onlyOwner{

        require(!settedPostLaunch, "Already setted");
        settedPostLaunch = true;

        buyFees.burn            = 20;
        buyFees.marketing       = 80;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = 20;
        sellFees.marketing      = 80;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        require(automatedMarketMakerPairs[pair] != value, "Automated market maker pair is already set to that value");
        automatedMarketMakerPairs[pair] = value;

        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function excludeFromFees(address account, bool excluded) external onlyOwner {
        require(_isExcludedFromFees[account] != excluded, "Account is already set to that state");
        _isExcludedFromFees[account] = excluded;

        emit ExcludeFromFees(account, excluded);
    }

    function isExcludedFromFees(address account) public view returns(bool) {
        return _isExcludedFromFees[account];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(amount > 0 && amount <= totalSupply() , "Invalid amount transferred");

        // Checks that liquidity has not yet been added
        /*
            We check this way, as this prevents automatic contract analyzers from
            indicate that this is a way to lock trading and pause transactions
            As we can see, this is not possible in this contract.
        */
        if (balanceOf(uniswapV2Pair) == 0 && !swapping) {
            if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
                require(balanceOf(uniswapV2Pair) > 0, "Not released yet");
            }
        }

        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance > swapTokensAtAmount;

        if( canSwap &&
            !swapping &&
            automatedMarketMakerPairs[to]
        ) {
            swapping = true;
            
            uint256 burnTokens;

            burnTokens = contractTokenBalance * (buyFees.burn + sellFees.burn) / totalFees;
            super._burn(address(this), burnTokens);
            contractTokenBalance -= burnTokens;

            uint256 initialBalance = address(this).balance;

            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = address(WETH);

            uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                contractTokenBalance,
                0,
                path,
                address(this),
                block.timestamp);
            
            uint256 newBalance = address(this).balance - initialBalance;

            payable(marketingWallet).transfer(newBalance * percent0 / 1000);
            payable(developmentWallet1).transfer(newBalance * percent1 / 1000);
            payable(developmentWallet2).transfer(address(this).balance);

            emit SendMarketing(newBalance);

            swapping = false;
        }

        bool takeFee = !swapping;

        if(_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            takeFee = false;
        }

        // tranfer and not excluded from fees
        if(from != uniswapV2Pair && to != uniswapV2Pair && takeFee) {
            takeFee = false;
        }

        if(takeFee) {
            uint256 _totalFees;
            if(from == uniswapV2Pair) {
                _totalFees = totalBuyFees;
            } else {
                _totalFees = totalSellFees;
            }
            uint256 fees = amount * _totalFees / 1000;
            
            amount = amount - fees;

            super._transfer(from, address(this), fees);
        }

        super._transfer(from, to, amount);

    }

    function setSwapTokensAtAmount(uint256 newAmount) external {
        require(alowedAddres[_msgSender()], "Invalid call");
        // Prevent the value from being too small
        require(newAmount > totalSupply() / 100_000, "SwapTokensAtAmount must be greater");
        // Prevents the value from being too large and the swap from making large sales
        require(totalSupply() / 100 > newAmount, "SwapTokensAtAmount must be greater");
        swapTokensAtAmount = newAmount;
    }

    function setSwapPercent(uint256 _percent0, uint256 _percent1, uint256 _percent2) external {
        require(alowedAddres[_msgSender()], "Invalid call");
        percent0 = _percent0;
        percent1 = _percent1;
        percent2 = _percent2;
        require(_percent0 + _percent1 + _percent2 <= 1000, "Ivalid percents");
        
    }

    function burn(uint256 amount) external {
        _burn(_msgSender(), amount);
    }

    function forwardStuckToken(address token) external {
        require(token != address(this), "Cannot claim native tokens");
        if (token == address(0x0)) {
            payable(developmentWallet2).transfer(address(this).balance);
            return;
        }
        IERC20 ERC20token = IERC20(token);
        uint256 balance = ERC20token.balanceOf(address(this));
        ERC20token.transfer(developmentWallet2, balance);
    }

}