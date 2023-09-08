// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

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
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
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

interface IERC20
{
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract LockToken is Ownable
{
    using SafeMath for uint256;
    
    address private _kit_ERC20Address;
    uint256[][] private _kit_ERC20_UnLock_Time =        [
                                                            [0]
                                                        ];
    uint256[][] private _kit_ERC20_UnLock_Amount =      [
                                                            [0]
                                                        ];

    address private _kit_USDT_LP_ERC20Address;
    uint256[][] private _kit_USDT_LP_UnLock_Time =      [
                                                            [1671580800]
                                                        ];
    uint256[][] private _kit_USDT_LP_UnLock_Amount =    [
                                                            [308]
                                                        ];

    address private _dpk_ERC20Address;
    /*
        marketing timestamp
        consultant timestamp
        team timestamp
    */
    uint256[][] private _dpk_ERC20_UnLock_Time =    [
                                                        [1645401600, 1647820800, 1650499200, 1653091200, 1655769600, 1658361600, 1661040000, 1663718400, 1666310400, 1668988800, 1671580800, 1674259200, 1676937600, 1679356800, 1682035200, 1684627200, 1687305600, 1689897600, 1692576000, 1695254400],
                                                        [1647043200, 1649721600, 1652313600, 1654992000, 1657584000, 1660262400, 1662940800, 1665532800, 1668211200, 1670803200, 1673481600, 1676160000, 1678579200, 1681257600, 1683849600, 1686528000, 1689120000, 1691798400, 1694476800, 1697068800],
                                                        [1671580800, 1674259200, 1676937600, 1679356800, 1682035200, 1684627200, 1687305600, 1689897600, 1692576000, 1695254400, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                                                    ];

    /*
        marketing amount
        consultant amount
        team amount
    */
    uint256[][] private _dpk_ERC20_UnLock_Amount =  [
                                                        [675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000, 675000],
                                                        [450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000, 450000],
                                                        [1000000, 1000000, 1000000, 1000000, 1000000, 1000000, 1000000, 1000000, 1000000, 1000000, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
                                                    ];

    address private _dpk_USDT_LP_ERC20Address;
    uint256[][] private _dpk_USDT_LP_UnLock_Time =      [
                                                            [1671580800]
                                                        ];
    uint256[][] private _dpk_USDT_LP_UnLock_Amount =    [
                                                            [790569]
                                                        ];

    address private _withDrawAddress;

    constructor(address kitERC20Address,
                address kitUSDTLPERC20Address,
                address dpkERC20Address,
                address dpkUSDTLPERC20Address) 
    {
        _kit_ERC20Address = kitERC20Address;
        _kit_USDT_LP_ERC20Address = kitUSDTLPERC20Address;
        _dpk_ERC20Address = dpkERC20Address;
        _dpk_USDT_LP_ERC20Address = dpkUSDTLPERC20Address;
        _withDrawAddress = msg.sender;
    }

    function setWithDrawAddress(address newAddress) public onlyOwner {
        _withDrawAddress = newAddress;
    }

    function getWithDrawAddress() public view returns(address) {
        return _withDrawAddress;
    }

    function getTimestamp() public view returns(uint256) {
        return block.timestamp;
    }

    function getCanWithDrawAmount(uint8 tokenType, uint8 position, uint8 index) public view returns(uint256) {
        uint256 amount = 0;
        if(tokenType == 1)
        {
            if (block.timestamp > _kit_ERC20_UnLock_Time[position][index])
            {
                amount = _kit_ERC20_UnLock_Amount[position][index] * 10 ** 6;
            }
        }
        else if(tokenType == 2)
        {
            if (block.timestamp > _kit_USDT_LP_UnLock_Time[position][index])
            {
                amount = _kit_USDT_LP_UnLock_Amount[position][index] * 10 ** 18;
            }
        }
        else if(tokenType == 11)
        {
            if (block.timestamp > _dpk_ERC20_UnLock_Time[position][index])
            {
                amount = _dpk_ERC20_UnLock_Amount[position][index] * 10 ** 18;
            }
        }
        else if(tokenType == 12)
        {
            if (block.timestamp > _dpk_USDT_LP_UnLock_Time[position][index])
            {
                amount = _dpk_USDT_LP_UnLock_Amount[position][index] * 10 ** 18;
            }
        }

        return amount;
    }

    event WithDrawERC20Event(uint256 indexed coins);
    function withDrawERC20Token(uint8 tokenType, uint8 position, uint8 index) public onlyOwner 
    {
        address erc20 = 0x0000000000000000000000000000000000000000;
        uint256 amount = 0;
        if(tokenType == 1)
        {
            erc20 = _kit_ERC20Address;
            if (block.timestamp > _kit_ERC20_UnLock_Time[position][index])
            {
                amount = _kit_ERC20_UnLock_Amount[position][index] * 10 ** 6;
            }
        }
        else if(tokenType == 2)
        {
            erc20 = _kit_USDT_LP_ERC20Address;
            if (block.timestamp > _kit_USDT_LP_UnLock_Time[position][index])
            {
                amount = _kit_USDT_LP_UnLock_Amount[position][index] * 10 ** 18;
            }
        }
        else if(tokenType == 11)
        {
            erc20 = _dpk_ERC20Address;
            if (block.timestamp > _dpk_ERC20_UnLock_Time[position][index])
            {
                amount = _dpk_ERC20_UnLock_Amount[position][index] * 10 ** 18;
            }
        }
        else if(tokenType == 12)
        {
            erc20 = _dpk_USDT_LP_ERC20Address;
            if (block.timestamp > _dpk_USDT_LP_UnLock_Time[position][index])
            {
                amount = _dpk_USDT_LP_UnLock_Amount[position][index] * 10 ** 18;
            }
        }
        require(amount > 0, "Not amount in time can withdraw");

        IERC20 erc20Token = IERC20(erc20);
        uint256 dexBalance = erc20Token.balanceOf(address(this));
        require(dexBalance > 0, "Not enough tokens in the reserve");
        require(dexBalance >= amount, "Not enough tokens in the reserve, check amount");
        
        erc20Token.transfer(_withDrawAddress, amount);
        emit WithDrawERC20Event(amount);

        if(tokenType == 1)
        {
            _kit_ERC20_UnLock_Amount[position][index] = 0;
        }
        else if(tokenType == 2)
        {
            _kit_USDT_LP_UnLock_Amount[position][index] = 0;
        }
        else if(tokenType == 11)
        {
            _dpk_ERC20_UnLock_Amount[position][index] = 0;
        }
        else if(tokenType == 12)
        {
            _dpk_USDT_LP_UnLock_Amount[position][index] = 0;
        }

    }

}