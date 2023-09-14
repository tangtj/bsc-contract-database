// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

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

// File: HPC_Drops.sol



pragma solidity ^0.8.17;

// Import the ERC20 interface


contract HPC_drops{
    // Address of the ERC20 token contract
    address public tokenAddress;

    // Address of the owner of this contract
    address public owner;

    // Maximum tokens a wallet can claim
    uint256 public claimableAirdrops= 500 * 10**8; // 500 tokens

    // Fee in wei for each claim
    uint256 public wad = 0.0097 ether;

    // Mapping to track the claimed tokens for each address
    mapping(address => uint256) public claimedTokens;

    // Mapping to track claimed addresses
    mapping(address => bool) public hasClaimed;

    // Declare the array to store all addresses that have claimed tokens
    address[] public allAddresses;

    // Boolean to track whether the contract is paused or not
    bool public paused = true;

    // Events
    event TokensClaimed(address indexed user, uint256 amount);
    event EthWithdrawn(address indexed user, uint256 amount);
    event TokensWithdrawn(address indexed owner, address indexed tokenAddress, uint256 amount);

    // Constructor to set the token address and contract owner
    constructor(address _tokenAddress) {
        tokenAddress = _tokenAddress;
        owner = msg.sender;
    }

       // Modifier to restrict access to the owner
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    // Modifier to check if the contract is not paused
    modifier whenNotPaused() {
        require(!paused, "Contract is paused");
        _;
    }

    // Modifier to check if the contract is paused
    modifier whenPaused() {
        require(paused, "Contract is not paused");
        _;
    }

    // Function to pause the contract (only the owner can call this)
    function pause() external onlyOwner {
        paused = true;
    }

    // Function to unpause the contract (only the owner can call this)
    function unpause() external onlyOwner {
        paused = false;
    }


    // To update reward token contract
    function updateTokenAddress(address newTokenAddress) external onlyOwner {
        tokenAddress = newTokenAddress;
    }

    // Function to change the maximum claim amount
    function setclaimableAirdrops(uint256 _claimableAirdrops) external onlyOwner {
        claimableAirdrops = _claimableAirdrops;
    }

    // Function to change the claim fee
    function setWad(uint256 _wad) external onlyOwner {
        wad = _wad;
    }

    // Function to allow users to claim tokens
    function claimTokens() external payable whenNotPaused {
        require(msg.sender != address(0), "Invalid sender address");
        require(!hasClaimed[msg.sender], "You have already claimed tokens");
        require(claimedTokens[msg.sender] < claimableAirdrops, "You have reached the maximum claim limit");
        require(msg.value >= wad, "Please send the correct wad");

        IERC20 token = IERC20(tokenAddress);
        uint256 remainingAmount = claimableAirdrops - claimedTokens[msg.sender];

        // Calculate the amount to claim, limited to the remaining amount
        uint256 claimAmount = remainingAmount < token.balanceOf(address(this)) ? remainingAmount : token.balanceOf(address(this));

        // Check if the contract has enough tokens to fulfill the claim
        require(token.balanceOf(address(this)) >= claimAmount, "Contract balance is insufficient for the claim");

        // Update the claimed amount (before transferring tokens)
        claimedTokens[msg.sender] += claimAmount;

        // Mark the address as claimed (before transferring tokens)
        hasClaimed[msg.sender] = true;

        // Add the address to the list of claimed addresses
        allAddresses.push(msg.sender);

        // Forward the claim fee to the contract owner
        payable(owner).transfer(wad);

        // Transfer tokens to the user (after updating claimedTokens and hasClaimed)
        require(token.transfer(msg.sender, claimAmount), "Token transfer failed");

        emit TokensClaimed(msg.sender, claimAmount);

    }

    // Function to withdraw excess ETH from the contract (only the owner can call this)
    function withdrawEth() external onlyOwner {
        require(address(this).balance > 0, "ETH balance is zero.");
        uint256 amountToWithdraw = address(this).balance;
        payable(owner).transfer(amountToWithdraw);

        emit EthWithdrawn(owner, amountToWithdraw);
    }

   // Function to withdraw tokens from the contract (only the owner can call this)
    function withdrawTokens(address tokenAddy, uint256 amount) external onlyOwner {
        IERC20 token = IERC20(tokenAddy);
        uint256 balance = token.balanceOf(address(this));
        require(balance >= amount, "Insufficient tokens to withdraw");
        require(token.transfer(owner, amount), "Token transfer failed");

        emit TokensWithdrawn(owner, tokenAddy, amount);
    }

    // Fallback function to receive ETH sent to the contract
    receive() external payable {
        }

    function dropsBalances() public view returns (uint256) {
        IERC20 token = IERC20(tokenAddress);
        return token.balanceOf(address(this));
    }

    function ethBalances() public view returns (uint256) {
        return address(this).balance;
    }
    // Function to get the total number of tokens claimed
    function dropsClaimed() public view returns (uint256) {
        uint256 totalClaimed = 0;
        for (uint256 i = 0; i < allAddresses.length; i++) {
            address user = allAddresses[i];
            if (hasClaimed[user]) {
                totalClaimed += claimedTokens[user];
            }
        }
        return totalClaimed;
    }

    // Function to clean the claimedTokens mapping
    function resetClaimedTokens() external onlyOwner {
        for (uint256 i = 0; i < allAddresses.length; i++) {
            claimedTokens[allAddresses[i]] = 0;
        }
    }

    // Function to clean the hasClaimed mapping
    function resetHasClaimed() external onlyOwner {
        for (uint256 i = 0; i < allAddresses.length; i++) {
            hasClaimed[allAddresses[i]] = false;
        }
    }

    // Total participants
    function totalParticipants() public view returns (uint256) {
    return allAddresses.length;
        }
    }