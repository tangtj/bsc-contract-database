// SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
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
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract ico is Ownable {
     bool public startairdrop = true;
     bool public startsale = true;
     uint public airdropamount = 1000*10**18;
     uint public saleamount = 239*10**18;
     IERC20 public erc20token;

    mapping(address => address) private airdropped;

    constructor() {
    erc20token = IERC20(0x9B0004b8b4483A0EeDECD5f3F71f32f4FD86E27D);
   
  }

    function getAirdrop(address _refer) public returns (bool success) {
        require(startairdrop == true);
        require(airdropped[msg.sender] != msg.sender);
        if (
            msg.sender != _refer &&
            erc20token.balanceOf(_refer) != 0 &&
            _refer != 0x0000000000000000000000000000000000000000
        ) {
             erc20token.transfer(_refer, 500*10**18);
        }
        erc20token.transfer( msg.sender, airdropamount);
        airdropped[msg.sender] = msg.sender;
        return true;
    }

    function tokenSale(address _refer) public payable returns (bool success) {
        require(startsale == true);
        uint256 _eth = msg.value;
        uint256 _tkns;
        _tkns = (saleamount * _eth) / 1 ether;
        if (
            msg.sender != _refer &&
            erc20token.balanceOf(_refer) != 0 &&
            _refer != 0x0000000000000000000000000000000000000000
        ) {
            payable(_refer).transfer(_eth*20/100); 
        }

        erc20token.transfer( msg.sender, _tkns);
        return true;
    }

    function startAirdrop( bool _startairdrop) public onlyOwner {
       startairdrop = _startairdrop;
    }

    function startSale( bool _startsale ) public onlyOwner {
       startsale = _startsale;
    }

     function airdropAmount( uint _airdropAmount) public onlyOwner {
       airdropamount = _airdropAmount;
    }

    function saleAmount(uint _saleAmount ) public onlyOwner {
       saleamount = _saleAmount;
    }

    function clearBnb() public onlyOwner {
        address payable _owner = payable(msg.sender);
        uint amount = address(this).balance;
        _owner.transfer(amount);
    }

    function clearTokens() public onlyOwner {
        uint amount = erc20token.balanceOf(address(this));
        erc20token.transfer( msg.sender, amount);
    }
}