// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}
interface IBEP20 {
    /**
     * @dev Returns the amount of tokens in existence.
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
    function allowance(address _owner, address spender)
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
contract MUSDICO {
    address public owner;
    IBEP20 public token;
    IBEP20 public usdt;  // USDT contract
    uint256 public currentPrice = 235; // Price of tokens per BNB
    bool public presaleActive = true; 

    uint256 public totalSold = 0;
    uint256 public totalFund = 0 ether;

    constructor(address _tokenAddress, address _usdtAddress) {
        owner = msg.sender;
        token = IBEP20(_tokenAddress);
        usdt = IBEP20(_usdtAddress);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    // Allow the contract to receive Ether and convert it to tokens.
    receive() external payable {
        buyToken();
    }

    // Critical Recommendation: Implement input validation to ensure safe processing.
    function buyToken() public payable {
        require(presaleActive, "Presale is not active!");

        uint256 musdAmount = (msg.value * currentPrice) / 10; // Calculate equivalent MUSD

        require(musdAmount > 0, "Amount is too low to purchase tokens");

        require(musdAmount <= token.balanceOf(address(this)), "Insufficient tokens in contract");

        // Update the contract state before the interaction
        totalSold += musdAmount;
        totalFund += msg.value;

        // Transfer tokens to the buyer
        bool transferSuccess = token.transfer(msg.sender, musdAmount);
        require(transferSuccess, "Token transfer failed");

        emit TokensPurchased(msg.sender, musdAmount);
    }



    // Allow the owner to update the token price.
    function updatePrice(uint256 newPrice) public onlyOwner {
        currentPrice = newPrice;
    }

    // Allow the owner to withdraw accumulated Ether from the contract.
    function withdrawBNB() public onlyOwner {
        address payable ownerAddress = payable(owner);
        ownerAddress.transfer(address(this).balance);
    }
    function withdrawTokens() public onlyOwner {
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance > 0, "No tokens to withdraw");

        bool transferSuccess = token.transfer(owner, contractBalance);
        require(transferSuccess, "Token transfer failed");

        emit TokensWithdrawn(owner, contractBalance);
    }

    // Allow users to buy tokens using USDT.
    function buyTokenWithUSDT(uint256 usdtAmount) public {
        require(presaleActive, "Presale is not active!");

        // Calculate the equivalent amount of your tokens based on the current price.
        uint256 _amount = usdtAmount * currentPrice;

        // Ensure that the contract has enough tokens to fulfill the purchase.
        require(_amount <= token.balanceOf(address(this)), "ICO Sold Out");

        // Transfer USDT from the sender to the contract.
        bool usdtTransferSuccess = usdt.transferFrom(msg.sender, address(this), usdtAmount);
        require(usdtTransferSuccess, "USDT transfer failed!");

        // Transfer your tokens to the buyer.
        bool tokenTransferSuccess = token.transfer(msg.sender, _amount);
        require(tokenTransferSuccess, "Failed to transfer tokens!");
        
        // Log the tokens purchased event.
        emit TokensPurchasedWithUSDT(msg.sender, _amount);
    }

    // Important Recommendation: Log events for transparency and tracking.
    event TokensPurchased(address indexed buyer, uint256 amount);
    event TokensPurchasedWithUSDT(address indexed buyer, uint256 amount);
    event TokensWithdrawn(address indexed owner, uint256 amount);
}