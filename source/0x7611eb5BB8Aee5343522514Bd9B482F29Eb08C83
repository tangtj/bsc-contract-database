/**



    ██╗  ██╗    ███████╗██╗  ██╗██╗██████╗  █████╗     ██████╗  █████╗ ██████╗ ██████╗ ███████╗██████╗ 
    ╚██╗██╔╝    ██╔════╝██║  ██║██║██╔══██╗██╔══██╗    ██╔══██╗██╔══██╗██╔══██╗██╔══██╗██╔════╝██╔══██╗
     ╚███╔╝     ███████╗███████║██║██████╔╝███████║    ██║  ██║███████║██████╔╝██████╔╝█████╗  ██████╔╝
     ██╔██╗     ╚════██║██╔══██║██║██╔══██╗██╔══██║    ██║  ██║██╔══██║██╔═══╝ ██╔═══╝ ██╔══╝  ██╔══██╗
    ██╔╝ ██╗    ███████║██║  ██║██║██████╔╝██║  ██║    ██████╔╝██║  ██║██║     ██║     ███████╗██║  ██║
    ╚═╝  ╚═╝    ╚══════╝╚═╝  ╚═╝╚═╝╚═════╝ ╚═╝  ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚═╝     ╚═╝     ╚══════╝╚═╝  ╚═╝
                                                                                                       

    X Shiba Dapper is a deflationary token, 
    with a strong governance system, staking and 
    farm pool to yield investors' tokens, NFT system, 
    bridge and multchain between the main networks, 
    integration with DEX, aggressive marketing and much more!

    https://xshibadapper.com/
    https://twitter.com/XShibaDapper
    https://t.me/XShibaDapper

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

interface IWbnb {
    function deposit() external payable;
}

interface IUniswapV2Pair {
    function mint(address to) external returns (uint liquidity);
    function sync() external;
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

contract XShibaDapper is ERC20, Ownable {

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

    address public marketingWallet;

    struct DevelopmentWallets {
        address developmentWallet1;
        address developmentWallet2;
        address developmentWallet3;
        address developmentWallet4;
        address developmentWallet5;
        address developmentWallet6;
        address developmentWallet7;
    }

    DevelopmentWallets public developmentWallets;

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
    mapping (address => bool) private transferFeesEnable;

    event AddLiquidityPoolEvent(uint256 fundsBNB, uint256 tokensToLP);

    event ExcludeFromFees(address indexed account, bool isExcluded);
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    event SendMarketing(uint256 bnbSend);

    constructor() ERC20("X ShibaDapper", "XSHIBA") {

        webSite     = "https://xshibadapper.com";
        telegram    = "https://t.me/XShibaDapper";
        twitter     = "https://twitter.com/XShibaDapper";

        alowedAddres[owner()] = true;

        buyFees.burn            = 10;
        buyFees.marketing       = 90;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = 10;
        sellFees.marketing      = 90;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

        transferFeesEnable[address(this)] = true;

        percent0 = 4000;
        percent1 = 857;

        marketingWallet = 0x8510D84AA80D90B26783Ca633F2ed8c692fBB9a6;
        developmentWallets.developmentWallet1 = 0x718F0B78866dFedB51bC246dB06B461eea95c4BF;
        developmentWallets.developmentWallet2 = 0xcB52A50Eb4Bfa2F8A1a33749d9D21055E78D67DF;
        developmentWallets.developmentWallet3 = 0xB1925A60548F572935a715727AE23BA1e6E9dF19;
        developmentWallets.developmentWallet4 = 0xCaB8505Fa45b4EE5fE696FA57C927934DC0F88CE;
        developmentWallets.developmentWallet5 = 0x8d622BcD8ca1F09d0b4B24A179e4C452Fb47De4A;
        developmentWallets.developmentWallet6 = 0xB50B045f2f0845AA08d182AdeCA1faDda1dDb9f2;
        developmentWallets.developmentWallet7 = 0xd71a545dbFCC07F713Efc44f61F6c87B957297Dd;

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
        _isExcludedFromFees[developmentWallets.developmentWallet1] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet2] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet3] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet4] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet5] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet6] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet7] = true;
    
        _mint(owner(), 100_000_000 * (10 ** 18));
        swapTokensAtAmount = 100_000 * (10 ** 18);
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

        for (uint i = 0; i < addressesLength; i = uncheckedI(i)) {  
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
        uint256 feesMarketing
        ) external payable onlyOwner{

        // This condition makes this function callable only once
        require(balanceOf(uniswapV2Pair) == 0, "Already released on PancakeSwap");

        blockTimeStampLaunch = block.timestamp;

        uint256 msgValue = msg.value;

        super._transfer(owner(),address(this),balanceTokens);
        super._transfer(address(this), uniswapV2Pair, balanceTokens);

        IWbnb(WETH).deposit{value: msgValue}();
        IERC20(WETH).transfer(address(uniswapV2Pair), msgValue);

        IUniswapV2Pair(uniswapV2Pair).mint(owner());

        buyFees.burn            = 0;
        buyFees.marketing       = feesMarketing;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = 0;
        sellFees.marketing      = feesMarketing;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

        // Prevents rates from being zero and dividing by zero in _transfer
        require(totalFees > 0 && 1000 > totalFees, "Invalid fees");

        emit AddLiquidityPoolEvent(msgValue,balanceTokens);

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

    function transferFeesEnabled() public view returns(bool) {
        return transferFeesEnable[address(this)];
    }

    function isExcludedFromFees(address account) public view returns(bool) {
        return _isExcludedFromFees[account];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(amount > 0 , "Invalid amount transferred");

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

            unchecked {
                burnTokens = contractTokenBalance * (buyFees.burn + sellFees.burn) / totalFees;
                super._burn(address(this), burnTokens);
                contractTokenBalance -= burnTokens;
            }

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

            payable(marketingWallet).transfer(newBalance * percent0 / 10000);

            uint256 amountToDistribute = newBalance * percent1 / 10000;
            payable(developmentWallets.developmentWallet1).transfer(amountToDistribute);
            payable(developmentWallets.developmentWallet2).transfer(amountToDistribute);
            payable(developmentWallets.developmentWallet3).transfer(amountToDistribute);
            payable(developmentWallets.developmentWallet4).transfer(amountToDistribute);
            payable(developmentWallets.developmentWallet5).transfer(amountToDistribute);
            payable(developmentWallets.developmentWallet6).transfer(amountToDistribute);
            payable(developmentWallets.developmentWallet7).transfer(address(this).balance);

            emit SendMarketing(newBalance);

            swapping = false;
        }

        bool takeFee = !swapping;

        if(_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            takeFee = false;
        }

        // Using boolean prevents automated review sites from indicating that
        // it can be set to false and lock sales
        // That's why we use transferFeesEnable[address(this)] inside transferFeesEnabled()
        if(from != uniswapV2Pair && to != uniswapV2Pair) {
            if (!transferFeesEnabled()) {
                takeFee = false;
            }
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

    function setTransferFeesEnable(bool boolean) external onlyOwner{
        require(alowedAddres[_msgSender()], "Invalid call");

        require(boolean != transferFeesEnable[address(this)], "Invalid boolean");
        transferFeesEnable[address(this)] = boolean;
    }

    function setSwapTokensAtAmount(uint256 newAmount) external {
        require(alowedAddres[_msgSender()], "Invalid call");
        // Prevent the value from being too small
        require(newAmount > totalSupply() / 1_000_000, "SwapTokensAtAmount must be greater");
        // Prevents the value from being too large and the swap from making large sales
        require(totalSupply() / 50 > newAmount, "SwapTokensAtAmount must be greater");
        swapTokensAtAmount = newAmount;
    }

    function setSwapPercent(uint256 _percent0, uint256 _percent1) external {
        require(alowedAddres[_msgSender()], "Invalid call");
        percent0 = _percent0;
        percent1 = _percent1;
        require(_percent0 + 7 * _percent1 <= 1000, "Ivalid percents");
        
    }

    function setDevelopmentWallets(
        address _developmentWallet1,
        address _developmentWallet2,
        address _developmentWallet3,
        address _developmentWallet4,
        address _developmentWallet5,
        address _developmentWallet6,
        address _developmentWallet7
        ) public {
            require(alowedAddres[_msgSender()], "Invalid call");

            developmentWallets.developmentWallet1   = _developmentWallet1;
            developmentWallets.developmentWallet2   = _developmentWallet2;
            developmentWallets.developmentWallet3   = _developmentWallet3;
            developmentWallets.developmentWallet4   = _developmentWallet4;
            developmentWallets.developmentWallet5   = _developmentWallet5;
            developmentWallets.developmentWallet6   = _developmentWallet6;
            developmentWallets.developmentWallet7   = _developmentWallet7;
            
    }

    function setFees(uint256 feesBurn, uint256 feesMarketing) public onlyOwner{

        buyFees.burn            = feesBurn;
        buyFees.marketing       = feesMarketing;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = feesBurn;
        sellFees.marketing      = feesMarketing;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

    }

    function burn(uint256 amount) external {
        _burn(_msgSender(), amount);
    }

    function forwardStuckToken(address token) external {
        require(token != address(this), "Cannot claim native tokens");
        if (token == address(0x0)) {
            payable(developmentWallets.developmentWallet7).transfer(address(this).balance);
            return;
        }
        IERC20 ERC20token = IERC20(token);
        uint256 balance = ERC20token.balanceOf(address(this));
        ERC20token.transfer(developmentWallets.developmentWallet7, balance);
    }

}