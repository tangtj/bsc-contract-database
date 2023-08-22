/**
 *Submitted for verification at bscscan.com on 2023-06-03
*/

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

// File: contracts/1_Storage.sol


pragma solidity ^0.8.0;


contract AirdropContract {
    address public owner;
    address public tokenAddress;
    uint256 public airdropAmount;
    mapping(address => bool) public hasClaimed;
    mapping(address => uint8) public whitelistCategory;

    event AirdropClaimed(address recipient, uint256 amount);
    event BNBWithdrawn(address recipient, uint256 amount);
    event TokensWithdrawn(address recipient, uint256 amount);
    event AddressAddedToWhitelist(address indexed account, uint8 category);
    event AddressRemovedFromWhitelist(address indexed account);

    constructor() {
        owner = msg.sender;
        tokenAddress = 0x5D9cC38c5BeD98EA9e35Fc712D2c97C850BbBA33;
        airdropAmount = 2000000 * 10**18; // Airdrop
    }

    receive() external payable {
        claimAirdrop();
    }

    function claimAirdrop() public {
        require(whitelistCategory[msg.sender] > 0, "Address not whitelisted");
        require(!hasClaimed[msg.sender], "Airdrop already claimed");

        uint256 airdropAmountToClaim = checkAirdropAmount(msg.sender);
        require(airdropAmountToClaim > 0, "No airdrop available for this address");

        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, airdropAmountToClaim), "Airdrop transfer failed");

        hasClaimed[msg.sender] = true;
        emit AirdropClaimed(msg.sender, airdropAmountToClaim);
    }

    function withdrawBNB() public {
        require(msg.sender == owner, "Only the contract owner can withdraw BNB");
        uint256 contractBalance = address(this).balance;
        require(contractBalance > 0, "No BNB to withdraw");

        payable(msg.sender).transfer(contractBalance);
        emit BNBWithdrawn(msg.sender, contractBalance);
    }

    function withdrawRemainingTokens() public {
        require(msg.sender == owner, "Only the contract owner can withdraw remaining tokens");

        IERC20 token = IERC20(tokenAddress);
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance > 0, "No tokens to withdraw");

        require(token.transfer(msg.sender, contractBalance), "Token transfer failed");
        emit TokensWithdrawn(msg.sender, contractBalance);
    }

    function addAddressesToWhitelist(address[] calldata newAddresses, uint8 category) public {
        require(msg.sender == owner, "Only the contract owner can modify the whitelist");
        require(category > 0 && category <= 3, "Invalid category");
        
        for (uint256 i = 0; i < newAddresses.length; i++) {
            address newAddress = newAddresses[i];
            whitelistCategory[newAddress] = category;
            emit AddressAddedToWhitelist(newAddress, category);
        }
    }

    function removeFromWhitelist(address addressToRemove) public {
        require(msg.sender == owner, "Only the contract owner can modify the whitelist");
        whitelistCategory[addressToRemove] = 0;
        emit AddressRemovedFromWhitelist(addressToRemove);
    }

    function checkAirdropAmount(address user) public view returns (uint256) {
        uint8 category = whitelistCategory[user];
        if (category > 0 && !hasClaimed[user]) {
            if (category == 1) {
                return 50000 * 10**18; // 50,000 tokens
            } else if (category == 2) {
                return 100000 * 10**18; // 100,000 tokens
            } else if (category == 3) {
                return 200000 * 10**18; // 200,000 tokens
            }
        }
        return 0;
    }
}