// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, 'ReentrancyGuard: reentrant call');

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}

contract Ownable {
    address payable public _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _owner = payable(msg.sender);
        emit OwnershipTransferred(address(0), msg.sender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == msg.sender, 'Ownable: caller is not the owner');
        _;
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address payable newOwner) public onlyOwner {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface Token {
    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);

    function balanceOf(address who) external view returns (uint256);
}

contract AmpleSwap is Ownable, ReentrancyGuard {
    using SafeMath for uint;

    address public tokenAddr;
    uint256 private exchangeAmount;
    uint256 public tokenPriceUsd = 5 ether;
    uint256 public tokenPriceUsdSell = 4.5 ether;
    uint256 public decimal = 18;
    uint256 public buyFee = 5;
    uint256 public buyFeeDivisor = 1000;
    uint256 public sellFee = 5;
    uint256 public sellFeeDivisor = 1000;

    uint256 public minBuyAmount = 1 ether;
    uint256 public maxBuyAmount = 1000 ether;
    uint256 public minSellAmount = 1 ether;
    uint256 public maxSellAmount = 100000 ether;

    struct Transaction {
        uint256 amount;
        uint256 timestamp;
        uint256 price;
    }

    mapping(address => Transaction[]) public buyTransactions;
    mapping(address => Transaction[]) public sellTransactions;
    // Define a mapping to store the last sell timestamps for each user
    mapping(address => uint256) public lastSellTimestamp;

    // Add a cooldown period (in seconds), e.g., 24 hours
    uint256 public sellCooldown = 24 hours;

    event ExchangeBUSDforTokenEvent(address beneficiary, uint amount);
    event ExchangeTokenforUsdEvent(address beneficiary, uint amount);

    mapping(address => uint256) public ampleBrought;
    mapping(address => uint256) public ampleSold;
    mapping(address => bool) public stableCoins;

    constructor(address _tokenAddr) {
        tokenAddr = _tokenAddr;
    }

    receive() external payable {}

    function buy(address _stableToken, uint256 _amount) external nonReentrant {
        require(stableCoins[_stableToken], 'Not a registered Stable Coin');
        require(_amount >= minBuyAmount, 'Amount is below the minimum buy amount');
        require(_amount <= maxBuyAmount, 'Amount exceeds the maximum buy amount');

        uint256 amount = _amount;
        address userAdd = msg.sender;
        uint256 fee = 0;
        if (buyFee > 0) {
            fee = (amount.mul(buyFee)).div(buyFeeDivisor);
            amount = amount.sub(fee);
        }

        exchangeAmount = ((amount.mul(10 ** uint256(decimal)).div(tokenPriceUsd)).mul(10 ** uint256(decimal))).div(
            10 ** uint256(decimal)
        );
        require(Token(tokenAddr).balanceOf(address(this)) >= exchangeAmount, 'There is low token balance in contract');

        require(Token(_stableToken).transferFrom(userAdd, address(this), _amount));
        if (buyFee > 0) {
            require(Token(_stableToken).transfer(owner(), fee));
        }
        require(Token(tokenAddr).transfer(userAdd, exchangeAmount));

        ampleBrought[msg.sender] = ampleBrought[msg.sender].add(exchangeAmount);
        // Record the buy transaction for the user
        Transaction memory buyTx = Transaction({amount: _amount, timestamp: block.timestamp, price: tokenPriceUsd});
        buyTransactions[msg.sender].push(buyTx);
        emit ExchangeBUSDforTokenEvent(userAdd, exchangeAmount);
    }

    function sell(address _stableToken, uint256 _amount) external nonReentrant {
        require(stableCoins[_stableToken], 'Not a registered Stable Coin');
        require(_amount >= minSellAmount, 'Amount is below the minimum sell amount');
        require(_amount <= maxSellAmount, 'Amount exceeds the maximum sell amount');
        // Check if the user's cooldown period has passed
        require(block.timestamp - lastSellTimestamp[msg.sender] >= sellCooldown, 'Cooldown period not elapsed');

        uint256 amount = _amount;
        address userAdd = (msg.sender);
        uint256 fee = 0;

        exchangeAmount = (
            ((amount.mul(10 ** uint256(decimal)).mul(tokenPriceUsdSell)).mul(10 ** uint256(decimal))).div(
                10 ** uint256(decimal)
            )
        ).div(10 ** uint256(decimal * 2));

        require(
            Token(_stableToken).balanceOf(address(this)) >= exchangeAmount,
            'There is low token balance in contract'
        );

        if (sellFee > 0) {
            fee = (exchangeAmount.mul(sellFee)).div(sellFeeDivisor);
            exchangeAmount = exchangeAmount.sub(fee);
        }

        require(Token(tokenAddr).transferFrom(userAdd, address(this), _amount));
        if (sellFee > 0) {
            require(Token(_stableToken).transfer(owner(), fee));
        }
        require(Token(_stableToken).transfer(userAdd, exchangeAmount));
        
        // Update the last sell timestamp for the user
        lastSellTimestamp[msg.sender] = block.timestamp;

        ampleSold[msg.sender] = ampleSold[msg.sender].add(amount);
        // Record the sell transaction for the user
        Transaction memory sellTx = Transaction({amount: _amount, timestamp: block.timestamp, price: tokenPriceUsdSell});
        sellTransactions[msg.sender].push(sellTx);
        emit ExchangeTokenforUsdEvent(userAdd, exchangeAmount);
    }

    function updateBuyFee(uint256 _buyFee, uint256 _buyDivisor) external onlyOwner {
        buyFee = _buyFee;
        buyFeeDivisor = _buyDivisor;
    }

    function updateSellFee(uint256 _sellFee, uint256 _sellDivisor) external onlyOwner {
        sellFee = _sellFee;
        sellFeeDivisor = _sellDivisor;
    }

    function addStableToken(address _stableToken, bool _value) external onlyOwner {
        stableCoins[_stableToken] = _value;
    }

    function updateTokenStablePriceBuy(uint256 newTokenValue) external onlyOwner {
        tokenPriceUsd = newTokenValue;
    }
    function updateTokenStablePriceSell(uint256 newTokenValue) external onlyOwner {
        tokenPriceUsdSell = newTokenValue;
    }

    function updateTokenAddress(address newTokenAddr) external onlyOwner {
        tokenAddr = newTokenAddr;
    }

    function removeForeignToken(address _tokenAddr, address beneficiary) external nonReentrant onlyOwner {
        require(Token(_tokenAddr).transfer(beneficiary, Token(_tokenAddr).balanceOf(address(this))));
    }

    function removeForeignCoin(address payable beneficiary) external nonReentrant onlyOwner {
        beneficiary.transfer(address(this).balance);
    }

    function tokenBalance() public view returns (uint256) {
        return Token(tokenAddr).balanceOf(address(this));
    }

    function busdToToken(uint256 amount) external view returns (uint256) {
        return
            ((amount.mul(10 ** uint256(decimal)).div(tokenPriceUsd)).mul(10 ** uint256(decimal))).div(
                10 ** uint256(decimal)
            );
    }

    function tokenToUsd(uint256 amount) external view returns (uint256) {
        return
            (
                ((amount.mul(10 ** uint256(decimal)).mul(tokenPriceUsdSell)).mul(10 ** uint256(decimal))).div(
                    10 ** uint256(decimal)
                )
            ).div(10 ** uint256(decimal * 2));
    }

    function busdToTokenFee(uint256 amount) external view returns (uint256) {
        uint256 fee = 0;
        if (buyFee > 0) {
            fee = (amount.mul(buyFee)).div(buyFeeDivisor);
            amount = amount.sub(fee);
        }
        return
            ((amount.mul(10 ** uint256(decimal)).div(tokenPriceUsd)).mul(10 ** uint256(decimal))).div(
                10 ** uint256(decimal)
            );
    }

    function tokenToUsdFee(uint256 amount) external view returns (uint256) {
        uint256 fee = 0;

        uint256 tokenReceieved = (
            ((amount.mul(10 ** uint256(decimal)).mul(tokenPriceUsdSell)).mul(10 ** uint256(decimal))).div(
                10 ** uint256(decimal)
            )
        ).div(10 ** uint256(decimal * 2));

        if (sellFee > 0) {
            fee = (tokenReceieved.mul(sellFee)).div(sellFeeDivisor);
            tokenReceieved = tokenReceieved.sub(fee);
        }

        return tokenReceieved;
    }

    function updateMinBuyAmount(uint256 _newMinBuyAmount) external onlyOwner {
        minBuyAmount = _newMinBuyAmount;
    }

    function updateMaxBuyAmount(uint256 _newMaxBuyAmount) external onlyOwner {
        maxBuyAmount = _newMaxBuyAmount;
    }

    function updateMinSellAmount(uint256 _newMinSellAmount) external onlyOwner {
        minSellAmount = _newMinSellAmount;
    }

    function updateMaxSellAmount(uint256 _newMaxSellAmount) external onlyOwner {
        maxSellAmount = _newMaxSellAmount;
    }

    function bnbBalance() public view returns (uint256) {
        return address(this).balance;
    }
    
    function changeSellCooldown(uint256 _newCooldown) external onlyOwner {
        require(_newCooldown > 0, 'Cooldown must be greater than zero');
        sellCooldown = _newCooldown;
    }
}