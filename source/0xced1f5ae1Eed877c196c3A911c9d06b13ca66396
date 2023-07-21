// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity 0.8.19;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

// File: @openzeppelin/contracts/utils/math/SafeMath.sol


// OpenZeppelin Contracts (last updated v4.6.0) (utils/math/SafeMath.sol)

pragma solidity 0.8.19;

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 * now has built in overflow checking.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

pragma solidity 0.8.19;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

// File: Contracts/AccesReferral.sol



// Metacces Referral program Acces token sale Version 1.0

pragma solidity 0.8.19;




contract ReferralContract is ReentrancyGuard {
    using SafeMath for uint256;

    IERC20 public tokenAcces =
        IERC20(0x1E5c718b4377B5deEEF01AFf5BDC29a9528df1A3);
    IERC20 public tokenUSDT =
        IERC20(0x55d398326f99059fF775485246999027B3197955);
    IERC20 public tokenUSDC =
        IERC20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);
    IERC20 public tokenBUSD =
        IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    IERC20 public tokenTUSD =
        IERC20(0x40af3827F39D0EAcBF4A168f8D4ee67c121D11c9);

    address public owner;

    address public bigDaddy = 0xd500c150dc9b2c70E4cCeBD0fcd5163366Fcba88;
    address public developer = 0x0151899167ea32C2bF9BB148817116d49Ba0Bc9d;

    uint256 public referrersCount;
    uint256 public normalPercentage = 10;

    struct buyerInfo {
        uint256 sales;
        uint256 amountUSDT;
    }

    struct ReferrerInfo {
        uint256 salesCount;
        uint256 buyersCount;
        uint256 totalBuyersAmount;
        uint256 totalRewards;
        address[] buyersList;
        mapping(address => uint256) buyersAmount;
        mapping(address => uint256) buyerRewards;
    }

    mapping(address => bool) public isReferrer;
    mapping(address => buyerInfo) public Buyers;
    mapping(address => uint256) public referrerPercentage;
    mapping(address => address) public firstReferrerOf;
    mapping(address => ReferrerInfo) public Referrers;
    mapping(address => mapping(address => bool)) public isReferrerForBuyer;
    mapping(IERC20 => bool) public isAllowedToken;

    address[] public referrers;
    address[] public buyers;

    // 20 cents in USD's (on BSC USDT has 18 Decimals)
    uint256 public priceUSD = 200000000000000000;

    bool paused = false;

    event buyAccess(address buyer, uint256 amount, address referrer);
    event ReferralLink(address Referrer);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not Owner");
        _;
    }

    modifier notPaused() {
        require(paused != true, "Contract is paused!");
        _;
    }

    constructor() {
        owner = msg.sender;
        thisBigDaddy(bigDaddy);
        isAllowedToken[tokenUSDT] = true;
        isAllowedToken[tokenUSDC] = true;
        isAllowedToken[tokenBUSD] = true;
        isAllowedToken[tokenTUSD] = true;
    }

    function setPaused(bool _isPaused) external onlyOwner {
        paused = _isPaused;
    }

    function changeOwner(address newOwner) public onlyOwner {
        owner = newOwner;
    }

    function setTokenAllowDisallow(IERC20 _paymentToken, bool _state)
        external
        onlyOwner
    {
        isAllowedToken[_paymentToken] = _state;
    }

    function changePrice(uint256 _newPrice) external onlyOwner {
        priceUSD = _newPrice;
    }

    function setUSDT(address _USDT) external onlyOwner {
        require(_USDT != address(0), "USDT address zero");
        tokenUSDT = IERC20(_USDT);
    }

    function setUSDC(address _USDC) external onlyOwner {
        require(_USDC != address(0), "USDC address zero");
        tokenUSDC = IERC20(_USDC);
    }

    function setBUSD(address _BUSD) external onlyOwner {
        require(_BUSD != address(0), "BUSD address zero");
        tokenBUSD = IERC20(_BUSD);
    }

    function setTUSD(address _TUSD) external onlyOwner {
        require(_TUSD != address(0), "TUSD address zero");
        tokenBUSD = IERC20(_TUSD);
    }

    function setSaleReceiver(address _saleReceiver) external onlyOwner {
        require(_saleReceiver != address(0), "address zero");
        bigDaddy = _saleReceiver;
    }

    function changeNormalPercentage(uint256 _newPercentage) external onlyOwner {
        require(_newPercentage > 0, "Percentage can't be zero");
        normalPercentage = _newPercentage;
    }

    function buyWithReferral(
        address referrer,
        IERC20 paymentToken,
        uint256 amount
    ) external notPaused nonReentrant {
        require(referrer != address(0), "Referrer address zero");
        require(
            isAllowedToken[paymentToken] == true,
            "Payment token not allowed or paused! Try another payment"
        );
        require(
            paymentToken.allowance(msg.sender, address(this)) >= amount,
            "Check USDT Allowance"
        );
        require(
            paymentToken.transferFrom(msg.sender, address(this), amount),
            "Check USDT balance"
        );
        if (!isReferrer[referrer]) {
            generateReferralLink(referrer);
        }
        uint256 referralBonus = (
            amount.mul(referrerPercentage[referrer]).div(100)
        ); // 10% referral bonus
        uint256 developerAmount = amount.mul(3).div(100);
        uint256 purchaseAmount = amount.div(priceUSD).mul(1e18);
        require(
            tokenAcces.balanceOf(address(this)) >= purchaseAmount,
            "Not enough Acces"
        );

        tokenAcces.transfer(msg.sender, purchaseAmount);
        paymentToken.transfer(referrer, referralBonus);
        paymentToken.transfer(developer, developerAmount);
        paymentToken.transfer(
            bigDaddy,
            amount.sub(referralBonus).sub(developerAmount)
        );

        Referrers[referrer].salesCount++;
        Referrers[referrer].totalBuyersAmount += amount;
        Referrers[referrer].totalRewards += referralBonus;
        Referrers[referrer].buyersAmount[msg.sender] += amount;
        Referrers[referrer].buyerRewards[msg.sender] += referralBonus;

        if (!isReferrerForBuyer[referrer][msg.sender]) {
            Referrers[referrer].buyersCount++;
            Referrers[referrer].buyersList.push(msg.sender);
            isReferrerForBuyer[referrer][msg.sender] = true;
        }

        if (Buyers[msg.sender].sales == 0) {
            buyers.push(msg.sender);
            firstReferrerOf[msg.sender] = referrer;
        }

        Buyers[msg.sender].sales++;
        Buyers[msg.sender].amountUSDT += amount;
        emit buyAccess(msg.sender, amount, referrer);
    }

    function thisBigDaddy(address _bigDaddy) private {
        isReferrer[_bigDaddy] = true;
        referrerPercentage[_bigDaddy] = 10;
        referrers.push(_bigDaddy);
        referrersCount++;
    }

    function generateReferralLinkSpecial(address referrer, uint256 _percentage)
        external
        onlyOwner
        returns (address)
    {
        isReferrer[referrer] = true;
        referrerPercentage[referrer] = _percentage;
        referrers.push(referrer);
        referrersCount++;
        return referrer;
    }

    function generateReferralLink(address _referrer)
        private
        notPaused
        returns (address referrer)
    {
        require(!isReferrer[_referrer], "Address already is a referrer");
        isReferrer[_referrer] = true;
        referrer = _referrer;
        referrerPercentage[referrer] = normalPercentage;
        referrersCount++;
        referrers.push(_referrer);

        emit ReferralLink(referrer);
        return referrer;
    }

    function changeReferrerPercentage(address _referrer, uint256 _newPercentage)
        external
        onlyOwner
    {
        referrerPercentage[_referrer] = _newPercentage;
    }

    function getReferrersWithPercentage()
        public
        view
        returns (address[] memory, uint256[] memory)
    {
        uint256 length = referrers.length;
        address[] memory referrerAddresses = new address[](length);
        uint256[] memory referrerPercentages = new uint256[](length);
        for (uint256 i = 0; i < length; i++) {
            referrerAddresses[i] = referrers[i];
            referrerPercentages[i] = referrerPercentage[referrers[i]];
        }
        return (referrerAddresses, referrerPercentages);
    }

    function getAllBuyers()
        public
        view
        returns (
            address[] memory BuyersAddress,
            uint256[] memory BuyersSales,
            uint256[] memory BuyersSpentAmounts
        )
    {
        uint256 buyerCount = buyers.length;

        address[] memory buyerAddresses = new address[](buyerCount);
        uint256[] memory buyerSales = new uint256[](buyerCount);
        uint256[] memory buyerUSDTAmounts = new uint256[](buyerCount);

        for (uint256 i = 0; i < buyerCount; i++) {
            buyerAddresses[i] = buyers[i];
            buyerSales[i] = Buyers[buyers[i]].sales;
            buyerUSDTAmounts[i] = Buyers[buyers[i]].amountUSDT;
        }

        return (buyerAddresses, buyerSales, buyerUSDTAmounts);
    }

    function getReferralTree(address _address, uint256 _depth)
        public
        view
        returns (address[] memory)
    {
        require(_depth > 0, "Depth must be greater than 0");
        address[] memory referrals = new address[](_depth);

        address currentReferrer = _address;
        for (uint256 i = 0; i < _depth; i++) {
            currentReferrer = firstReferrerOf[currentReferrer];
            if (currentReferrer == address(0)) {
                break;
            }
            referrals[i] = currentReferrer;
        }
        return referrals;
    }

    function getReferrerInfo(address _referrer)
        public
        view
        returns (
            uint256 salesCount,
            uint256 buyersCount,
            uint256 totalBuyersAmount,
            uint256 totalRewards,
            address[] memory buyersList,
            uint256[] memory buyersAmounts,
            uint256[] memory buyerRewards
        )
    {
        // Fetch ReferrerInfo struct for the provided referrer address
        ReferrerInfo storage referrerInfo = Referrers[_referrer];

        salesCount = referrerInfo.salesCount;
        buyersCount = referrerInfo.buyersCount;
        totalBuyersAmount = referrerInfo.totalBuyersAmount;
        totalRewards = referrerInfo.totalRewards;
        buyersList = referrerInfo.buyersList;
        buyersAmounts = _getBuyersAmounts(_referrer, referrerInfo.buyersList);
        buyerRewards = _getBuyerRewards(_referrer, referrerInfo.buyersList);

        return (
            salesCount,
            buyersCount,
            totalBuyersAmount,
            totalRewards,
            buyersList,
            buyersAmounts,
            buyerRewards
        );
    }

    function _getBuyersAmounts(address _referrer, address[] memory buyersList)
        private
        view
        returns (uint256[] memory)
    {
        uint256[] memory amounts = new uint256[](buyersList.length);

        for (uint256 i = 0; i < buyersList.length; i++) {
            amounts[i] = Referrers[_referrer].buyersAmount[buyersList[i]];
        }

        return amounts;
    }

    function _getBuyerRewards(address _referrer, address[] memory buyersList)
        private
        view
        returns (uint256[] memory)
    {
        uint256[] memory rewards = new uint256[](buyersList.length);

        for (uint256 i = 0; i < buyersList.length; i++) {
            rewards[i] = Referrers[_referrer].buyerRewards[buyersList[i]];
        }

        return rewards;
    }

    function getBuyerInfoForReferrer(address _referrer, address _buyer)
        public
        view
        returns (uint256 buyerAmount, uint256 reward)
    {
        return (
            Referrers[_referrer].buyersAmount[_buyer],
            Referrers[_referrer].buyerRewards[_buyer]
        );
    }

    function withdrawTokens(
        IERC20 _token,
        address _to,
        uint256 _amount
    ) external onlyOwner {
        require(_token.transfer(_to, _amount), "Failed to transfer the tokens");
    }
}