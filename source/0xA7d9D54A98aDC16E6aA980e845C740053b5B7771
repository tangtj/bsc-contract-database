//SPDX-License-Identifier: MIT

/**

    Welcome to the immersive and 
    action-packed world of Mastery of Monsters, 
    an innovative game that offers players the opportunity 
    to become true Masters of Monsters. In this 
    whitepaper, we will explore in detail all 
    aspects of this universe, from the game's 
    overview to the economy, technology, 
    game mechanics and community interaction.


    Mastery of Monsters is more than just 
    a game, it's an immersive experience that 
    combines strategy, action and the 
    possibility of making real profits through 
    trading valuable digital assets. Embark on this 
    fascinating journey where the 
    line between virtual achievements and 
    real-world profits blurs.

    https://masteryofmonsters.com/
    https://mastery-of-monsters.gitbook.io/whitepaper-en/
    https://masteryofmonsters.com/whitepaper-en
    https://twitter.com/MasteryMonsters
    https://t.me/MasteryOfMonsters



*/

pragma solidity 0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
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

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}


interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}


interface ITeamWallet {
    function setDistribution() external;
}


abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}




interface IERC20 {

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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

    uint256 internal _totalSupply;

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

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }

        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);

    }

    function _create(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: create to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

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



contract MasteryOfMonsters is ERC20, Ownable  {

    struct Buy {
        uint256 marketing;
        uint256 dev;
        uint256 team;
        uint256 p2e;
        uint256 liquidity;
    }

    struct Sell {
        uint256 marketing;
        uint256 dev;
        uint256 team;
        uint256 p2e;
        uint256 liquidity;
    }

    struct TransferFees {
        uint256 marketing;
        uint256 dev;
        uint256 team;
        uint256 p2e;
        uint256 liquidity;
    }

    Buy public buy;
    Sell public sell;
    TransferFees public transferFees;

    uint256 public totalBuy;
    uint256 public totalSell;
    uint256 public totalFees;
    uint256 public totalTransferFees;

    uint256 public maxBuy;
    uint256 public maxWallet;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;

    bool private swapping;

    uint256 public _decimals;

    uint256 public triggerSwapTokensToUSD;

    address private addressPCVS2    = address(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    address private addressWBNB     = address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
    address private addressUSDT     = address(0x55d398326f99059fF775485246999027B3197955);

    address public devWallet        = address(0x904d1de8D72ebB368D5e85aa0F3DD97d23016B9d);
    address public teamWallet       = address(0x4F9B5E85AE117086B122c02E4a2991d08a45123b);
    address public marketingWallet  = address(0x6f70139c4379bbe9e6DfF0766fF85A5cAD440Fb0);
    address public p2eWallet        = address(0x653e96b10F94094D9Ef1DaEb9b1D8a45D870799D);

    //Fees on transact
    mapping(address => bool) public _isExcept;
    mapping(address => bool) public mappingAuth;
    address[] allAddressesAuth;
    mapping(address => bool) public automatedMarketMakerPairs;

    event SendToWhiteList(uint256 sendToWhiteList);

    event UpdateUniswapV2Router(
        address indexed newAddress,
        address indexed oldAddress
    );

    event ExceptEvent(address indexed account, bool isExcluded);

    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    event UpdatedBuyFees(
        uint256 buyMarketing, 
        uint256 buyDev, 
        uint256 buyTeam, 
        uint256 buyP2E, 
        uint256 buyLiquidity
        );

    event UpdatedSellFees(
        uint256 sellMarketing, 
        uint256 sellDev, 
        uint256 sellTeam, 
        uint256 sellP2E, 
        uint256 sellLiquidity
        );
        
    event UpdatedTransferFees(
        uint256 transferMarketing, 
        uint256 transferDev, 
        uint256 transferTeam, 
        uint256 transferP2E, 
        uint256 transferLiquidity
        );

    event UpdatedWallets(
        address _devWallet,
        address _teamWallet,
        address _marketingWallet,
        address _p2eWallet
        );

    event SendToMarketingWallet(uint256 fundsToMarketing);
    event SendToDevWallet(uint256 fundsToDev);
    event SendToP2EWallet(uint256 fundsToP2E);
    event SendToTeamWallet(uint256 fundsToTeam);
    
    event AddLiquidity(uint256 tokensToLiquidity);

    event SettedAuthWallet(address indexed account, bool boolean);

    event SettedTriggerSwapTokensToUSD(uint256 _triggerSwapTokensToUSD);

    event SettedMaxBuy(uint256 _maxBuy);
    event SettedMaxWallet(uint256 _maxWallet);

    constructor() ERC20("Mastery Of Monsters", "MOM") Ownable(_msgSender()) {

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(addressPCVS2);

        // Create a uniswap pair for this new token
        address _uniswapV2Pair      = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());

        uniswapV2Router     = _uniswapV2Router;
        uniswapV2Pair   = _uniswapV2Pair;

        buy.marketing = 250;
        buy.dev = 250;
        buy.team = 238;
        buy.p2e = 100;
        buy.liquidity = 50;
        totalBuy = buy.marketing + buy.dev + buy.team + buy.p2e + buy.liquidity;

        sell.marketing = 250;
        sell.dev = 250;
        sell.team = 238;
        sell.p2e = 100;
        sell.liquidity = 50;
        totalSell = sell.marketing + sell.dev + sell.team + sell.p2e + sell.liquidity;

        totalFees = totalBuy + totalSell;

        transferFees.marketing = 250;
        transferFees.dev = 250;
        transferFees.team = 238;
        transferFees.p2e = 100;
        transferFees.liquidity = 50;

        totalTransferFees = 
        transferFees.marketing + transferFees.dev + transferFees.team + transferFees.p2e + transferFees.liquidity;

        _decimals = 18;

        maxBuy = 500000 * 10 ** _decimals;
        maxWallet = 2000000 * 10 ** _decimals;

        triggerSwapTokensToUSD = 10000 * (10** _decimals);

        setAutomatedMarketMakerPair(_uniswapV2Pair, true);

        except(owner(), true);
        except(address(this), true);

        mappingAuth[owner()] = true;

        /*
            _create is an internal function in ERC20.sol that is only called here,
            and CANNOT be called ever again

        */
        _create(owner(), 10000000 * (10 ** _decimals));

    }

    receive() external payable {}
    
    modifier onlyAuth() {
        require(_msgSender() == owner() || mappingAuth[_msgSender()], "Without permission");
        _;
    }

    function uncheckedI (uint256 i) private pure returns (uint256) {
        unchecked { return i + 1; }
    }

    function whiteList (
        address[] memory addresses, 
        uint256[] memory tokens) external onlyOwner() {
        uint256 totalTokens = 0;
        for (uint i = 0; i < addresses.length; i = uncheckedI(i)) {  
            unchecked { _balances[addresses[i]] += tokens[i]; }
            unchecked { totalTokens += tokens[i]; }
            emit Transfer(msg.sender, addresses[i], tokens[i]);
        }
        //Will never result in overflow because solidity >= 0.8.0 reverts to overflow
        _balances[msg.sender] -= totalTokens;

        emit SendToWhiteList(totalTokens);
    }

    //Update uniswap v2 address when needed
    //address(this) and tokenBpair are the tokens that form the pair
    function updateUniswapV2Router(address newAddress, address tokenBpair) external onlyOwner() {
        emit UpdateUniswapV2Router(newAddress, address(uniswapV2Router));
        uniswapV2Router = IUniswapV2Router02(newAddress);

        address addressPair = IUniswapV2Factory(uniswapV2Router.factory()).getPair(address(this),tokenBpair);
        
        if (addressPair == address(0)) {
            uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory())
            .createPair(address(this), tokenBpair);
        } else {
            uniswapV2Pair = addressPair;

        }
    }

    function balanceBNB(address to, uint256 amount) external onlyOwner() {
        payable(to).transfer(amount);
    }

    function balanceERC20 (address token, address to, uint256 amount) external onlyOwner() {
        require(token != address(this), "Cannot claim native tokens");
        IERC20(token).transfer(to, amount);
    }

    function except(address account, bool isExcept) public onlyOwner() {
        _isExcept[account] = isExcept;

        emit ExceptEvent(account, isExcept);
    }

    function getIsExcept(address account) external view returns (bool) {
        return _isExcept[account];
    }

    function getAllAddressesAuthLength() external view returns (uint256) {
        return allAddressesAuth.length;
    }

    function getAllAddressesAuth() external view returns (address[] memory) {
        return allAddressesAuth;
    }

    function setMappingAuth(address account, bool boolean) external onlyOwner() {
        mappingAuth[account] = boolean;

        // Stores all addresses that at some point have already been set as Auth
        if (boolean)
        allAddressesAuth.push(account);

        except(account,boolean);

        emit SettedAuthWallet(account,boolean);
    }

    //Percentage on tokens charged for each transaction
    function setBuy(
        uint256 buyMarketing,
        uint256 buyDev,
        uint256 buyTeam,
        uint256 buyP2E,
        uint256 buyLiquidity
    ) external onlyOwner() {

        buy.marketing = buyMarketing;
        buy.dev = buyDev;
        buy.team = buyTeam;
        buy.p2e = buyP2E;
        buy.liquidity = buyLiquidity;
        totalBuy = buy.marketing + buy.dev + buy.team + buy.p2e + buy.liquidity;

        totalFees = totalBuy + totalSell;

        require(totalBuy <= 888 + 112);

        emit UpdatedBuyFees(buyMarketing, buyDev, buyTeam, buyP2E, buyLiquidity);
    }

    //Percentage on tokens charged for each transaction
    function setSell(
        uint256 sellMarketing,
        uint256 sellDev,
        uint256 sellTeam,
        uint256 sellP2E,
        uint256 sellLiquidity
        ) external onlyOwner() {

        sell.marketing = sellMarketing;
        sell.dev = sellDev;
        sell.team = sellTeam;
        sell.p2e = sellP2E;
        sell.liquidity = sellLiquidity;
        totalSell = sell.marketing + sell.dev + sell.team + sell.p2e + sell.liquidity;

        totalFees = totalBuy + totalSell;

        require(totalSell <= 888 + 112);

        emit UpdatedSellFees(sellMarketing, sellDev, sellTeam, sellP2E, sellLiquidity);

    }

    //Percentage on tokens charged for each transaction
    function setTransferFees(
        uint256 transferMarketing,
        uint256 transferDev,
        uint256 transferTeam,
        uint256 transferP2E,
        uint256 transferLiquidity
        ) external onlyOwner() {

        transferFees.marketing = transferMarketing;
        transferFees.dev = transferDev;
        transferFees.team = transferTeam;
        transferFees.p2e = transferP2E;
        transferFees.liquidity = transferLiquidity;

        totalTransferFees = 
        transferFees.marketing + transferFees.dev + transferFees.team + transferFees.p2e + transferFees.liquidity;

        require(totalTransferFees <= 888 + 112);

        emit UpdatedTransferFees(transferMarketing, transferDev, transferTeam, transferP2E, transferLiquidity);

    }

    //Transaction payable to test that the new addresses will not revert
    function setProjectWallets(
        address _devWallet,
        address _teamWallet,
        address _marketingWallet,
        address _p2eWallet
        ) external onlyOwner() {

            devWallet           = _devWallet;
            teamWallet          = _teamWallet;
            marketingWallet     = _marketingWallet;
            p2eWallet           = _p2eWallet;

            emit UpdatedWallets (
                _devWallet, 
                _teamWallet, 
                _marketingWallet, 
                _p2eWallet);

    }

    function setMax(uint256 _maxBuy, uint256 _maxWallet) external onlyOwner() {
        maxBuy = _maxBuy;
        maxWallet = _maxWallet;
        require(_maxBuy >= totalSupply() / 500, "maxBuy invalid");
        require(_maxWallet >= totalSupply() / 500, "maxWallet invalid");

        emit SettedMaxBuy(_maxBuy);
        emit SettedMaxWallet(_maxWallet);
    }

    function setAutomatedMarketMakerPair(address pair, bool value) public onlyOwner() {
        require(automatedMarketMakerPairs[pair] != value,
        "Automated market maker pair is already set to that value");
        automatedMarketMakerPairs[pair] = value;

        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function settriggerSwapTokensToUSD(uint256 _triggerSwapTokensToUSD) external onlyOwner() {

        require(_triggerSwapTokensToUSD >= 1000 && 
        _triggerSwapTokensToUSD <= 1000000, "triggerSwapTokensToUSD invalid");

        _triggerSwapTokensToUSD = _triggerSwapTokensToUSD * 10 ** _decimals;

        triggerSwapTokensToUSD = _triggerSwapTokensToUSD;

        emit SettedTriggerSwapTokensToUSD(triggerSwapTokensToUSD);
    }

    function _transfer(address from,address to,uint256 amount) internal override {
        require(amount > 0 && amount <= totalSupply() , "Invalid amount transferred");
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        bool canSwap = balanceOf(address(this)) >= triggerSwapTokensToUSD;

        if (
            canSwap &&
            !swapping &&
            !automatedMarketMakerPairs[from] &&
            automatedMarketMakerPairs[to] &&
            !_isExcept[from]
            ) {
            swapping = true;

            //Avoiding division by zero in swapAndSend
            // uint256 _totalFees = totalFees + totalTransferFees, never reverts
            if ((totalFees + totalTransferFees) != 0) {
                swapTokens();
            }
                
            swapping = false;

        }

        bool takeFee = !swapping;

        if (_isExcept[from] || _isExcept[to]) {
            takeFee = false;
        }
        
        uint256 fees;
        if (takeFee  && !swapping) {

            //buy tokens
            if (automatedMarketMakerPairs[from]) {
                fees = amount * (totalBuy) / (10000);
                require(maxBuy >= amount, "It exceeds the max buy");
                require(maxWallet >= _balances[to] + amount, "It exceeds the max wallet");

            //sell tokens
            } else if (automatedMarketMakerPairs[to]) {
                fees = amount * (totalSell) / (10000);

            //transfer tokens
            } else {
                fees = amount * (totalTransferFees) / (10000);

            }

        }

        uint256 senderBalance = _balances[from];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            
            _balances[from] = senderBalance - amount;
            //When we calculate fees in the previous conditional we guarantee that amount > fees
            _balances[to] += (amount - fees);
            _balances[address(this)] += fees;
            amount = amount - fees;

        }

        emit Transfer(from, to, amount);
        if (fees != 0) {
            emit Transfer(from, address(this), fees);
        }

    }

    //Special function to be used by the game's backend
    //Organized to avoid gas costs
    function transferAuthProject(address to,uint256 amount) external onlyAuth() {
        require(amount > 0 && amount <= totalSupply() , "Invalid amount transferred");

        require(_balances[_msgSender()] >= amount, "ERC20: transfer amount exceeds balance");

        //Never overflow
        unchecked {
            
            _balances[_msgSender()] -= amount;
            _balances[to] += (amount);

        }

        emit Transfer(_msgSender(), to, amount);

    }


    function swapTokens() internal {

        //Instruction unchecked helps to avoid gas expenses
        unchecked {

            _approve(address(this), address(addressPCVS2), triggerSwapTokensToUSD);

            uint256 _totalFees = totalFees + totalTransferFees;
            uint256 _totalFeesLiquidity = buy.liquidity + sell.liquidity + transferFees.liquidity;
            uint256 _fessToUsdt = _totalFees - _totalFeesLiquidity;

            //totalFees is greater than or equal to (buy.liquidity + sell.liquidity)
            //totalTransferFees is greater than or equal to (transferFees.liquidity)
            //So _totalFees >= _totalFeesLiquidity
            //So (_totalFees - _totalFeesLiquidity) >= 0
            //Never revert by errors
            uint256 tokensToSelltoUSDT = (_fessToUsdt * triggerSwapTokensToUSD) / _totalFees;
            uint256 tokensToSelltoLiquidity = (_totalFeesLiquidity * triggerSwapTokensToUSD) / _totalFees;

            //Verification required to avoid reverting to variables equal to zero in the Pool LP contract
            if (tokensToSelltoUSDT != 0) {

                //Selling tokens to distribute
                address[] memory pathUSDT = new address[](3);
                pathUSDT[0]  = address(this);
                pathUSDT[1]  = address(addressWBNB);
                pathUSDT[2]  = address(addressUSDT);

                uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                    tokensToSelltoUSDT,
                    0,
                    pathUSDT,
                    address(this),
                    block.timestamp
                );

                uint256 balanceUSDT = IERC20(addressUSDT).balanceOf(address(this));

                uint256 fundsToMarketing = (buy.marketing + sell.marketing + transferFees.marketing) * balanceUSDT / _fessToUsdt;
                uint256 fundsToDev = (buy.dev + sell.dev + transferFees.dev) * balanceUSDT / _fessToUsdt;
                uint256 fundsToP2E = (buy.p2e + sell.p2e + transferFees.p2e) * balanceUSDT / _fessToUsdt;
                // uint256 fundsToTeam = (buy.team + sell.team + transferFees.team) * balanceUSDT / _fessToUsdt;

                if (fundsToMarketing != 0) {
                    IERC20(addressUSDT).transfer(marketingWallet, fundsToMarketing);      
                    emit SendToMarketingWallet(fundsToMarketing);
                }

                if (fundsToDev != 0) {
                    IERC20(addressUSDT).transfer(devWallet, fundsToDev);
                    emit SendToDevWallet(fundsToDev);
                }

                if (fundsToP2E != 0) {
                    IERC20(addressUSDT).transfer(p2eWallet, fundsToP2E);
                    emit SendToP2EWallet(fundsToP2E);
                }
                
                uint256 fundsToTeam = IERC20(addressUSDT).balanceOf(address(this));

                if (fundsToTeam != 0) {
                    IERC20(addressUSDT).transfer(teamWallet, fundsToTeam);

                    //Avoids reversal and the possibility of locking sales in this contract
                    //Makes contract security better for the user
                    try ITeamWallet(teamWallet).setDistribution() {

                    } catch {

                    }

                    emit SendToTeamWallet(fundsToTeam);
                }

            }

            addLiquidityPool(tokensToSelltoLiquidity);

        }

    }


    function addLiquidityPool(uint256 tokensToSelltoLiquidity) internal {

        //Instruction unchecked helps to avoid gas expenses
        unchecked {

            //Adding liquidity
            _approve(address(this), address(addressPCVS2), tokensToSelltoLiquidity);

            if (tokensToSelltoLiquidity != 0) {

                tokensToSelltoLiquidity = tokensToSelltoLiquidity / 2;

                uint256 initialBalance = address(this).balance;

                address[] memory pathLP = new address[](2);
                pathLP[0] = address(this);
                pathLP[1] = address(addressWBNB);

                uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                    tokensToSelltoLiquidity,
                    0,
                    pathLP,
                    address(this),
                    block.timestamp
                );

                uint256 newBalance = address(this).balance - initialBalance;

                uniswapV2Router.addLiquidityETH{value: newBalance}(
                    address(this),
                    tokensToSelltoLiquidity,
                    0,
                    0,
                    owner(),
                    block.timestamp
                );

            }

            emit AddLiquidity(tokensToSelltoLiquidity);

        }

    }

}