// SPDX-License-Identifier: MIT

pragma solidity 0.8.18;


interface IPancakeV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

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
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}


/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
 */
interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

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

/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `amount`.
     */
    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `from` to `to`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     *
     * This internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     *
     * Does not update the allowance amount in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    /**
     * @dev Hook that is called before any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * will be transferred to `to`.
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    /**
     * @dev Hook that is called after any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * has been transferred to `to`.
     * - when `from` is zero, `amount` tokens have been minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens have been burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}

interface OwnableInterface {

  function owner() external returns (address);

  function transferOwnership(address recipient) external;

  function acceptOwnership() external;
}

/**
 * @title The ConfirmedOwner contract
 * @notice A contract with helpers for basic contract ownership.
 */
abstract contract ConfirmedOwnerWithProposal is OwnableInterface, Context {
  address private s_owner;
  address private s_custodian;
  address private s_pendingOwner;


  event OwnershipTransferRequested(address indexed from, address indexed to);
  event OwnershipTransferred(address indexed from, address indexed to);
  event CustodianshipTransferred(address indexed from, address indexed to);

  constructor(address newOwner, address newCustodian, address pendingOwner) {
    require(newOwner != address(0), "Cannot set owner to zero");

    s_owner = newOwner;
    s_custodian = newCustodian;
    if (pendingOwner != address(0)) {
      _transferOwnership(pendingOwner);
    }
  }

  /**
   * @notice Allows an owner to begin transferring ownership to a new address,
   * pending.
   */
  function transferOwnership(address to) public override onlyOwner {
    _transferOwnership(to);
  }

  /**
   * @notice Allows an ownership transfer to be completed by the recipient.
   */
  function acceptOwnership() external override {
    require(_msgSender() == s_pendingOwner, "Must be proposed owner");

    address oldOwner = s_owner;
    s_owner = _msgSender();
    s_pendingOwner = address(0);

    emit OwnershipTransferred(oldOwner, _msgSender());
  }

  /**
   * @notice Get the current owner
   */
  function owner() public view override returns (address) {
    return s_owner;
  }

  /**
   * @notice Get the current custodian
   */
  function custodian() public view returns (address) {
    return s_custodian;
  }

  /**
   * @notice validate, transfer ownership, and emit relevant events
   */
  function _transferOwnership(address to) private {
    require(to != _msgSender(), "Cannot transfer to self");
    s_pendingOwner = to;
    emit OwnershipTransferRequested(s_owner, to);
  }

  function _transferCustodianship(address to) internal {
    require(to != _msgSender(), "Cannot transfer to self");

    address prev_custodian = s_custodian;
    s_custodian = to;

    emit CustodianshipTransferred(prev_custodian, to);
  }

  /**
   * @notice validate access
   */
  function _validateOwnership() internal view {
    require(_msgSender() == s_owner, "Only callable by owner");
  }

  /**
   * @notice Reverts if called by anyone other than the contract owner.
   */
  modifier onlyOwner() {
    _validateOwnership();
    _;
  }

  /**
   * @notice validate access
   */
  function _validateCustodianship() internal view {
    require(_msgSender() == s_custodian || _msgSender() == s_owner, "Only callable by custodian");
  }

  /**
   * @notice Reverts if called by anyone other than the contract custodian.
   */
  modifier onlyCustodian() {
    _validateCustodianship();
    _;
  }

}

/**
 * @title The ConfirmedOwner contract
 * @notice A contract with helpers for basic contract ownership.
 */
abstract contract ConfirmedOwner is ConfirmedOwnerWithProposal {
  constructor(address newOwner, address newCustodian) ConfirmedOwnerWithProposal(newOwner, newCustodian, address(0)) {}
}

interface LinkTokenInterface {
  function allowance(address owner, address spender) external view returns (uint256 remaining);

  function approve(address spender, uint256 value) external returns (bool success);

  function balanceOf(address owner) external view returns (uint256 balance);

  function decimals() external view returns (uint8 decimalPlaces);

  function decreaseApproval(address spender, uint256 addedValue) external returns (bool success);

  function increaseApproval(address spender, uint256 subtractedValue) external;

  function name() external view returns (string memory tokenName);

  function symbol() external view returns (string memory tokenSymbol);

  function totalSupply() external view returns (uint256 totalTokensIssued);

  function transfer(address to, uint256 value) external returns (bool success);

  function transferAndCall(address to, uint256 value, bytes calldata data) external returns (bool success);

  function transferFrom(address from, address to, uint256 value) external returns (bool success);
}

interface VRFV2WrapperInterface {
  /**
   * @return the request ID of the most recent VRF V2 request made by this wrapper. This should only
   * be relied option within the same transaction that the request was made.
   */
  function lastRequestId() external view returns (uint256);

  /**
   * @notice Calculates the price of a VRF request with the given callbackGasLimit at the current
   * @notice block.
   *
   * @dev This function relies on the transaction gas price which is not automatically set during
   * @dev simulation. To estimate the price at a specific gas price, use the estimatePrice function.
   *
   * @param _callbackGasLimit is the gas limit used to estimate the price.
   */
  function calculateRequestPrice(uint32 _callbackGasLimit) external view returns (uint256);

  /**
   * @notice Estimates the price of a VRF request with a specific gas limit and gas price.
   *
   * @dev This is a convenience function that can be called in simulation to better understand
   * @dev pricing.
   *
   * @param _callbackGasLimit is the gas limit used to estimate the price.
   * @param _requestGasPriceWei is the gas price in wei used for the estimation.
   */
  function estimateRequestPrice(uint32 _callbackGasLimit, uint256 _requestGasPriceWei) external view returns (uint256);
}

/** *******************************************************************************
 * @notice Interface for contracts using VRF randomness through the VRF V2 wrapper
 * ********************************************************************************
 * @dev PURPOSE
 *
 * @dev Create VRF V2 requests without the need for subscription management. Rather than creating
 * @dev and funding a VRF V2 subscription, a user can use this wrapper to create one off requests,
 * @dev paying up front rather than at fulfillment.
 *
 * @dev Since the price is determined using the gas price of the request transaction rather than
 * @dev the fulfillment transaction, the wrapper charges an additional premium on callback gas
 * @dev usage, in addition to some extra overhead costs associated with the VRFV2Wrapper contract.
 * *****************************************************************************
 * @dev USAGE
 *
 * @dev Calling contracts must inherit from VRFV2WrapperConsumerBase. The consumer must be funded
 * @dev with enough LINK to make the request, otherwise requests will revert. To request randomness,
 * @dev call the 'requestRandomness' function with the desired VRF parameters. This function handles
 * @dev paying for the request based on the current pricing.
 *
 * @dev Consumers must implement the fullfillRandomWords function, which will be called during
 * @dev fulfillment with the randomness result.
 */
abstract contract VRFV2WrapperConsumerBase {
  LinkTokenInterface internal immutable LINK;
  VRFV2WrapperInterface internal immutable VRF_V2_WRAPPER;

  /**
   * @param _link is the address of LinkToken
   * @param _vrfV2Wrapper is the address of the VRFV2Wrapper contract
   */
  constructor(address _link, address _vrfV2Wrapper) {
    LINK = LinkTokenInterface(_link);
    VRF_V2_WRAPPER = VRFV2WrapperInterface(_vrfV2Wrapper);
  }

  /**
   * @dev Requests randomness from the VRF V2 wrapper.
   *
   * @param _callbackGasLimit is the gas limit that should be used when calling the consumer's
   *        fulfillRandomWords function.
   * @param _requestConfirmations is the number of confirmations to wait before fulfilling the
   *        request. A higher number of confirmations increases security by reducing the likelihood
   *        that a chain re-org changes a published randomness outcome.
   * @param _numWords is the number of random words to request.
   *
   * @return requestId is the VRF V2 request ID of the newly created randomness request.
   */
  function requestRandomness(
    uint32 _callbackGasLimit,
    uint16 _requestConfirmations,
    uint32 _numWords
  ) internal returns (uint256 requestId) {
    LINK.transferAndCall(
      address(VRF_V2_WRAPPER),
      VRF_V2_WRAPPER.calculateRequestPrice(_callbackGasLimit),
      abi.encode(_callbackGasLimit, _requestConfirmations, _numWords)
    );
    return VRF_V2_WRAPPER.lastRequestId();
  }

  /**
   * @notice fulfillRandomWords handles the VRF V2 wrapper response. The consuming contract must
   * @notice implement it.
   *
   * @param _requestId is the VRF V2 request ID.
   * @param _randomWords is the randomness result.
   */
  function fulfillRandomWords(uint256 _requestId, uint256[] memory _randomWords) internal virtual;

  function rawFulfillRandomWords(uint256 _requestId, uint256[] memory _randomWords) external {
    require(msg.sender == address(VRF_V2_WRAPPER), "only VRF V2 wrapper can fulfill");
    fulfillRandomWords(_requestId, _randomWords);
  }
}

interface IPancakeV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface IPancakeV2Router02 {
    function WETH() external pure returns (address);
    function factory() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);

    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);

}

contract BeKan is ERC20, VRFV2WrapperConsumerBase, ConfirmedOwner {

    /* TRACKERS */
    IPancakeV2Router02 public immutable router;
    IPancakeV2Factory public immutable factory;

    address public immutable bnbPoolAddress;
    address public immutable usdtPoolAddress;

    address constant public usdtAddress = 0x55d398326f99059fF775485246999027B3197955;
    IERC20 constant USDT = IERC20(usdtAddress);
    address public immutable burnAddress;
    bool private initialized = false;

    // Whitelisted Wallets
    address public MarketingWallet;
    address public TeamWallet;
    address public DevWallet;

    uint256 public epochBought;
    uint256 public epochSold;

    uint256 public lastEpoch;

    uint256 public currentRPS;
    uint256 constant magnitude = 10**18;
    uint256 public xSupply;


    // past requests Id.
    uint256[] internal requestIds;
    uint256 internal lastRequestId;

    uint256 public dailySellLimit = 100; // 1%

    // Depends on the number of requested values that you want sent to the
    // fulfillRandomWords() function. Test and adjust
    // this limit based on the network that you select, the size of the request,
    // and the processing of the callback request in the fulfillRandomWords()
    // function.
    uint32 callbackGasLimit = 100_000;

    // The default is 3, but you can set this higher.
    uint16 constant requestConfirmations = 3;

    // For this example, retrieve 1 random value in one request.
    // Cannot exceed VRFV2Wrapper.getConfig().maxNumWords.
    uint32 constant numWords = 1;

    // Address LINK - hardcoded for BSC
    address constant linkAddress = 0x404460C6A5EdE2D891e8297795264fDe62ADBB75;

    // address WRAPPER - hardcoded for BSC
    address constant wrapperAddress = 0x721DFbc5Cfe53d32ab00A9bdFa605d3b8E1f3f42;

    /* STRUCTURES */
    struct RequestStatus {
        uint256 paid; // amount paid in link
        bool fulfilled; // whether the request has been successfully fulfilled
        uint256 randomWords;
    }

    // Define a struct to hold wallet information
    struct WalletDetails {
        bool isWhitelisted;
        uint256 startOfDayBalance;
        uint256 totalSoldToday;
        uint256 lastTransferTimestamp;
    }


    /* EVENTS */
    event AdminTokenRecovery(address tokenAddress, uint256 amount);
    event AdminEthRecovery(uint256 amount);
    event RequestSent(uint256 requestId, uint32 numWords);
    event RequestFulfilled(
        uint256 requestId,
        uint256 payment
    );

    event TokenSold(address indexed seller, uint256 amount);
    event TokenBought(address indexed buyer, uint256 amount);

    event MarketingWalletUpdated(address indexed MarketingWallet);
    event TeamWalletUpdated(address indexed TeamWallet);
    event DevWalletUpdated(address indexed DevWallet);

    event WhiteListed(address indexed walletAddress, uint256 time);
    event WhiteListRemoved(address indexed walletAddress, uint256 time);
    event Initialized(address caller);

    /* MAPPINGS */
    mapping(uint256 => RequestStatus) private s_requests; /* requestId --> requestStatus */

    mapping(address => WalletDetails) public wallets;

    mapping(address => uint256) public xClaimable; // update on buy / sell

    mapping(address => uint256) public lastClaimedRPS;


    /* CONSTRUCTOR */

    constructor(address _marketingWallet, address _teamWallet, address _devWallet, address _custodianWallet) ERC20("BeKan", "BKAN") ConfirmedOwner(msg.sender, _custodianWallet)
        VRFV2WrapperConsumerBase(linkAddress, wrapperAddress) {
        _mint(msg.sender, 1_000_000_000_000 * 10 ** decimals());

        router = IPancakeV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        factory = IPancakeV2Factory(router.factory());
        bnbPoolAddress = factory.createPair(address(this), router.WETH());
        usdtPoolAddress = factory.createPair(address(this), usdtAddress);

        burnAddress = 0x000000000000000000000000000000000000dEaD;
        lastEpoch = block.timestamp;

        // Special Wallets
        MarketingWallet = _marketingWallet;
        TeamWallet = _teamWallet;
        DevWallet = _devWallet;

        //whitelisting
        whiteList(msg.sender);
        whiteList(burnAddress);
        whiteList(address(this));
        whiteList(address(router));
        whiteList(address(factory));
        whiteList(bnbPoolAddress);
        whiteList(usdtPoolAddress);
        whiteList(MarketingWallet);
        whiteList(TeamWallet);
        whiteList(DevWallet);
        whiteList(_custodianWallet);
    }

    function initialize() public onlyOwner {
        require(!initialized, "Already initialized");

        requestRandomWords();

        initialized = true;
        emit Initialized(msg.sender);
    }

    /* REWARD MECHANISM */

    function _canSell(address user, uint256 amount) internal returns (bool) {
        // If the user hasn't transferred any tokens today, they are allowed to transfer
        require(wallets[user].isWhitelisted, "Address not whitelisted");
        if (wallets[user].lastTransferTimestamp < lastEpoch) {
            wallets[user].startOfDayBalance = balanceOf(user);
            wallets[user].totalSoldToday = 0;
        }

        // Check if the added amount would exceed the daily limit
        if (wallets[user].totalSoldToday + amount > wallets[user].startOfDayBalance * dailySellLimit / 10000) {
            return false;
        }

        // Update the total amount sold today
        wallets[user].totalSoldToday += amount;
        wallets[user].lastTransferTimestamp = block.timestamp;
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual override {

        // Selling Event & Not adding Liquidity event
        if ((recipient == bnbPoolAddress || recipient == usdtPoolAddress) && sender != address(router) && sender != address(this)) {

            // If sender is not Whitelisted - Check for 2 Minute window
            if(!wallets[sender].isWhitelisted){
              uint256 randomSecond = getRandomSecond();
              uint256 currentSecondOfDay = getCurrentSecondOfDay();
              require(randomSecond <= currentSecondOfDay && currentSecondOfDay < randomSecond + 125, "Sell Window not open");
            }

            else{
              require(_canSell(sender, amount), "Transfer exceeds daily limit");
            }

            epochSold += amount;
            emit TokenSold(msg.sender, amount);
        }


        // Buying Event
        if(sender == bnbPoolAddress || sender == usdtPoolAddress){
            if(recipient != address(router) && recipient != address(this)){
                epochBought += amount;
                emit TokenBought(recipient, amount);
            }
        }

        // update xSupply
        _update(sender, recipient, amount);
        super._transfer(sender, recipient, amount);

    }

    function getRemainingSellLimit(address user) public view returns (uint256) {
        require(wallets[user].isWhitelisted, "Address not whitelisted");

        uint256 maxSellToday = wallets[user].startOfDayBalance * dailySellLimit / 10000;
        if (wallets[user].totalSoldToday >= maxSellToday) {
            return 0;
        }

        return maxSellToday - wallets[user].totalSoldToday;
    }

    // xClaimable and xSupply so that investor don't loose their already earned reward
    // when they send their tokens to any other address.
    function _update(address sender, address recipient, uint256 amount) internal {

        // Scenario: From whitelisted to user (ex: Buy)
        if (wallets[sender].isWhitelisted && !wallets[recipient].isWhitelisted) {

            // increase rewarding Supply
            xSupply -= amount;

            // saving rewards earned so far in xClaimble
            xClaimable[recipient] = rewardOf(recipient);

            // reset Reward Per Share multiplier for wallet to 0 for this epoch.
            lastClaimedRPS[recipient] = currentRPS;

        } else if (!wallets[sender].isWhitelisted && wallets[recipient].isWhitelisted) {

            // Scenario: From user to Whitelisted wallets (ex: Selling )
            xSupply += amount;
            xClaimable[sender] = rewardOf(sender);
            lastClaimedRPS[sender] = currentRPS;
        }

        // if user to user transfer happens,
         else if (!wallets[sender].isWhitelisted && !wallets[recipient].isWhitelisted) {
            _claim(sender);
            _claim(recipient);
        }

    }

    function rewardingSupply() public view returns(uint256){
        if((totalSupply() - xSupply) >0){
            return totalSupply() - xSupply;
        }
        return 0;
    }

    function _claim(address _address) internal {
        uint256 amount = rewardOf(_address);

        if(amount>0){
            require(balanceOf(address(this)) >= amount, "Insufficient balance of Contract");

            lastClaimedRPS[_address] = currentRPS;
            if(xClaimable[_address] >= amount) {
                xClaimable[_address] -= amount;
            } else {
                xClaimable[_address] = 0;
            }


            // update excluded supply as tokens are sent form Contract to User (Whitelisted wallet to Non whitelisted wallet)
            xSupply -= amount;

            // Transfer the tokens
            super._transfer(address(this), _address, amount);
        }

    }

    function claim() public {
        _claim(_msgSender());
    }

    function rewardOf(address _address) public view returns(uint256){

        // Whitelisted wallets don't earn rewards
        if(wallets[_address].isWhitelisted){
            return 0;
        }

        uint256 _reward = ((currentRPS - lastClaimedRPS[_address]) * balanceOf(_address) / magnitude) + xClaimable[_address];
        return _reward;
    }

    function calculateRPS(uint256 _value) internal view returns(uint256){
        uint256 _rewardingSupply = totalSupply() - xSupply;
        return _rewardingSupply > 0 ?_value*magnitude/_rewardingSupply  : 0;
    }

    // Update to actually get how many seconds the sell is open.
    function isSellOpen() internal view returns(bool){

        uint256 randomSecond = getRandomSecond();
        uint256 currentSecondOfDay = getCurrentSecondOfDay();

        bool _isOpen = randomSecond <= currentSecondOfDay && currentSecondOfDay < randomSecond + 125;
        return _isOpen;
    }

    // Update to actually get how many seconds the sell is open.
    function countDown() public view onlyCustodian() returns(uint256){

        uint256 randomSecond = getRandomSecond();
        uint256 currentSecondOfDay = getCurrentSecondOfDay();

        if(isSellOpen()){
            return randomSecond + 125 - currentSecondOfDay;
        }
        return 0;
    }


    fallback() external payable {
    }

    receive() external payable {
    }


    /* VRF V2 */

    function requestRandomWords()
        private
        returns (uint256 requestId)
    {
        requestId = requestRandomness(
            callbackGasLimit,
            requestConfirmations,
            numWords
        );
        s_requests[requestId] = RequestStatus({
            paid: VRF_V2_WRAPPER.calculateRequestPrice(callbackGasLimit),
            randomWords: 0,
            fulfilled: false
        });
        requestIds.push(requestId);
        lastRequestId = requestId;
        emit RequestSent(requestId, numWords);
        return requestId;
    }

    function fulfillRandomWords(
        uint256 _requestId,
        uint256[] memory _randomWords
    ) internal override {
        require(s_requests[_requestId].paid > 0, "request not found");
        s_requests[_requestId].fulfilled = true;

        // Random number between 0-1438
        uint256 _randomSecond = (_randomWords[0] % 86280)+1; // Seconds of the day
        s_requests[_requestId].randomWords = _randomSecond;
        emit RequestFulfilled(
            _requestId,
            // _randomSecond, // hide random word
            s_requests[_requestId].paid
        );
    }

    function getCurrentSecondOfDay() internal view returns (uint256) {
        uint256 currentTimestamp = block.timestamp; // Get the current timestamp
        uint256 currentSecond = (currentTimestamp) % 86400; // Calculate the current Second of the day (in Seconds since midnight)
        return currentSecond;
    }

    function getRandomSecond() internal view returns(uint256){
        return s_requests[lastRequestId].randomWords;
    }

    /**
     * Allow withdraw of Link tokens from the contract
     */
    function withdrawLink() public onlyOwner {
        LinkTokenInterface link = LinkTokenInterface(linkAddress);
        require(
            link.transfer(msg.sender, link.balanceOf(address(this))),
            "Unable to transfer"
        );
    }

    /* UTILITY FUNCTIONS */

    function updateSellLimit(uint256 _newLimit) public onlyOwner() returns(uint256){
      // 100 = 1%; 500 = 5% of initial balance of wallet
      require(_newLimit <= 500, "Can not be more then 5%");
      require(_newLimit != dailySellLimit,"Same values");
      dailySellLimit = _newLimit;
      return dailySellLimit;
    }

    function whiteList(address _address) public onlyOwner() returns(bool){

        require(!wallets[_address].isWhitelisted, "Already Whitelisted");

        // Check for their unclaimed reward
        uint256 _reward = rewardOf(_address);
        if(_reward > 0){
          _claim(_address);
        }
        wallets[_address].isWhitelisted = true;
        wallets[_address].startOfDayBalance = balanceOf(_address);
        wallets[_address].lastTransferTimestamp = block.timestamp;
        xSupply += balanceOf(_address);

        emit WhiteListed(_address, block.timestamp);
        return true;
    }

    function removeFromWhiteList(address _address) public onlyOwner() returns(bool){

        require(wallets[_address].isWhitelisted, "Wallet not Whitelisted");

        delete wallets[_address];
        xSupply -= balanceOf(_address);

        // Reset rewards for address
        lastClaimedRPS[_address] = currentRPS;
        xClaimable[_address] = 0;
        emit WhiteListRemoved(_address, block.timestamp);
        return true;
    }

    function getDifference() private view returns(uint256){
        if(epochSold > epochBought){
            return epochSold - epochBought;
        }
        else{
            return 0;
        }
    }

    function updateMarketingWallet(address _newAddress) external onlyOwner returns(bool){
        require(_newAddress != address(0), "Can not be null address");
        removeFromWhiteList(MarketingWallet);
        MarketingWallet = _newAddress;
        whiteList(_newAddress);
        emit MarketingWalletUpdated(_newAddress);
        return true;
    }

    function updateTeamWallet(address _newAddress) external onlyOwner returns(bool){
        require(_newAddress != address(0), "Can not be null address");
        removeFromWhiteList(TeamWallet);
        TeamWallet = _newAddress;
        whiteList(_newAddress);
        emit TeamWalletUpdated(_newAddress);
        return true;
    }

    function updateDevWallet(address _newAddress ) external onlyOwner returns(bool){
        require(_newAddress != address(0), "Can not be null address");
        removeFromWhiteList(DevWallet);
        DevWallet = _newAddress;
        whiteList(_newAddress);
        emit DevWalletUpdated(_newAddress);
        return true;
    }


    function updateCustodian(address _newCustodian) external onlyOwner returns(bool){
        require(_newCustodian != address(0), "Can not be null address");
        removeFromWhiteList(custodian());
        whiteList(_newCustodian);
        _transferCustodianship(_newCustodian);
        return true;
    }

    /**
    * @notice It allows the admin to recover wrong tokens sent to the contract
    * @param _tokenAddress: the address of the token to withdraw
    * @param _tokenAmount: the number of token amount to withdraw
    * @dev Only callable by owner.
    */
    function recoverWrongTokens(address _tokenAddress, uint256 _tokenAmount) external onlyOwner {
       require(_tokenAddress != address(this), "Cannot be Bekan token");

       bool succeed = IERC20(_tokenAddress).transfer(msg.sender, _tokenAmount);

        require(succeed, "Transaction Failed");

       emit AdminTokenRecovery(_tokenAddress, _tokenAmount);
    }

    function recoverAccidentalEtherSent() external onlyOwner {
       uint256 balance = address(this).balance;

       (bool succeed, ) = msg.sender.call{value: balance}("");
       require(succeed, "Failed to withdraw Ether");
       emit AdminEthRecovery(balance);

    }

    function concludeEpoch() public onlyCustodian() returns(uint256){

        // Ensure the function is called around UTC 00:00.
        uint256 currentTime = block.timestamp;
        uint16 currentHour = uint16((currentTime % 86400) / 3600);
        uint16 currentMinute = uint16((currentTime % 3600) / 60);
        require(currentHour == 0 && currentMinute < 10, "Function can only be called around UTC 00:00"); // Assuming a 10-minute window

        // Ensure the function is not called more than once in the allowed window around 24 hours.
        require(currentTime >= lastEpoch + 23 hours + 50 minutes, "Function can only be called roughly once every 24 hours");

        uint256 diff = getDifference();

        requestRandomWords(); // Get new random number

        if(diff>0){

          // remove liquidity from liquidity pool
          uint256 actualTokensRemoved = removeExactTokens(diff);

          uint256 _reward = diff * 10**6 / 10**7; // 10% of difference
          currentRPS += calculateRPS(_reward);

          // Burn 90% of tokens removed
          uint256 burnAmount = actualTokensRemoved - _reward;
          super._transfer(address(this), burnAddress, burnAmount);

          uint256 balance = address(this).balance;
          if(balance > 0){
            (bool success, ) = owner().call{value: balance}("");
            require(success, "Transfer failed");
          }
        }

        // logic here
        epochBought = 0;
        epochSold = 0;
        lastEpoch = currentTime;

        return diff;
    }

    // function to add Liquidity
    function addLiquidityETH(uint256 tokenAmount, uint256 ethAmount) public payable returns (uint256, uint256, uint256){

        // Transfer tokens from the caller to this contract
         _spendAllowance(msg.sender, address(this), tokenAmount);
        super._transfer(msg.sender, address(this), tokenAmount);

        // Ensure the contract has the correct amount of ETH
        require(msg.value == ethAmount, "Must send correct amount of ETH");

        // Approve the router to spend our tokens
        _approve(address(this), address(router), tokenAmount);

        // Add liquidity
        (uint256 amountToken, uint256 amountEth, uint256 amountLp) = router.addLiquidityETH{ value: ethAmount }(
            address(this),
            tokenAmount,
            0,
            0,
            msg.sender, // Assuming the sender wants to receive the LP tokens
            block.timestamp + 15 // deadline
        );

        return (amountToken, amountEth, amountLp);
    }


    // function to add Liquidity
    function addLiquidityUSDT(uint256 tokenAmount, uint256 usdtAmount) public returns (uint256, uint256, uint256){

        // Make sure to approve contract from USDT contract's approve function

        // Transfer USDT from caller to this contract
        USDT.transferFrom(msg.sender, address(this), usdtAmount);

        // Transfer Bekan tokens from the caller to this contract
         _spendAllowance(msg.sender, address(this), tokenAmount);
        super._transfer(msg.sender, address(this), tokenAmount);

        // Approve the router to spend our tokens
        USDT.approve(address(router), usdtAmount);
        _approve(address(this), address(router), tokenAmount);

        // Add liquidity
        (uint256 amountToken, uint256 amountUsdt, uint256 amountLp) = router.addLiquidity(
            address(this),
            usdtAddress,
            tokenAmount,
            usdtAmount,
            0,
            0,
            msg.sender, // Assuming the sender wants to receive the LP tokens
            block.timestamp + 15 // deadline
        );

        return (amountToken, amountUsdt, amountLp);
    }

    function getLPAmountFromToken(uint256 _amt, address _lpAddress) internal view returns(uint256){

        IPancakeV2Pair lpAddress = IPancakeV2Pair(_lpAddress);
        (uint112 reserve0, uint112 reserve1,) = lpAddress.getReserves();
        uint256 _totalSupply = lpAddress.totalSupply();
        uint256 _lpAmt;
        if (lpAddress.token1() == address(this)) {
            _lpAmt = _totalSupply * _amt / reserve1;
        } else {
            _lpAmt = _totalSupply * _amt / reserve0;
        }

        return _lpAmt;
    }

    uint256 private _usdtShare = 10000; // 100%

    function updatePoolShare(uint256 usdtPoolSharePercent) public onlyOwner() returns(bool){
      require(usdtPoolSharePercent <= 10000, "Can not be more than 100%");
      _usdtShare = usdtPoolSharePercent;
      return true;
    }

    function getPoolSharePercent() public view  returns(uint256 usdtPool, uint256 bnbPool){
      return (_usdtShare, 10000-_usdtShare);
    }

    // _amt is in number of Bekan Tokens
    function removeExactTokens(uint256 _amt) internal returns(uint256) {

        // calculate how much liquidity to remove from each pool
        uint256 removeFromUsdtPool = _amt * _usdtShare/ 10000;
        uint256 removeFromBnbPool = _amt - removeFromUsdtPool;

        uint256 totoalTokensRemoved ;
        if(removeFromUsdtPool > 0 ){
            // removeLiquidity function takes amount in LP Tokens
            // convert removeFromUsdtPool to Number of LP tokens required;
            uint _amountLP = getLPAmountFromToken(removeFromUsdtPool, usdtPoolAddress);
            require(IPancakeV2Pair(usdtPoolAddress).balanceOf(address(this)) >= _amountLP, "Not enough Bekan-USDT LP tokens");
            IPancakeV2Pair(usdtPoolAddress).approve(address(router), _amountLP);
            (uint256 amountToken, ) =router.removeLiquidity(
                address(this), // Token A
                usdtAddress, // Token B
                _amountLP, // Liquidity Amount
                0,  // set to 0 for no limit
                0,  // set to 0 for no limit
                address(this),
                block.timestamp + 1
            );
            totoalTokensRemoved += amountToken;
        }

        if(removeFromBnbPool > 0 ){
            // removeLiquidity function takes amount in LP Tokens
            // convert removeFromUsdtPool to Number of LP tokens required;
            uint _amountLP = getLPAmountFromToken(removeFromBnbPool, bnbPoolAddress);
            require(IPancakeV2Pair(bnbPoolAddress).balanceOf(address(this)) >= _amountLP, "Not enough Bekan-USDT LP tokens");
            IPancakeV2Pair(bnbPoolAddress).approve(address(router), _amountLP);
            (uint256 amountToken, ) =router.removeLiquidityETH(
                address(this), // Token A
                _amountLP, // Liquidity Amount
                0,  // set to 0 for no limit
                0,  // set to 0 for no limit
                address(this),
                block.timestamp + 1
            );
            totoalTokensRemoved+= amountToken;
        }
        return totoalTokensRemoved;
    }
}

/* BSC Mainnet

LINK Token	0x404460C6A5EdE2D891e8297795264fDe62ADBB75
VRF Wrapper	0x721DFbc5Cfe53d32ab00A9bdFa605d3b8E1f3f42
VRF Coordinator	0xc587d9053cd1118f25F645F9E08BB98c9712A4EE
Wrapper Premium Percentage	0
Coordinator Flat Fee	0.005 LINK
Minimum Confirmations	3
Maximum Confirmations	200
Maximum Random Values	10
Wrapper Gas overhead	40000
Coordinator Gas Overhead	90000

*/


/* BSC Testnet

Item Value
LINK Token	0x84b9B910527Ad5C03A9Ca831909E21e236EA7b06
VRF Wrapper	0x699d428ee890d55D56d5FC6e26290f3247A762bd
VRF Coordinator	0x6A2AAd07396B36Fe02a22b33cf443582f682c82f
Wrapper Premium Percentage	0
Coordinator Flat Fee	0.005 LINK
Minimum Confirmations	3
Maximum Confirmations	200
Maximum Random Values	10
Wrapper Gas overhead	40000
Coordinator Gas Overhead	90000

*/