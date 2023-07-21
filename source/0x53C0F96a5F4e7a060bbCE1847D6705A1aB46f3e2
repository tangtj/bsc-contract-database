// Sources flattened with hardhat v2.12.7 https://hardhat.org

// File @openzeppelin/contracts/utils/Context.sol@v4.3.3

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}


// File @openzeppelin/contracts/access/Ownable.sol@v4.3.3

pragma solidity ^0.8.0;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _setOwner(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


// File @chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol@v0.6.1


pragma solidity ^0.8.0;

interface AggregatorV3Interface {
  function decimals() external view returns (uint8);

  function description() external view returns (string memory);

  function version() external view returns (uint256);

  function getRoundData(uint80 _roundId)
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );

  function latestRoundData()
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );
}


// File @openzeppelin/contracts/security/ReentrancyGuard.sol@v4.3.3



pragma solidity ^0.8.0;

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
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}


// File @openzeppelin/contracts/utils/math/SafeCast.sol@v4.3.3


pragma solidity ^0.8.0;

/**
 * @dev Wrappers over Solidity's uintXX/intXX casting operators with added overflow
 * checks.
 *
 * Downcasting from uint256/int256 in Solidity does not revert on overflow. This can
 * easily result in undesired exploitation or bugs, since developers usually
 * assume that overflows raise errors. `SafeCast` restores this intuition by
 * reverting the transaction when such an operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 *
 * Can be combined with {SafeMath} and {SignedSafeMath} to extend it to smaller types, by performing
 * all math on `uint256` and `int256` and then downcasting.
 */
library SafeCast {
    /**
     * @dev Returns the downcasted uint224 from uint256, reverting on
     * overflow (when the input is greater than largest uint224).
     *
     * Counterpart to Solidity's `uint224` operator.
     *
     * Requirements:
     *
     * - input must fit into 224 bits
     */
    function toUint224(uint256 value) internal pure returns (uint224) {
        require(value <= type(uint224).max, "SafeCast: value doesn't fit in 224 bits");
        return uint224(value);
    }

    /**
     * @dev Returns the downcasted uint128 from uint256, reverting on
     * overflow (when the input is greater than largest uint128).
     *
     * Counterpart to Solidity's `uint128` operator.
     *
     * Requirements:
     *
     * - input must fit into 128 bits
     */
    function toUint128(uint256 value) internal pure returns (uint128) {
        require(value <= type(uint128).max, "SafeCast: value doesn't fit in 128 bits");
        return uint128(value);
    }

    /**
     * @dev Returns the downcasted uint96 from uint256, reverting on
     * overflow (when the input is greater than largest uint96).
     *
     * Counterpart to Solidity's `uint96` operator.
     *
     * Requirements:
     *
     * - input must fit into 96 bits
     */
    function toUint96(uint256 value) internal pure returns (uint96) {
        require(value <= type(uint96).max, "SafeCast: value doesn't fit in 96 bits");
        return uint96(value);
    }

    /**
     * @dev Returns the downcasted uint64 from uint256, reverting on
     * overflow (when the input is greater than largest uint64).
     *
     * Counterpart to Solidity's `uint64` operator.
     *
     * Requirements:
     *
     * - input must fit into 64 bits
     */
    function toUint64(uint256 value) internal pure returns (uint64) {
        require(value <= type(uint64).max, "SafeCast: value doesn't fit in 64 bits");
        return uint64(value);
    }

    /**
     * @dev Returns the downcasted uint32 from uint256, reverting on
     * overflow (when the input is greater than largest uint32).
     *
     * Counterpart to Solidity's `uint32` operator.
     *
     * Requirements:
     *
     * - input must fit into 32 bits
     */
    function toUint32(uint256 value) internal pure returns (uint32) {
        require(value <= type(uint32).max, "SafeCast: value doesn't fit in 32 bits");
        return uint32(value);
    }

    /**
     * @dev Returns the downcasted uint16 from uint256, reverting on
     * overflow (when the input is greater than largest uint16).
     *
     * Counterpart to Solidity's `uint16` operator.
     *
     * Requirements:
     *
     * - input must fit into 16 bits
     */
    function toUint16(uint256 value) internal pure returns (uint16) {
        require(value <= type(uint16).max, "SafeCast: value doesn't fit in 16 bits");
        return uint16(value);
    }

    /**
     * @dev Returns the downcasted uint8 from uint256, reverting on
     * overflow (when the input is greater than largest uint8).
     *
     * Counterpart to Solidity's `uint8` operator.
     *
     * Requirements:
     *
     * - input must fit into 8 bits.
     */
    function toUint8(uint256 value) internal pure returns (uint8) {
        require(value <= type(uint8).max, "SafeCast: value doesn't fit in 8 bits");
        return uint8(value);
    }

    /**
     * @dev Converts a signed int256 into an unsigned uint256.
     *
     * Requirements:
     *
     * - input must be greater than or equal to 0.
     */
    function toUint256(int256 value) internal pure returns (uint256) {
        require(value >= 0, "SafeCast: value must be positive");
        return uint256(value);
    }

    /**
     * @dev Returns the downcasted int128 from int256, reverting on
     * overflow (when the input is less than smallest int128 or
     * greater than largest int128).
     *
     * Counterpart to Solidity's `int128` operator.
     *
     * Requirements:
     *
     * - input must fit into 128 bits
     *
     * _Available since v3.1._
     */
    function toInt128(int256 value) internal pure returns (int128) {
        require(value >= type(int128).min && value <= type(int128).max, "SafeCast: value doesn't fit in 128 bits");
        return int128(value);
    }

    /**
     * @dev Returns the downcasted int64 from int256, reverting on
     * overflow (when the input is less than smallest int64 or
     * greater than largest int64).
     *
     * Counterpart to Solidity's `int64` operator.
     *
     * Requirements:
     *
     * - input must fit into 64 bits
     *
     * _Available since v3.1._
     */
    function toInt64(int256 value) internal pure returns (int64) {
        require(value >= type(int64).min && value <= type(int64).max, "SafeCast: value doesn't fit in 64 bits");
        return int64(value);
    }

    /**
     * @dev Returns the downcasted int32 from int256, reverting on
     * overflow (when the input is less than smallest int32 or
     * greater than largest int32).
     *
     * Counterpart to Solidity's `int32` operator.
     *
     * Requirements:
     *
     * - input must fit into 32 bits
     *
     * _Available since v3.1._
     */
    function toInt32(int256 value) internal pure returns (int32) {
        require(value >= type(int32).min && value <= type(int32).max, "SafeCast: value doesn't fit in 32 bits");
        return int32(value);
    }

    /**
     * @dev Returns the downcasted int16 from int256, reverting on
     * overflow (when the input is less than smallest int16 or
     * greater than largest int16).
     *
     * Counterpart to Solidity's `int16` operator.
     *
     * Requirements:
     *
     * - input must fit into 16 bits
     *
     * _Available since v3.1._
     */
    function toInt16(int256 value) internal pure returns (int16) {
        require(value >= type(int16).min && value <= type(int16).max, "SafeCast: value doesn't fit in 16 bits");
        return int16(value);
    }

    /**
     * @dev Returns the downcasted int8 from int256, reverting on
     * overflow (when the input is less than smallest int8 or
     * greater than largest int8).
     *
     * Counterpart to Solidity's `int8` operator.
     *
     * Requirements:
     *
     * - input must fit into 8 bits.
     *
     * _Available since v3.1._
     */
    function toInt8(int256 value) internal pure returns (int8) {
        require(value >= type(int8).min && value <= type(int8).max, "SafeCast: value doesn't fit in 8 bits");
        return int8(value);
    }

    /**
     * @dev Converts an unsigned uint256 into a signed int256.
     *
     * Requirements:
     *
     * - input must be less than or equal to maxInt256.
     */
    function toInt256(uint256 value) internal pure returns (int256) {
        // Note: Unsafe cast below is okay because `type(int256).max` is guaranteed to be positive
        require(value <= uint256(type(int256).max), "SafeCast: value doesn't fit in an int256");
        return int256(value);
    }
}


// File contracts/AddressHandler.sol

pragma solidity 0.8.17;

contract AddressHandler {
    function stringToAddress(string memory _address) public pure returns (address) {
        string memory cleanAddress = remove0xPrefix(_address);
        bytes20 _addressBytes = parseHexStringToBytes20(cleanAddress);
        return address(_addressBytes);
    }

    function remove0xPrefix(string memory _hexString) internal pure returns (string memory) {
        if (bytes(_hexString).length >= 2 && bytes(_hexString)[0] == '0' && (bytes(_hexString)[1] == 'x' || bytes(_hexString)[1] == 'X')) {
            return substring(_hexString, 2, bytes(_hexString).length);
        }
        return _hexString;
    }

    function substring(string memory _str, uint256 _start, uint256 _end) internal pure returns (string memory) {
        bytes memory _strBytes = bytes(_str);
        bytes memory _result = new bytes(_end - _start);
        for (uint256 i = _start; i < _end; i++) {
            _result[i - _start] = _strBytes[i];
        }
        return string(_result);
    }

    function parseHexStringToBytes20(string memory _hexString) internal pure returns (bytes20) {
        bytes memory _bytesString = bytes(_hexString);
        uint160 _parsedBytes = 0;
        for (uint256 i = 0; i < _bytesString.length; i += 2) {
            _parsedBytes *= 256;
            uint8 _byteValue = parseByteToUint8(_bytesString[i]);
            _byteValue *= 16;
            _byteValue += parseByteToUint8(_bytesString[i + 1]);
            _parsedBytes += _byteValue;
        }
        return bytes20(_parsedBytes);
    }

    function parseByteToUint8(bytes1 _byte) internal pure returns (uint8) {
        if (uint8(_byte) >= 48 && uint8(_byte) <= 57) {
            return uint8(_byte) - 48;
        } else if (uint8(_byte) >= 65 && uint8(_byte) <= 70) {
            return uint8(_byte) - 55;
        } else if (uint8(_byte) >= 97 && uint8(_byte) <= 102) {
            return uint8(_byte) - 87;
        } else {
            revert(string(abi.encodePacked("Invalid byte value: ", _byte)));
        }
    }
}


// File contracts/ICOPrimary.sol

pragma solidity 0.8.17;


interface IBEP20 {
    function transfer(address, uint256) external returns (bool);
    function transferFrom(address, address, uint256) external returns (bool);
    function allowance(address, address) external view returns (uint256);
    function balanceOf(address) external view returns (uint256);
}

contract ICOPrimary is ReentrancyGuard, Ownable {

    using SafeCast for int256;

    mapping (address => mapping (CurrencyType => uint256)) private TokenBought;

    enum CurrencyType {
        PRIMARYCURRENCY,
        PRIMARYUSDT,
        SECONDARYCURRENCY,
        SECONDARYUSDT
    }

        IBEP20 public immutable _token;
    IBEP20 public immutable _usdtToken;
        address payable public _wallet;
    address payable public _bridge;
        uint256 public _startPrice;
    uint256 public _endPrice;
        uint256 public _time_unit = 1 minutes;

        uint256 public usdtRaised;
    uint256 public suppliedTokens;
    uint256 public tokenHold;
        uint256 public hardCap;

        uint256 public startICOTimestamp;
        uint256 public endICOTimestamp;

    uint8 public stageCount;

        uint256 public minPurchase;
        uint256 public maxPurchase;
    bool private stopIcoRun;

        uint256 public availableTokens;

    AddressHandler public addressHandler;

    AggregatorV3Interface internal immutable primaryCurrencyPriceFeed;
    AggregatorV3Interface internal immutable secondaryCurrencyPriceFeed;

        event TokensPurchased(address indexed purchaser, uint256 indexed purchased_token_amount, uint256 token_amount, uint256 usdtRaised);
    event TokensDelivered(address indexed purchaser, uint256 token_amount);
    event HardCapUpdated(uint256 newValue);
    event MaxPurchaseUpdated(uint256 newValue);
    event MinPurchaseUpdated(uint256 newValue);
    event WalletUpdated(address newWallet);

    constructor(address payable wallet, address payable bridge, address tokenAddress, address usdtAddress, address _primaryCurrencyPriceFeed, address _secondaryCurrencyPriceFeed) {
        require(wallet != address(0), "wallet is the zero address");
        require(bridge != address(0), "bridge is the zero address");
        require(tokenAddress != address(0), "token address is the zero");
        require(usdtAddress != address(0), "usdt address is the zero");
        require(_primaryCurrencyPriceFeed != address(0), "Eth primaryCurrencyPriceFeed is the zero address");
        require(_secondaryCurrencyPriceFeed != address(0), "Bsc primaryCurrencyPriceFeed is the zero address");

        _wallet = wallet;
        _bridge = bridge;
        _token = IBEP20(tokenAddress);
        _usdtToken = IBEP20(usdtAddress);
        primaryCurrencyPriceFeed = AggregatorV3Interface(_primaryCurrencyPriceFeed);
        secondaryCurrencyPriceFeed = AggregatorV3Interface(_secondaryCurrencyPriceFeed);
        addressHandler = new AddressHandler();
        }

        receive() external payable {
        buyTokensByPrimaryCurrencyWithoutReferral(_msgSender());
        }

    fallback() external payable {
        buyTokensByPrimaryCurrencyWithoutReferral(_msgSender());
    }

    function buyTokensByPrimaryCurrencyWithoutReferral(address beneficiary) public nonReentrant icoActive payable {
        uint256 usdtAmount = getPrimaryCurrencyPrice() * msg.value / ( 10 ** 8);
        uint256 tokens = _getTokenAmountFromUSDT(usdtAmount);
        _preValidatePurchase(beneficiary, usdtAmount, tokens);
        _purchaseTokens(usdtAmount, tokens, beneficiary, CurrencyType.PRIMARYCURRENCY);
    }

    function buyTokensByPrimaryCurrency(string memory addressString) public nonReentrant icoActive payable {
        address beneficiary = _msgSender();
        address referral = getAddressFromString(addressString);
        uint256 usdtAmount = getPrimaryCurrencyPrice() * msg.value / ( 10 ** 8);
        uint256 tokens = _getTokenAmountFromUSDT(usdtAmount);
        _preValidatePurchase(beneficiary, usdtAmount, tokens);
        _purchaseTokens(usdtAmount, tokens, beneficiary, CurrencyType.PRIMARYCURRENCY);
        if (!isZeroAddress(referral) && (referral != beneficiary) && (referral != address(this)))
            _purchaseTokens(0, tokens / 10, referral, CurrencyType.PRIMARYCURRENCY);
    }

    function buyTokensByPrimaryUSDT(uint256 usdtAmount, string memory addressString) public nonReentrant icoActive {
        address beneficiary = _msgSender();
        address referral = getAddressFromString(addressString);
        uint256 tokens = _getTokenAmountFromUSDT(usdtAmount);
        _preValidatePurchase(beneficiary, usdtAmount, tokens);

        uint256 ourAllowance = _usdtToken.allowance(
            _msgSender(),
            address(this)
        );

        require(usdtAmount <= ourAllowance, "Make sure to add enough allowance");
        (bool success, ) = address(_usdtToken).call(
            abi.encodeWithSignature(
                "transferFrom(address,address,uint256)",
                _msgSender(),
                address(this),
                usdtAmount
            )
        );
        require(success, "Token payment failed");
        _purchaseTokens(usdtAmount, tokens, beneficiary, CurrencyType.PRIMARYUSDT);
        if (!isZeroAddress(referral) && (referral != beneficiary) && (referral != address(this)))
            _purchaseTokens(0, tokens / 10, referral, CurrencyType.PRIMARYUSDT);
    }

    function getAddressFromString(string memory addressString) public view returns (address) {
        try addressHandler.stringToAddress(addressString) returns (address newAddress) {
            return newAddress;
        } catch {
            return address(0);
        }
    }

    function isZeroAddress(address _address) public pure returns (bool) {
        return _address == address(0);
    }

    function checkIfPurchaseEnabledBySecondaryCurrency(uint256 currencyAmount, address beneficiary) public view returns (uint8) {
        uint256 usdtAmount = getSecondaryCurrencyPrice() * currencyAmount / ( 10 ** 8);
        return checkIfPurchaseEnabledBySecondaryUSDT(usdtAmount, beneficiary);
    }

    function checkIfPurchaseEnabledBySecondaryUSDT(uint256 usdtAmount, address beneficiary) public view returns (uint8) {
        uint256 tokens = _getTokenAmountFromUSDT(usdtAmount);
        if (_isIcoActive() == false) return 0;
        else if (beneficiary == address(0)) return 1;
        else if (usdtAmount == 0) return 2;
        else if (tokens < minPurchase) return 3;
        else if (getTokenBoughtFor(beneficiary) + tokens > maxPurchase) return 4;
        else if (usdtRaised + usdtAmount > hardCap) return 5;
        return 6;
    }

    function buyTokensBySecondaryCurrency(uint256 secondaryCurrencyAmount, address beneficiary, address referral) public nonReentrant icoActive onlyBridge {
        uint256 usdtAmount = getSecondaryCurrencyPrice() * secondaryCurrencyAmount / ( 10 ** 8);
        uint256 tokens = _getTokenAmountFromUSDT(usdtAmount);
        _preValidatePurchase(beneficiary, usdtAmount, tokens);
        _purchaseTokens(usdtAmount, tokens, beneficiary, CurrencyType.SECONDARYCURRENCY);
        if (!isZeroAddress(referral) && (referral != beneficiary) && (referral != address(this)))
            _purchaseTokens(0, tokens / 10, referral, CurrencyType.PRIMARYCURRENCY);
    }

    function buyTokensBySecondaryUSDT(uint256 usdtAmount, address beneficiary, address referral) public nonReentrant icoActive onlyBridge {
        uint256 tokens = _getTokenAmountFromUSDT(usdtAmount);
        _preValidatePurchase(beneficiary, usdtAmount, tokens);
        _purchaseTokens(usdtAmount, tokens, beneficiary, CurrencyType.SECONDARYUSDT);
        if (!isZeroAddress(referral) && (referral != beneficiary) && (referral != address(this)))
            _purchaseTokens(0, tokens / 10, referral, CurrencyType.PRIMARYCURRENCY);
    }

    function _purchaseTokens(uint256 usdtAmount, uint256 tokens, address beneficiary, CurrencyType _currencyType) internal {
        usdtRaised = usdtRaised + usdtAmount;
        tokenHold = tokenHold + tokens;
        availableTokens = availableTokens - tokens;
        TokenBought[beneficiary][_currencyType] = TokenBought[beneficiary][_currencyType] + tokens;
        emit TokensPurchased(beneficiary, tokens, getTokenBoughtFor(beneficiary), usdtRaised);
    }

    function getTokenBoughtFor(address beneficiary) public view returns (uint256) {
        return TokenBought[beneficiary][CurrencyType.PRIMARYCURRENCY] + TokenBought[beneficiary][CurrencyType.PRIMARYUSDT] + TokenBought[beneficiary][CurrencyType.SECONDARYCURRENCY] + TokenBought[beneficiary][CurrencyType.SECONDARYUSDT];
    }

    function _preValidatePurchase(address beneficiary, uint256 usdtAmount, uint256 tokens) internal view {
        require(beneficiary != address(0), "beneficiary is the zero address");
        require(usdtAmount != 0, "weiAmount is 0");
        require(tokens != 0, "trying to purchase 0 tokens");
        require(tokens >= minPurchase, 'have to send at least: minPurchase');
        require(getTokenBoughtFor(beneficiary) + tokens <= maxPurchase, 'can\'t buy more than: maxPurchase');
        require(usdtRaised + usdtAmount <= hardCap, 'Hard Cap reached');
    }

    function _getTokenAmountFromUSDT(uint256 usdtAmount) internal view returns (uint256) {
        uint256 currentPrice = _getCurrentTokenPrice();
        require(currentPrice > 0, "current price is wrongly 0");
        uint256 tokenAmount = usdtAmount * 10 ** 18 / currentPrice;
        return tokenAmount;
    }

    function getMaximumUSDTAmount() external view returns (uint256) {
        require(_startPrice > 0, "start price need to be set above 0");
        uint256 tokenAmount = maxPurchase * _startPrice / (10 ** 18);
        return tokenAmount;
    } 

    function getPrimaryCurrencyPrice() public view returns (uint256) {
        (
            /* uint80 roundID */,
            int256 price,
            /* uint256 startedAt */,
            /* uint256 timeStamp */,
            /* uint80 answeredInRound */
        ) = primaryCurrencyPriceFeed.latestRoundData();

        // int256 price = 31100000000;
        return price.toUint256();
    }

    function getSecondaryCurrencyPrice() public view returns (uint256) {
        (
            /* uint80 roundID */,
            int256 price,
            /* uint256 startedAt */,
            /* uint256 timeStamp */,
            /* uint80 answeredInRound */
        ) = secondaryCurrencyPriceFeed.latestRoundData();

        return price.toUint256();
    }

    function declareCurrentAvailableTokensAsSuppliedTokens() external onlyOwner {
        suppliedTokens = _token.balanceOf(address(this));
    }

    function startICO(uint duration_, uint8 stageCount_, uint256 startPrice_, uint256 endPrice_, uint minPurchase_, uint maxPurchase_, uint256 hardCap_) external onlyOwner icoNotActive() {    
        require(duration_ > 0, 'duration is 0');
        require(stageCount_ > 0, 'stageCount is 0');
        require(startPrice_ > 0, 'ICO startprice is 0');
        require(endPrice_ >= startPrice_, 'ICO endPrice is lower than startPrice');
        require(minPurchase_ > 0, 'minPurchase is 0');
        require(minPurchase_ < maxPurchase_, "minPurchase must be lower than maxPurchase");
        require(hardCap_ > 0, 'hardCap is 0');

        startICOTimestamp = block.timestamp;
        endICOTimestamp = startICOTimestamp + duration_ * _time_unit;
        availableTokens = _token.balanceOf(address(this));
        minPurchase = minPurchase_;
        maxPurchase = maxPurchase_;
        hardCap = hardCap_;
        stageCount = stageCount_;
        _startPrice = startPrice_;
        _endPrice = endPrice_;
        usdtRaised = 0;
        stopIcoRun = false;
    }

    function stopICO() external onlyOwner {
        endICOTimestamp = 0;
        startICOTimestamp = 0;
        availableTokens = 0;
        stopIcoRun = true;
    }

    function withdrawUsdt() external onlyOwner onlyContractHasUSDT {
        uint balance = _usdtToken.balanceOf(address(this));
        _usdtToken.transfer(_wallet, balance);
    }

    function withdrawCurrency() external onlyOwner onlyContractHasCurrency {
        payable(_wallet).transfer(address(this).balance);
    }

    function getAvailableTokenAmount() public view returns (uint256) {
        return availableTokens;
    }

    function _getCurrentTokenPrice() internal view returns (uint256) {
        uint256 timestamp = block.timestamp;

        if (endICOTimestamp > 0 && timestamp < endICOTimestamp) {
            require(endICOTimestamp > startICOTimestamp, "Exception: end date is below start time");
            uint256 duration = endICOTimestamp - startICOTimestamp;
            for (uint8 i = 0; i <= stageCount; i++) {
                if (timestamp >= startICOTimestamp + duration * i / (stageCount + 1) && timestamp < startICOTimestamp + duration * (i + 1) / (stageCount + 1)) {
                    return _startPrice + i * (_endPrice - _startPrice) / (stageCount + 1) ;
                }
            }
        }
        return _endPrice;
    }

    function _deliverTokens(address beneficiary, uint256 tokenAmount) internal {
        require(_token.balanceOf(address(this)) >= tokenAmount, 'No sufficient tokens to deliver in contract');
        _token.transfer(beneficiary, tokenAmount);
        emit TokensDelivered(beneficiary, tokenAmount);
    }

    function retakeRemainingTokens() public onlyOwner icoNotActive {
        uint256 tokenAmount = suppliedTokens - tokenHold;
        require(tokenAmount > 0, 'No Remaining tokens');
        _deliverTokens(_wallet, tokenAmount);
    }

    function invokeTokensAfterPresale() public icoNotActive nonReentrant {
        address beneficiary = _msgSender();
        uint256 tokenAmount  = getTokenBoughtFor(beneficiary);
        require(tokenAmount > 0, 'No tokens to claim');
        TokenBought[beneficiary][CurrencyType.PRIMARYCURRENCY] = 0;
        TokenBought[beneficiary][CurrencyType.PRIMARYUSDT] = 0;
        TokenBought[beneficiary][CurrencyType.SECONDARYCURRENCY] = 0;
        TokenBought[beneficiary][CurrencyType.SECONDARYUSDT] = 0;
        _deliverTokens(beneficiary, tokenAmount);
    }

    function getIcoStatus() external view returns (uint8) {
        return _isIcoActive() ? 3 : 2;
    }

    function _isIcoActive() internal view returns (bool) {
        if (endICOTimestamp > 0 && startICOTimestamp < endICOTimestamp && block.timestamp < endICOTimestamp && availableTokens > 0)
            return true;
        return false;
    }

    function getUsdtRaised() external view returns (uint256) {
        return usdtRaised;
    }

    function getToken() public view returns (IBEP20) {
        return _token;
    }

    function getStopIcoRun() external view returns (bool) {
        return stopIcoRun;
    }

    function getHardCap() external view icoActive returns (uint256) {
        return hardCap;
    }

    function getMaxPurchase() external view icoActive returns (uint256) {
        return maxPurchase;
    }

    function getMinPurchase() external view icoActive returns (uint256) {
        return minPurchase;
    }

    function getStartTimestamp() external view icoActive returns (uint256) {
        return startICOTimestamp;
    }

    function getEndTimestamp() external view icoActive returns (uint256) {
        return endICOTimestamp;
    }

    function getStartPrice() external view icoActive returns (uint256) {
        return _startPrice;
    }

    function getEndPrice() external view icoActive returns (uint256) {
        return _endPrice;
    }

    function getStageCount() external view icoActive returns (uint8) {
        return stageCount;
    }

    function setWallet(address payable newWallet) external onlyOwner {
        require(newWallet != address(0), "Pre-Sale: newWallet is the zero address");
        _wallet = newWallet;
        emit WalletUpdated(_wallet);
    }

    function setHardCap(uint256 value) external onlyOwner icoActive {
        require(value > 0, "HardCap is incorrect");
        hardCap = value;
        emit HardCapUpdated(hardCap);
    }

    function setMaxPurchase(uint256 value) external onlyOwner icoActive {
        require(value > 0, "MaxPurchase is incorrect");
        maxPurchase = value;
        emit MaxPurchaseUpdated(maxPurchase);
    }

    function setMinPurchase(uint256 value) external onlyOwner icoActive {
        require(value > 0, "MinPurchase is incorrect");
        minPurchase = value;
        emit MinPurchaseUpdated(minPurchase);
    }

    modifier icoActive() {
        require(endICOTimestamp > 0 && startICOTimestamp < endICOTimestamp, "ICO must be active.");
        require(block.timestamp < endICOTimestamp, "current Time should be lower than end time.");
        require(availableTokens > 0, "available tokens must exist.");
        _;
    }

    modifier icoNotActive() {
        require(_isIcoActive() == false, 'ICO should not be active');
        _;
    }

    modifier onlyContractHasUSDT() {
        require(_usdtToken.balanceOf(address(this)) > 0, 'Pre-Sale: Contract has no usdt.');
        _;
    }

    modifier onlyContractHasCurrency() {
        require(address(this).balance > 0, 'Pre-Sale: Contract has no main currency.');
        _;
    }

    modifier onlyBridge() {
        require(_bridge == _msgSender(), 'Only bridge can invoke this function.');
        _;
    }
}