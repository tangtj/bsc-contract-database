/**

    █████  ██    ██  █████  ███    ██  ██████  ███████ ██████  ███████ 
    ██   ██ ██    ██ ██   ██ ████   ██ ██       ██      ██   ██ ██      
    ███████ ██    ██ ███████ ██ ██  ██ ██   ███ █████   ██████  ███████ 
    ██   ██  ██  ██  ██   ██ ██  ██ ██ ██    ██ ██      ██   ██      ██ 
    ██   ██   ████   ██   ██ ██   ████  ██████  ███████ ██   ██ ███████ 

    Discover AVENGERS Fan Token
    Unleash Your Superpowers In The Marvel Universe!
                                                                    
    What Is AVANGERS?
    Welcome to the AVENGERS meme token - the ultimate 
    playground for superfans of the iconic Avengers franchise! 
    Say goodbye to boring tokens and hello to a 
    whole new world of memes, engagement, and community-driven fun.

    AVENGERS is not your typical crypto token. It's a 
    meme-powered crypto sensation that brings together 
    the best of both worlds - the excitement of 
    DeFi and the thrill of meme culture. We've crafted 
    this token with superfans like you in mind, giving you the 
    power to express your love for the Avengers in the 
    most playful and rewarding way possible.

    https://t.me/avangersio
    https://avangers.io



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
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

library SafeMathInt {
    int256 private constant MIN_INT256 = int256(1) << 255;
    int256 private constant MAX_INT256 = ~(int256(1) << 255);

    function mul(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a * b;

        // Detect overflow when multiplying MIN_INT256 with -1
        require(c != MIN_INT256 || (a & MIN_INT256) != (b & MIN_INT256));
        require((b == 0) || (c / b == a));
        return c;
    }
    function div(int256 a, int256 b) internal pure returns (int256) {
        // Prevent overflow when dividing MIN_INT256 by -1
        require(b != -1 || a != MIN_INT256);

        // Solidity already throws when dividing by 0.
        return a / b;
    }
    function sub(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a - b;
        require((b >= 0 && c <= a) || (b < 0 && c > a));
        return c;
    }
    function add(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a + b;
        require((b >= 0 && c >= a) || (b < 0 && c < a));
        return c;
    }
    function abs(int256 a) internal pure returns (int256) {
        require(a != MIN_INT256);
        return a < 0 ? -a : a;
    }
    function toUint256Safe(int256 a) internal pure returns (uint256) {
        require(a >= 0);
        return uint256(a);
    }
}


library SafeMathUint {
    function toInt256Safe(uint256 a) internal pure returns (int256) {
        int256 b = int256(a);
        require(b >= 0);
        return b;
    }
}


library IterableMapping {
    struct Map {
        address[] keys;
        mapping(address => uint) values;
        mapping(address => uint) indexOf;
        mapping(address => bool) inserted;
    }

    function get(Map storage map, address key) public view returns (uint) {
        return map.values[key];
    }

    function getIndexOfKey(Map storage map, address key) public view returns (int) {
        if(!map.inserted[key]) {
            return -1;
        }
        return int(map.indexOf[key]);
    }

    function getKeyAtIndex(Map storage map, uint index) public view returns (address) {
        return map.keys[index];
    }

    function size(Map storage map) public view returns (uint) {
        return map.keys.length;
    }

    function set(Map storage map, address key, uint val) public {
        if (map.inserted[key]) {
            map.values[key] = val;
        } else {
            map.inserted[key] = true;
            map.values[key] = val;
            map.indexOf[key] = map.keys.length;
            map.keys.push(key);
        }
    }

    function remove(Map storage map, address key) public {
        if (!map.inserted[key]) {
            return;
        }

        delete map.inserted[key];
        delete map.values[key];

        uint index = map.indexOf[key];
        uint lastIndex = map.keys.length - 1;
        address lastKey = map.keys[lastIndex];

        map.indexOf[lastKey] = index;
        delete map.indexOf[key];

        map.keys[index] = lastKey;
        map.keys.pop();
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
    using SafeMath for uint256;

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
        _balances[sender] = _balances[sender] - amount;
        _balances[recipient] = _balances[recipient] + amount;

        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply = _totalSupply + amount;
        _balances[account] = _balances[account] + amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        require(_balances[account] >= amount, "ERC20: burn amount exceeds balance");
        _balances[account] = _balances[account] - amount;
        _totalSupply = _totalSupply - amount;
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



interface DividendPayingTokenInterface {
    function dividendOf(address _owner) external view returns(uint256);
    function withdrawDividend() external;
  
    event DividendsDistributed(
        address indexed from,
        uint256 weiAmount
    );
    event DividendWithdrawn(
        address indexed to,
        uint256 weiAmount
    );
}

interface DividendPayingTokenOptionalInterface {
    function withdrawableDividendOf(address _owner) external view returns(uint256);
    function withdrawnDividendOf(address _owner) external view returns(uint256);
    function accumulativeDividendOf(address _owner) external view returns(uint256);
}

contract DividendPayingToken is ERC20, Ownable, DividendPayingTokenInterface, DividendPayingTokenOptionalInterface {
    using SafeMath for uint256;
    using SafeMathUint for uint256;
    using SafeMathInt for int256;

    uint256 constant internal magnitude = 2**128;
    uint256 internal magnifiedDividendPerShare;
    uint256 public totalDividendsDistributed;
    
    address public immutable rewardToken;
    
    mapping(address => int256) internal magnifiedDividendCorrections;
    mapping(address => uint256) internal withdrawnDividends;

    constructor(string memory _name, string memory _symbol, address _rewardToken) ERC20(_name, _symbol) { 
        rewardToken = _rewardToken;
    }

    function distributeDividends(uint256 amount) public onlyOwner{
        require(totalSupply() > 0);

        if (amount > 0) {
            magnifiedDividendPerShare = magnifiedDividendPerShare.add(
                (amount).mul(magnitude) / totalSupply()
            );
            emit DividendsDistributed(msg.sender, amount);

            totalDividendsDistributed = totalDividendsDistributed.add(amount);
        }
    }

    function withdrawDividend() public virtual override {
        _withdrawDividendOfUser(payable(msg.sender));
    }

    function _withdrawDividendOfUser(address payable user) internal returns (uint256) {
        uint256 _withdrawableDividend = withdrawableDividendOf(user);
        if (_withdrawableDividend > 0) {
            withdrawnDividends[user] = withdrawnDividends[user].add(_withdrawableDividend);
            emit DividendWithdrawn(user, _withdrawableDividend);
            bool success = IERC20(rewardToken).transfer(user, _withdrawableDividend);

            if(!success) {
                withdrawnDividends[user] = withdrawnDividends[user].sub(_withdrawableDividend);
                return 0;
            }

            return _withdrawableDividend;
        }
        return 0;
    }

    function dividendOf(address _owner) public view override returns(uint256) {
        return withdrawableDividendOf(_owner);
    }

    function withdrawableDividendOf(address _owner) public view override returns(uint256) {
        return accumulativeDividendOf(_owner).sub(withdrawnDividends[_owner]);
    }

    function withdrawnDividendOf(address _owner) public view override returns(uint256) {
        return withdrawnDividends[_owner];
    }

    function accumulativeDividendOf(address _owner) public view override returns(uint256) {
        return magnifiedDividendPerShare.mul(balanceOf(_owner)).toInt256Safe()
        .add(magnifiedDividendCorrections[_owner]).toUint256Safe() / magnitude;
    }

    function _transfer(address from, address to, uint256 value) internal virtual override {
        require(false);

        int256 _magCorrection = magnifiedDividendPerShare.mul(value).toInt256Safe();
        magnifiedDividendCorrections[from] = magnifiedDividendCorrections[from].add(_magCorrection);
        magnifiedDividendCorrections[to] = magnifiedDividendCorrections[to].sub(_magCorrection);
    }

    function _mint(address account, uint256 value) internal override {
        super._mint(account, value);

        magnifiedDividendCorrections[account] = magnifiedDividendCorrections[account]
        .sub( (magnifiedDividendPerShare.mul(value)).toInt256Safe() );
    }

    function _burn(address account, uint256 value) internal override {
        super._burn(account, value);

        magnifiedDividendCorrections[account] = magnifiedDividendCorrections[account]
        .add( (magnifiedDividendPerShare.mul(value)).toInt256Safe() );
    }

    function _setBalance(address account, uint256 newBalance) internal {
        uint256 currentBalance = balanceOf(account);

        if(newBalance > currentBalance) {
            uint256 mintAmount = newBalance.sub(currentBalance);
            _mint(account, mintAmount);
        } else if(newBalance < currentBalance) {
            uint256 burnAmount = currentBalance.sub(newBalance);
            _burn(account, burnAmount);
        }
    }
}



contract DividendTracker is Ownable, DividendPayingToken {
    using SafeMath for uint256;
    using SafeMathInt for int256;
    using IterableMapping for IterableMapping.Map;

    IterableMapping.Map private tokenHoldersMap;
    uint256 public lastProcessedIndex;

    mapping (address => bool) public excludedFromDividends;
    mapping (address => uint256) public lastClaimTimes;

    uint256 public claimWait;
    uint256 public minimumTokenBalanceForDividends;

    event ExcludeFromDividends(address indexed account);
    event ClaimWaitUpdated(uint256 indexed newValue, uint256 indexed oldValue);

    event Claim(address indexed account, uint256 amount, bool indexed automatic);

    constructor(uint256 minBalance, address _rewardToken) DividendPayingToken(
        "Avangers Tracker", "AVGRS Tracker", _rewardToken
        ) {
        claimWait = 3600;
        minimumTokenBalanceForDividends = minBalance * 10 ** 18;
    }

    function _transfer(address, address, uint256) internal pure override {
        require(false, "No transfers allowed");
    }

    function withdrawDividend() public pure override {
        require(false, "withdrawDividend disabled. Use the 'claim' function on the main contract.");
    }

    function updateMinimumTokenBalanceForDividends(uint256 _newMinimumBalance) external onlyOwner {
        require(_newMinimumBalance != minimumTokenBalanceForDividends, "New mimimum balance for dividend cannot be same as current minimum balance");
        minimumTokenBalanceForDividends = _newMinimumBalance;
    }

    function excludeFromDividends(address account) external onlyOwner {
        require(!excludedFromDividends[account]);
        excludedFromDividends[account] = true;

        _setBalance(account, 0);
        tokenHoldersMap.remove(account);

        emit ExcludeFromDividends(account);
    }

    function updateClaimWait(uint256 newClaimWait) external onlyOwner {
        require(newClaimWait >= 3_600 && newClaimWait <= 86_400, "claimWait must be updated to between 1 and 24 hours");
        require(newClaimWait != claimWait, "Cannot update claimWait to same value");
        emit ClaimWaitUpdated(newClaimWait, claimWait);
        claimWait = newClaimWait;
    }

    function setLastProcessedIndex(uint256 index) external onlyOwner {
        lastProcessedIndex = index;
    }

    function getLastProcessedIndex() external view returns(uint256) {
        return lastProcessedIndex;
    }

    function getNumberOfTokenHolders() external view returns(uint256) {
        return tokenHoldersMap.keys.length;
    }

    function getAccount(address _account)
        public view returns (
            address account,
            int256 index,
            int256 iterationsUntilProcessed,
            uint256 withdrawableDividends,
            uint256 totalDividends,
            uint256 lastClaimTime,
            uint256 nextClaimTime,
            uint256 secondsUntilAutoClaimAvailable) {
        account = _account;

        index = tokenHoldersMap.getIndexOfKey(account);

        iterationsUntilProcessed = -1;

        if(index >= 0) {
            if(uint256(index) > lastProcessedIndex) {
                iterationsUntilProcessed = index.sub(int256(lastProcessedIndex));
            }
            else {
                uint256 processesUntilEndOfArray = tokenHoldersMap.keys.length > lastProcessedIndex ?
                                                        tokenHoldersMap.keys.length.sub(lastProcessedIndex) :
                                                        0;


                iterationsUntilProcessed = index.add(int256(processesUntilEndOfArray));
            }
        }


        withdrawableDividends = withdrawableDividendOf(account);
        totalDividends = accumulativeDividendOf(account);

        lastClaimTime = lastClaimTimes[account];

        nextClaimTime = lastClaimTime > 0 ?
                                    lastClaimTime.add(claimWait) :
                                    0;

        secondsUntilAutoClaimAvailable = nextClaimTime > block.timestamp ?
                                                    nextClaimTime.sub(block.timestamp) :
                                                    0;
    }

    function getAccountAtIndex(uint256 index)
        public view returns (
            address,
            int256,
            int256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256) {
        if(index >= tokenHoldersMap.size()) {
            return (0x0000000000000000000000000000000000000000, -1, -1, 0, 0, 0, 0, 0);
        }

        address account = tokenHoldersMap.getKeyAtIndex(index);

        return getAccount(account);
    }

    function canAutoClaim(uint256 lastClaimTime) private view returns (bool) {
        if(lastClaimTime > block.timestamp)  {
            return false;
        }

        return block.timestamp.sub(lastClaimTime) >= claimWait;
    }

    function setBalance(address payable account, uint256 newBalance) external onlyOwner {
        if(excludedFromDividends[account]) {
            return;
        }

        if(newBalance >= minimumTokenBalanceForDividends) {
            _setBalance(account, newBalance);
            tokenHoldersMap.set(account, newBalance);
        }
        else {
            _setBalance(account, 0);
            tokenHoldersMap.remove(account);
        }

        processAccount(account, true);
    }

    function process(uint256 gas) public returns (uint256, uint256, uint256) {
        uint256 numberOfTokenHolders = tokenHoldersMap.keys.length;

        if(numberOfTokenHolders == 0) {
            return (0, 0, lastProcessedIndex);
        }

        uint256 _lastProcessedIndex = lastProcessedIndex;

        uint256 gasUsed = 0;

        uint256 gasLeft = gasleft();

        uint256 iterations = 0;
        uint256 claims = 0;

        while(gasUsed < gas && iterations < numberOfTokenHolders) {
            _lastProcessedIndex++;

            if(_lastProcessedIndex >= tokenHoldersMap.keys.length) {
                _lastProcessedIndex = 0;
            }

            address account = tokenHoldersMap.keys[_lastProcessedIndex];

            if(canAutoClaim(lastClaimTimes[account])) {
                if(processAccount(payable(account), true)) {
                    claims++;
                }
            }

            iterations++;

            uint256 newGasLeft = gasleft();

            if(gasLeft > newGasLeft) {
                gasUsed = gasUsed.add(gasLeft.sub(newGasLeft));
            }

            gasLeft = newGasLeft;
        }

        lastProcessedIndex = _lastProcessedIndex;

        return (iterations, claims, lastProcessedIndex);
    }

    function processAccount(address payable account, bool automatic) public onlyOwner returns (bool) {
        uint256 amount = _withdrawDividendOfUser(account);

        if(amount > 0) {
            lastClaimTimes[account] = block.timestamp;
            emit Claim(account, amount, automatic);
            return true;
        }

        return false;
    }

    function rescueAnyBEP20Tokens(address _tokenAddr,address _to, uint256 amount) external onlyOwner {
        IERC20(_tokenAddr).transfer(_to, amount);
    }
}



contract Avangers is ERC20, Ownable {

    struct BuyFees {
        uint256 burn;
        uint256 marketing;
        uint256 development;
        uint256 rewards;
        uint256 nfts;
    }
   
    struct SellFees {
        uint256 burn;
        uint256 marketing;
        uint256 development;
        uint256 rewards;
        uint256 nfts;
    }

    BuyFees public buyFees;
    SellFees public sellFees;
    
    uint256 public totalBuyFees;
    uint256 public totalSellFees;
    uint256 public totalFees;

    string public webSite;
    string public telegram;

    address public projectWallet;
    address public nftsAddress;

    struct DevelopmentWallets {
        address developmentWallet1;
        address developmentWallet2;
        address developmentWallet3;
        address developmentWallet4;
        address developmentWallet5;
        address developmentWallet6;
        address developmentWallet7;
        address developmentWallet8;
        address developmentWallet9;
        address developmentWallet10;
    }

    DevelopmentWallets public developmentWallets;

    IUniswapV2Router02 public uniswapV2Router;
    address public  uniswapV2Pair;
    address private WETH;

    uint256 public immutable blockTimestampDeploy;

    address private constant DEAD = address(0xdEaD);

    bool    private swapping;
    uint256 public swapTokensAtAmount;

    mapping (address => bool) private _isExcludedFromFees;
    mapping (address => bool) public automatedMarketMakerPairs;
    mapping (address => bool) private transferFeesEnable;

    DividendTracker public dividendTracker;
    address public immutable rewardToken;
    uint256 public gasForProcessing = 300_000;

    event ExcludeFromFees(address indexed account, bool isExcluded);

    event SettedProjectWallet(address indexed account);
    event SettedNftsAddress(address indexed account);
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);
    event SendBNB(uint256 indexed bnbSend);

    event UpdateUniswapV2Router(address indexed newAddress, address indexed oldAddress);
    event UpdateDividendTracker(address indexed newAddress, address indexed oldAddress);
    event GasForProcessingUpdated(uint256 indexed newValue, uint256 indexed oldValue);
    event SendDividends(uint256 amount);
    event ProcessedDividendTracker(
        uint256 iterations,
        uint256 claims,
        uint256 lastProcessedIndex,
        bool indexed automatic,
        uint256 gas,
        address indexed processor
    );

    constructor() ERC20("Avangers", "AVGRS") {

        blockTimestampDeploy = block.timestamp;

        webSite     = "https://avangers.io";
        telegram    = "https://t.me/avangersio";

        buyFees.burn            = 0;
        buyFees.marketing       = 300;
        buyFees.development     = 300;
        buyFees.rewards         = 300;
        buyFees.nfts            = 100;

        totalBuyFees = 
        buyFees.burn + buyFees.marketing + 
        buyFees.development + buyFees.rewards + buyFees.nfts;

        sellFees.burn           = 0;
        sellFees.marketing      = 300;
        sellFees.development    = 300;
        sellFees.rewards        = 300;
        sellFees.nfts           = 100;

        totalSellFees = 
        sellFees.burn + sellFees.marketing + 
        sellFees.development + sellFees.rewards + sellFees.nfts;

        totalFees               = totalBuyFees + totalSellFees;

        //USDT
        rewardToken = 0x55d398326f99059fF775485246999027B3197955;

        projectWallet                           = 0xd7cd7E3F2114CB93BbBde56422e60419B788B8Af;
        nftsAddress                             = 0xd7cd7E3F2114CB93BbBde56422e60419B788B8Af;

        developmentWallets.developmentWallet1   = 0x3d67f60F0cFf5121ba4298269f3677a21c189b5f;
        developmentWallets.developmentWallet2   = 0xad158b658388df8C730C5156843E13Bd9263815b;
        developmentWallets.developmentWallet3   = 0x8d3c42BAe2C1Fc0a2727CFb9BeC23CCAE4887670;
        developmentWallets.developmentWallet4   = 0x729F909e207628D40023b9999691Ea5Ff1a3817C;
        developmentWallets.developmentWallet5   = 0x81C09d5D523023FE423322038914A9652bBB6E20;
        developmentWallets.developmentWallet6   = 0x704e2C96e3833C1776b57d93c686b726a3ee0899;
        developmentWallets.developmentWallet7   = 0xA5976a2751a50E3437CbD663244c9b60D2646F45;
        developmentWallets.developmentWallet8   = 0x8312904d0B0b0D43E4994f9835430Cbfc604bA4b;
        developmentWallets.developmentWallet9   = 0x18D653f4db6Fe08A20c9d22955350387B590f6fa;
        developmentWallets.developmentWallet10  = 0xB538Db147eE944A23a693a3FBa296C92856c779A;

        dividendTracker = new DividendTracker(500 * 10 ** 2, rewardToken);

        WETH = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
            );
            
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(
            address(this), WETH
            );

        uniswapV2Router = _uniswapV2Router;
        uniswapV2Pair   = _uniswapV2Pair;

        _approve(address(this), address(uniswapV2Router), type(uint256).max);

        _setAutomatedMarketMakerPair(_uniswapV2Pair, true);

        dividendTracker.excludeFromDividends(address(dividendTracker));
        dividendTracker.excludeFromDividends(address(this));
        dividendTracker.excludeFromDividends(DEAD);
        dividendTracker.excludeFromDividends(address(_uniswapV2Router));
        dividendTracker.excludeFromDividends(owner());
        dividendTracker.excludeFromDividends(projectWallet);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet1);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet2);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet3);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet4);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet5);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet6);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet7);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet8);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet9);
        dividendTracker.excludeFromDividends(developmentWallets.developmentWallet10);

        transferFeesEnable[address(this)] = true;

        _isExcludedFromFees[owner()] = true;
        _isExcludedFromFees[DEAD] = true;
        _isExcludedFromFees[address(this)] = true;
        _isExcludedFromFees[projectWallet] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet1] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet2] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet3] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet4] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet5] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet6] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet7] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet8] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet9] = true;
        _isExcludedFromFees[developmentWallets.developmentWallet10] = true;
    
        _mint(owner(), 500 * 10 ** 6 * (10 ** 18));
        swapTokensAtAmount = 500_000 * (10 ** 18);
    }

    receive(

    ) external payable {

    }

    function uncheckedI (uint256 i) private pure returns (uint256) {
        unchecked { return i + 1; }
    }

    // Sending tokens from the venture capital phase
    // Batch send make it easy
    function ventureCapital (
        address[] memory addresses, 
        uint256[] memory tokens) external onlyOwner() {
        uint256 totalTokens = 0;

        require(addresses.length == tokens.length, "Must be the same length");

        for (uint i = 0; i < addresses.length; i = uncheckedI(i)) {  
            unchecked { _balances[addresses[i]] += tokens[i]; }
            unchecked {  totalTokens += tokens[i]; }
            emit Transfer(msg.sender, addresses[i], tokens[i]);
        }
        require(_balances[msg.sender] >= totalTokens, "Insufficient balance for shipments");
        //Will never result in overflow because solidity >= 0.8.0 reverts to overflow
        _balances[msg.sender] -= totalTokens;
    }
    
    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        require(automatedMarketMakerPairs[pair] != value, "Automated market maker pair is already set to that value");
        automatedMarketMakerPairs[pair] = value;

        if(value) {
            dividendTracker.excludeFromDividends(pair);
        }

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

    function transferFeesEnabled() public view returns(bool) {
        return transferFeesEnable[address(this)];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(amount > 0, "Can't be zero");

        // Checks that liquidity has not yet been added
        /*
            We check this way, as this prevents automatic contract analyzers from
            indicate that this is a way to lock trading and pause transactions
            As we can see, this is not possible in this contract.
        */
        if (balanceOf(uniswapV2Pair) == 0) {
            if (!_isExcludedFromFees[from] && !_isExcludedFromFees[to]) {
                require(balanceOf(uniswapV2Pair) > 0, "Not released yet");
            }
        }

        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance >= swapTokensAtAmount;

        if( canSwap &&
            !swapping &&
            automatedMarketMakerPairs[to] &&
            !_isExcludedFromFees[from]
        ) {
            swapping = true;
            
            unchecked {

                uint256 burnFees = buyFees.burn + sellFees.burn;

                if(burnFees > 0 && totalFees > 0) {
                    uint256 burnTokens;
                    burnTokens = contractTokenBalance * (burnFees) / totalFees;
                    super._transfer(address(this), DEAD, burnTokens);

                    contractTokenBalance -= burnTokens;
                }

                uint256 totalFeesDistribution = 
                buyFees.marketing   + sellFees.marketing + 
                buyFees.development + sellFees.development + 
                buyFees.rewards     + sellFees.rewards + 
                buyFees.nfts        + sellFees.nfts;
                
                if (contractTokenBalance > 0 && totalFeesDistribution > 0) {

                    address[] memory path = new address[](2);
                    path[0] = address(this);
                    path[1] = address(WETH);

                    uint256 initialBalance = address(this).balance;
                    uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                        contractTokenBalance,
                        0,
                        path,
                        address(this),
                        block.timestamp);
                    uint256 newBalance = address(this).balance - initialBalance;

                    uint256 marketingFeesDistribution = 
                    newBalance * (buyFees.marketing + sellFees.marketing) / totalFeesDistribution;

                    uint256 developmentFeesDistribution = 
                    newBalance * (buyFees.development + sellFees.development) / totalFeesDistribution;

                    uint256 rewardsFeesDistribution = 
                    newBalance * (buyFees.rewards + sellFees.rewards) / totalFeesDistribution;

                    uint256 nftsFeesDistribution = 
                    newBalance * (buyFees.nfts + sellFees.nfts) / totalFeesDistribution;

                    payable(projectWallet).transfer(marketingFeesDistribution);

                    payable(nftsAddress).transfer(
                        nftsFeesDistribution
                    );
                    
                    if(rewardsFeesDistribution > 0) {
                        swapAndSendDividends(rewardsFeesDistribution);
                    }

                    developmentFeesDistribution = developmentFeesDistribution / 10;

                    payable(developmentWallets.developmentWallet1).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet2).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet3).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet4).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet5).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet6).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet7).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet8).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet9).transfer(
                        developmentFeesDistribution
                    );
                    payable(developmentWallets.developmentWallet10).transfer(
                        address(this).balance
                    );
                    
                    emit SendBNB(newBalance);
                }

            }

            swapping = false;
        }

        bool takeFee = !swapping;
        
        if(_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            takeFee = false;
        }
       
        // Using boolean prevents automated review sites from indicating that
        // it can be set to false and lock sales
        // That's why we use transferFeesEnable[address(this)] inside transferFeesEnabled()
        bool isTransfer;
        if(from != uniswapV2Pair && to != uniswapV2Pair) {
            if (!transferFeesEnabled()) {
                takeFee = false;
            }
            isTransfer = true;
        }

        if(takeFee) {
            uint256 _totalFees;
            if(from == uniswapV2Pair) {
                _totalFees = totalBuyFees;

            // sell and transfer
            } else {
                _totalFees = totalSellFees;
            }

            uint256 fees = amount * _totalFees / 10000;
            
            amount = amount - fees;

            super._transfer(from, address(this), fees);
        }

        super._transfer(from, to, amount);

        try dividendTracker.setBalance(payable(from), balanceOf(from)) {} catch {}
        try dividendTracker.setBalance(payable(to), balanceOf(to)) {} catch {}

        if(!swapping && buyFees.rewards + sellFees.rewards > 0 && !isTransfer) {

            uint256 gas = gasForProcessing;

            try dividendTracker.process(gas) 
                returns (uint256 iterations, uint256 claims, uint256 lastProcessedIndex) {
                emit ProcessedDividendTracker(
                    iterations, claims, lastProcessedIndex, true, gas, tx.origin
                    );
            }
            catch {

            }
        }

    }

    function swapAndSendDividends(uint256 amount) private{
        address[] memory path = new address[](2);
        path[0] = address(WETH);
        path[1] = address(rewardToken);

        uniswapV2Router.swapExactETHForTokensSupportingFeeOnTransferTokens{value: amount}(
            0,
            path,
            address(this),
            block.timestamp
        );
        
        uint256 balanceRewardToken = IERC20(rewardToken).balanceOf(address(this));
        bool success = IERC20(rewardToken).transfer(address(dividendTracker), balanceRewardToken);

        if (success) {
            dividendTracker.distributeDividends(balanceRewardToken);
            emit SendDividends(balanceRewardToken);
        }

    }

    function updateClaimWait(uint256 newClaimWait) external onlyOwner {
        require(newClaimWait >= 3_600 && newClaimWait <= 86_400, "claimWait must be updated to between 1 and 24 hours");
        dividendTracker.updateClaimWait(newClaimWait);
    }

    function getClaimWait() external view returns(uint256) {
        return dividendTracker.claimWait();
    }

    function getTotalDividendsDistributed() external view returns (uint256) {
        return dividendTracker.totalDividendsDistributed();
    }

    function withdrawableDividendOf(address account) public view returns(uint256) {
        return dividendTracker.withdrawableDividendOf(account);
    }

    function dividendTokenBalanceOf(address account) public view returns (uint256) {
        return dividendTracker.balanceOf(account);
    }

    function totalRewardsEarned(address account) public view returns (uint256) {
        return dividendTracker.accumulativeDividendOf(account);
    }

    function excludeFromDividends(address account) external onlyOwner{
        dividendTracker.excludeFromDividends(account);
    }

    function getAccountDividendsInfo(address account)
        external view returns (
            address,
            int256,
            int256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256) {
        return dividendTracker.getAccount(account);
    }

    function getAccountDividendsInfoAtIndex(uint256 index)
        external view returns (
            address,
            int256,
            int256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256) {
        return dividendTracker.getAccountAtIndex(index);
    }

    function processDividendTracker(uint256 gas) external {
        (uint256 iterations, uint256 claims, uint256 lastProcessedIndex) = dividendTracker.process(gas);
        emit ProcessedDividendTracker(iterations, claims, lastProcessedIndex, false, gas, tx.origin);
    }

    function claim() external {
        dividendTracker.processAccount(payable(msg.sender), false);
    }

    function claimAddress(address claimee) external onlyOwner {
        dividendTracker.processAccount(payable(claimee), false);
    }

    function getLastProcessedIndex() external view returns(uint256) {
        return dividendTracker.getLastProcessedIndex();
    }

    function setLastProcessedIndex(uint256 index) external onlyOwner {
        dividendTracker.setLastProcessedIndex(index);
    }

    function getNumberOfDividendTokenHolders() external view returns(uint256) {
        return dividendTracker.getNumberOfTokenHolders();
    }

    function setSwapTokensAtAmount(uint256 newAmount) external onlyOwner{
        require(newAmount < totalSupply() / 100, "swapTokensAtAmount invalid");
        swapTokensAtAmount = newAmount;
    }

    // Prevents funds loss of funds when contract is renounced
    function getTokensDividendTracker(address _tokenAddr) external {
        require(blockTimestampDeploy + 360 days <= block.timestamp, "Before the allowed time");

        dividendTracker.rescueAnyBEP20Tokens(_tokenAddr, developmentWallets.developmentWallet10, 
            IERC20(_tokenAddr).balanceOf(address(dividendTracker))
        );
        
    }

    function setProjectWallet(address _projectWallet) external onlyOwner{
        projectWallet   = _projectWallet;
        _isExcludedFromFees[projectWallet] = true;

        emit SettedProjectWallet(_projectWallet);
        emit ExcludeFromFees(_projectWallet, true);
    }

    function setNftsAddress(address _nftsAddress) external onlyOwner {
        nftsAddress = _nftsAddress;
        _isExcludedFromFees[_nftsAddress] = true;

        emit SettedNftsAddress(_nftsAddress);
        emit ExcludeFromFees(_nftsAddress, true);
    }

    function setDevelopmentWallets(
        address _developmentWallet1,
        address _developmentWallet2,
        address _developmentWallet3,
        address _developmentWallet4,
        address _developmentWallet5,
        address _developmentWallet6,
        address _developmentWallet7,
        address _developmentWallet8,
        address _developmentWallet9,
        address _developmentWallet10
        ) public  onlyOwner{

            developmentWallets.developmentWallet1   = _developmentWallet1;
            developmentWallets.developmentWallet2   = _developmentWallet2;
            developmentWallets.developmentWallet3   = _developmentWallet3;
            developmentWallets.developmentWallet4   = _developmentWallet4;
            developmentWallets.developmentWallet5   = _developmentWallet5;
            developmentWallets.developmentWallet6   = _developmentWallet6;
            developmentWallets.developmentWallet7   = _developmentWallet7;
            developmentWallets.developmentWallet8   = _developmentWallet8;
            developmentWallets.developmentWallet9   = _developmentWallet9;
            developmentWallets.developmentWallet10  = _developmentWallet10;
    }

    function setFees(
        uint256 buyFeesBurn,
        uint256 buyFeesMarketing,
        uint256 buyFeesDevelopment,
        uint256 buyFeesRewards,
        uint256 buyFeesNfts,
        uint256 sellFeesBurn,
        uint256 sellFeesMarketing,
        uint256 sellFeesDevelopment,
        uint256 sellFeesRewards,
        uint256 sellFeesNfts
        ) public  onlyOwner{

            buyFees.burn            = buyFeesBurn;
            buyFees.marketing       = buyFeesMarketing;
            buyFees.development     = buyFeesDevelopment;
            buyFees.rewards         = buyFeesRewards;
            buyFees.nfts            = buyFeesNfts;

            totalBuyFees = 
            buyFees.burn + buyFees.marketing + 
            buyFees.development + buyFees.rewards+ buyFees.nfts;

            sellFees.burn           = sellFeesBurn;
            sellFees.marketing      = sellFeesMarketing;
            sellFees.development    = sellFeesDevelopment;
            sellFees.rewards        = sellFeesRewards;
            sellFees.nfts           = sellFeesNfts;

            totalSellFees = 
            sellFees.burn + sellFees.marketing + 
            sellFees.development + sellFees.rewards + sellFees.nfts;

            totalFees               = totalBuyFees + totalSellFees;

            require(totalBuyFees     <= 1000, "Total fees BUY must be less than 1000");
            require(totalSellFees    <= 1000, "Total fees SELL must be less than 1000");

    }

    function setTransferFeesEnable(bool boolean) external onlyOwner{
        require(boolean != transferFeesEnable[address(this)], "Invalid boolean");
        transferFeesEnable[address(this)] = boolean;
    }

    // Prevents loss of funds when contract is renounced
    function forwardStuckToken(address token) external {
        require(token != address(this), "Cannot claim native tokens");
        if (token == address(0x0)) {
            payable(developmentWallets.developmentWallet10).transfer(address(this).balance);
            return;
        }
        IERC20 ERC20token = IERC20(token);
        uint256 balance = ERC20token.balanceOf(address(this));
        ERC20token.transfer(developmentWallets.developmentWallet10, balance);
    }

}