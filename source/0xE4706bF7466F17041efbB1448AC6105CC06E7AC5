// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC165 standard, as defined in the
 * https://eips.ethereum.org/EIPS/eip-165[EIP].
 *
 * Implementers can declare support of contract interfaces, which can then be
 * queried by others ({ERC165Checker}).
 *
 * For an implementation, see {ERC165}.
 */
interface IERC165 {
    /**
     * @dev Returns true if this contract implements the interface defined by
     * `interfaceId`. See the corresponding
     * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]
     * to learn more about how these ids are created.
     *
     * This function call must use less than 30 000 gas.
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}

/**
 * @dev Required interface of an ERC721 compliant contract.
 */
interface IERC721 is IERC165 {
    /**
     * @dev Emitted when `tokenId` token is transferred from `from` to `to`.
     */
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables `approved` to manage the `tokenId` token.
     */
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables or disables (`approved`) `operator` to manage all of its assets.
     */
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    /**
     * @dev Returns the number of tokens in ``owner``'s account.
     */
    function balanceOf(address owner) external view returns (uint256 balance);

    /**
     * @dev Returns the owner of the `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function ownerOf(uint256 tokenId) external view returns (address owner);

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Transfers `tokenId` token from `from` to `to`.
     *
     * WARNING: Note that the caller is responsible to confirm that the recipient is capable of receiving ERC721
     * or else they may be permanently lost. Usage of {safeTransferFrom} prevents loss, though the caller must
     * understand this adds an external call which potentially creates a reentrancy vulnerability.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Gives permission to `to` to transfer `tokenId` token to another account.
     * The approval is cleared when the token is transferred.
     *
     * Only a single account can be approved at a time, so approving the zero address clears previous approvals.
     *
     * Requirements:
     *
     * - The caller must own the token or be an approved operator.
     * - `tokenId` must exist.
     *
     * Emits an {Approval} event.
     */
    function approve(address to, uint256 tokenId) external;

    /**
     * @dev Approve or remove `operator` as an operator for the caller.
     * Operators can call {transferFrom} or {safeTransferFrom} for any token owned by the caller.
     *
     * Requirements:
     *
     * - The `operator` cannot be the caller.
     *
     * Emits an {ApprovalForAll} event.
     */
    function setApprovalForAll(address operator, bool _approved) external;

    /**
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId) external view returns (address operator);

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);
}

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function totalSupply() external view returns (uint256);

    function limitSupply() external view returns (uint256);

    function availableSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface IBUSDBANK {
    function viewUserInfo(address user) external view returns (uint256, uint256, uint256, uint256, uint256, uint256, bool);
}

interface INODES {
    function viewUserInfo(address user, uint256 tier) external view returns (uint256, uint256, uint256, uint256, uint256, uint256);
}

contract BUSDBANKWEALTH {
    
    struct User {
        uint256 stakedAmount;
        uint256 lastUpdateTime;
        uint256 rewards;
        uint256 referralRewards;
    }
    
    IERC20 private busdToken;
    IERC721 private bankKeyNFT;
    IERC721 private gemNFT;
    IBUSDBANK private busdBank;
    INODES private nodes;
    address private devPoolAddress;

    uint256 private constant MIN_INVEST = 10 ether; // min 10 BUSD
    uint256 private constant DAILY_RETURN = 50; // 5% daily return
    uint256 private constant DAILY_RETURN_BOOSTED = 70; // 7% daily return
    uint256 private constant STAKE_FEE = 100; // 10%
    uint256 private constant REFERRAL_FEE = 400; // 40% of 10%
    uint256 private constant PRECISION = 1000; // Precision for percentage calculations
    uint256 private constant TIME_STEP = 1 days; // time step for daily apr

    uint256 private constant BUSD_BANK_VAULT_MIN_STAKE = 2000 ether; // 2000 BUSD
    uint256 private constant NODES_MIN_STAKE = 1500 ether; // 1500 BUSD


    mapping(address => User) private users;
    address[] private userAddresses;

    // launch
    uint256 private launched;

    
    event Staked(address indexed user, uint256 amount);
    event PartialWithdrawn(address indexed user, uint256 amount);
    event ReferralReward(address indexed referrer, uint256 amount);
    
    constructor(address busdTokenAddress, address _devPoolAddress, address bankKeyNFTAddress, address gemNFTAddress, address busdBankAddress, address nodesAddress) {
        busdToken = IERC20(busdTokenAddress);
        devPoolAddress = _devPoolAddress;
        bankKeyNFT = IERC721(bankKeyNFTAddress);
        gemNFT = IERC721(gemNFTAddress);
        busdBank = IBUSDBANK(busdBankAddress);
        nodes = INODES(nodesAddress);
    }

    function launch() external {
        require(msg.sender == devPoolAddress, "Only the admin can launch contract");
        require(launched == 0, "contract has already been launched");
        launched = 1;
    }

    function ownsBankKeyNFT(address user) public view returns(bool) {
        uint256 balance = bankKeyNFT.balanceOf(user);

        if (balance > 0){
            return true;
        }
        return false;
    }

    function ownsGemNFT(address user) public view returns(bool) {
        uint256 balance = gemNFT.balanceOf(user);

        if (balance > 0){
            return true;
        }
        return false;
    }

    function hasBusdBankStake(address user) public view returns(bool) {
        uint256 balance;

        (balance, , , , , , ) = busdBank.viewUserInfo(user);

        
        if (balance >= BUSD_BANK_VAULT_MIN_STAKE){ // at least 2000 staked in busd bank vault
            return true;
        }
        return false;
    }

    function hasNotEmergencyWithdraw(address user) public view returns(bool) {
        bool emergencyWithdrawn;

        (, , , , , , emergencyWithdrawn) = busdBank.viewUserInfo(user);

        
        return !emergencyWithdrawn; // NOT emergency withdrawn from VAULT
    }

    function hasNodesStake(address user) public view returns(bool) {
        uint256 balance_tier0;
        uint256 balance_tier1;
        uint256 balance_tier2;
        uint256 balance;

        (balance_tier0, , , , ,) = nodes.viewUserInfo(user, 0);
        (balance_tier1, , , , ,) = nodes.viewUserInfo(user, 1);
        (balance_tier2, , , , ,) = nodes.viewUserInfo(user, 2);

        balance = balance_tier0 + balance_tier1 + balance_tier2;
        
        if (balance >= NODES_MIN_STAKE){ // at least 1500 staked in busd bank nodes
            return true;
        }
        return false;
    }

    function checklistComplete(address user) public view returns(bool) {
        // ALL must hold true in order to receive 7% daily ROI
        return ownsBankKeyNFT(user) && ownsGemNFT(user) && hasBusdBankStake(user) && hasNodesStake(user) && hasNotEmergencyWithdraw(user);
    } 
    
    function stake(uint256 amount, address referrer) external {
        require(launched > 0, "Contract has not launched yet");
        require(amount >= MIN_INVEST, "Amount must be greater than or equal to 10");
        require(busdToken.balanceOf(msg.sender) >= amount, "Insufficient BUSD balance");
        require(busdToken.allowance(msg.sender, address(this)) >= amount, "Insufficient allowance");
        
        busdToken.transferFrom(msg.sender, address(this), amount); // Transfer the staked BUSD to the contract

        uint256 fee = (amount * STAKE_FEE) / PRECISION; // Calculate the 10% fee
        uint256 stakedAmount = amount - fee; // Subtract the fee from the staked amount

        // Check if the user has been referred by someone
        if (referrer != address(0) && referrer != msg.sender) {
            uint256 referralFee = (fee * REFERRAL_FEE) / PRECISION; // Calculate the 4% referral fee
            
            // Transfer the referral fee to the referrer
            busdToken.transfer(referrer, referralFee);
            
            emit ReferralReward(referrer, referralFee);
            
            fee -= referralFee; // Deduct the referral fee from the total fee
            
            // Update the referral rewards for the referrer
            users[referrer].referralRewards += referralFee;
        }
        
        // Transfer the remaining fee to the devPoolAddress
        busdToken.transfer(devPoolAddress, fee);
        
        updateReward(msg.sender); // Update the rewards before changing the staked amount
        
        
        users[msg.sender].stakedAmount += stakedAmount;
        users[msg.sender].lastUpdateTime = block.timestamp;

        
        addUser(msg.sender); // add the user
        
        updateReward(msg.sender); // Update
        emit Staked(msg.sender, stakedAmount);
    }

    function compound() external {
        require(users[msg.sender].stakedAmount > 0, "No staked amount to compound");

        updateReward(msg.sender);

        uint256 reward = users[msg.sender].rewards;

        require(reward > 0, "No rewards to compound");

        uint256 compoundAmount = reward;

        users[msg.sender].rewards -= compoundAmount;
        users[msg.sender].stakedAmount += compoundAmount;

        emit Staked(msg.sender, compoundAmount);
    }


    function withdrawRewards() external {
        require(users[msg.sender].stakedAmount > 0, "No staked amount to withdraw rewards");
        
        uint256 reward = calculateReward(msg.sender);
        require(reward > 0, "No rewards to withdraw");
        require(reward < busdToken.balanceOf(address(this)));
        
        updateReward(msg.sender);
        
        users[msg.sender].rewards = 0;
        
        busdToken.transfer(msg.sender, reward);
    }

    function calculateReward(address user) public view returns (uint256) {
        uint256 stakedAmount = users[user].stakedAmount;
        uint256 lastUpdateTime = users[user].lastUpdateTime;
        uint256 rewards = users[user].rewards;
        
        uint256 reward = rewards;
        
        if (stakedAmount > 0) {
            uint256 timeDifference = block.timestamp - lastUpdateTime;
            
            if (checklistComplete(user)) {
                reward = rewards + ((stakedAmount * DAILY_RETURN_BOOSTED * timeDifference) / TIME_STEP / PRECISION);
            }
            else {
                reward = rewards + ((stakedAmount * DAILY_RETURN * timeDifference) / TIME_STEP / PRECISION);
            }
            
        }
        
        return reward;
    }

    function updateReward(address user) internal {
        uint256 reward = calculateReward(user);
        users[user].rewards = reward;
        users[user].lastUpdateTime = block.timestamp;
    }

    

    function addUser(address user) internal {
        for (uint256 i = 0; i < userAddresses.length; i++) {
            if (userAddresses[i] == user) {
                return; // do not re-add user
            }
        }
        
        userAddresses.push(user);
    }


    function viewUserInfo(address user) external view returns (uint256, uint256, uint256, uint256){
        return (users[user].stakedAmount, users[user].lastUpdateTime, users[user].rewards, users[user].referralRewards);
    }

    function viewUserEstYield(address user) external view returns (uint256) {
        uint256 estYield;

        if (checklistComplete(user)) {
            estYield = (users[user].stakedAmount * DAILY_RETURN_BOOSTED) / PRECISION;
        }
        else{
            estYield = (users[user].stakedAmount * DAILY_RETURN) / PRECISION;
        }
        return estYield;
    }

}