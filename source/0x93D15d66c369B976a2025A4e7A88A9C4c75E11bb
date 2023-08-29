/*
   ____ _____ _   _ _____   _____ ___  _  _______ _   _ 
  / ___| ____| \ | |_   _| |_   _/ _ \| |/ / ____| \ | |
 | |   |  _| |  \| | | |     | || | | | ' /|  _| |  \| |
 | |___| |___| |\  | | |     | || |_| | . \| |___| |\  |
  \____|_____|_| \_| |_|     |_| \___/|_|\_\_____|_| \_|
                                                                                                               
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

// Interface for BEP20 Token Standard
interface IBEP20 {
    // Standard functions
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    // Additional features
    function mint(address account, uint256 amount) external returns (bool);
    function burn(uint256 amount) external;
    function pause() external;
    function unpause() external;
    function lock(address account, uint256 amount, uint256 until) external;
    function unlock(address account) external;
    function recoverTokens(address tokenAddress, uint256 amount) external returns (bool);

    // Events
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Mint(address indexed account, uint256 amount);
    event Burn(uint256 amount);
    event PausingEnabled(bool enabled);
    event Paused(address account);
    event Unpaused(address account);
    event Lock(address indexed account, uint256 amount, uint256 until);
}

// Cent Token contract implementing the BEP20 Token Standard
contract CentToken is IBEP20 {
    string public name = "Cent Token";
    string public symbol = "CTPAY";
    uint8 public decimals = 18;
    uint256 public totalSupply = 100000000 * 10**uint256(decimals);
    uint256 public ownerLockAmount = totalSupply / 5; // 20% of total supply locked for the owner
    address public owner = msg.sender;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public isLocked;
    mapping(address => uint256) public lockAmount;
    mapping(address => uint256) public lockedUntil;

    bool public mintingFinished;
    bool public pausingEnabled;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    modifier notLocked(address account) {
        require(!isLocked[account], "Account is locked");
        _;
    }

    modifier pausingNotEnabled() {
        require(!pausingEnabled, "Pausing is enabled");
        _;
    }

    modifier pausingEnabledOnly() {
        require(pausingEnabled, "Pausing is not enabled");
        _;
    }

    constructor() {
        balanceOf[owner] = totalSupply - ownerLockAmount;
        balanceOf[address(this)] = ownerLockAmount;
    }

    /**
     * @dev Transfer tokens from the sender's address to the recipient's address.
     * @param to The recipient's address.
     * @param value The amount of tokens to transfer.
     * @return A boolean value indicating whether the transfer was successful or not.
     */
    function transfer(address to, uint256 value) external override notLocked(msg.sender) returns (bool) {
        require(to != address(0), "Invalid recipient");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }

    /**
     * @dev Approve the spender to spend the specified amount of tokens on behalf of the sender.
     * @param spender The address allowed to spend tokens.
     * @param value The amount of tokens approved for spending.
     * @return A boolean value indicating whether the approval was successful or not.
     */
    function approve(address spender, uint256 value) external override notLocked(msg.sender) returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    /**
     * @dev Transfer tokens from the owner's address to the recipient's address on behalf of the owner.
     * @param from The owner's address.
     * @param to The recipient's address.
     * @param value The amount of tokens to transfer.
     * @return A boolean value indicating whether the transfer was successful or not.
     */
    function transferFrom(address from, address to, uint256 value) external override notLocked(from) returns (bool) {
        require(to != address(0), "Invalid recipient");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Insufficient allowance");

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }

    /**
     * @dev Mint new tokens and add them to the specified account.
     * @param account The account to receive the newly minted tokens.
     * @param amount The amount of tokens to mint.
     * @return A boolean value indicating whether the minting was successful or not.
     */
    function mint(address account, uint256 amount) external override onlyOwner pausingNotEnabled returns (bool) {
        require(!mintingFinished, "Minting is finished");
        require(account != address(0), "Invalid account address");
        require(amount > 0, "Amount must be greater than zero");

        balanceOf[account] += amount;
        totalSupply += amount;

        emit Mint(account, amount);
        emit Transfer(address(0), account, amount);

        return true;
    }

    /**
     * @dev Finish the minting process, preventing any further minting of tokens.
     * @return A boolean value indicating whether finishing the minting was successful or not.
     */
    function finishMinting() external onlyOwner pausingNotEnabled returns (bool) {
        require(!mintingFinished, "Minting already finished");

        mintingFinished = true;

        return true;
    }

    /**
     * @dev Burn tokens from the sender's address.
     * @param amount The amount of tokens to burn.
     */
    function burn(uint256 amount) external override notLocked(msg.sender) {
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");

        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;

        emit Burn(amount);
        emit Transfer(msg.sender, address(0), amount);
    }

    /**
     * @dev Pause token transfers.
     */
    function pause() external override onlyOwner pausingNotEnabled {
        pausingEnabled = true;

        emit PausingEnabled(true);
        emit Paused(msg.sender);
    }

    /**
     * @dev Unpause token transfers.
     */
    function unpause() external override onlyOwner pausingEnabledOnly {
        pausingEnabled = false;

        emit PausingEnabled(false);
        emit Unpaused(msg.sender);
    }

    /**
     * @dev Lock tokens of the specified account for a specified duration.
     * @param account The address whose tokens will be locked.
     * @param amount The amount of tokens to lock.
     * @param until The timestamp until which the tokens will be locked.
     */
    function lock(address account, uint256 amount, uint256 until) external override onlyOwner pausingNotEnabled {
        require(account != address(0), "Invalid account address");
        require(amount > 0, "Amount must be greater than zero");

        isLocked[account] = true;
        lockAmount[account] = amount;
        lockedUntil[account] = until;

        emit Lock(account, amount, until);
    }

    /**
     * @dev Unlock tokens of the specified account.
     * @param account The address whose tokens will be unlocked.
     */
    function unlock(address account) external override onlyOwner pausingNotEnabled {
        isLocked[account] = false;
        lockAmount[account] = 0;
        lockedUntil[account] = 0;
    }

    /**
     * @dev Recover tokens mistakenly sent to the contract.
     * @param tokenAddress The address of the token to recover.
     * @param amount The amount of tokens to recover.
     * @return A boolean value indicating whether the token recovery was successful or not.
     */
    function recoverTokens(address tokenAddress, uint256 amount) external override onlyOwner pausingNotEnabled returns (bool) {
        require(tokenAddress != address(this), "Cannot recover Cent Tokens");
        IBEP20 token = IBEP20(tokenAddress);
        require(token.transfer(msg.sender, amount), "Token transfer failed");
        return true;
    }
}