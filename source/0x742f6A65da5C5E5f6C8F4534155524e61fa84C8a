/**
 *Submitted for verification at BscScan.com on 2023-08-10
*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

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



contract X_Stake {

	IERC20 private immutable usdt;
	IERC721 private immutable nftPass;
	address private immutable devAddress;
	

    modifier onlyOwner() {
    require(msg.sender == devAddress, "Only the owner can call this function");
    _;
}
	uint private constant PRECISION = 1000;
	uint private constant REF = 50; //5% ref level 1
	uint private constant FEE = 20; //2% tax
	uint private constant DAILY = 10; //1% Daily
	uint private constant NFTBONUS = 10; //10b
	uint private constant MINIMUM = 1 * 10**9; //
    uint private constant COOLDOWN = 1 days;
	
	mapping(address => uint) private deposits;
	mapping(address => uint) private claimCheckpoint;
	mapping(address => uint) private checkpoint;
	mapping(address => uint) private claimable;
	
	mapping(address => address) private referrals;
	
	uint private constant launch = 1691333146;
	
	uint private totalDeposits = 0;
	uint private totalUsers = 0;
	
	event Deposit(address indexed user, uint amount);
	
	constructor(address nftPassAddress, address usdtTokenAddress) {
		usdt = IERC20(usdtTokenAddress);
		nftPass = IERC721(nftPassAddress);
		devAddress = msg.sender;
	}
	
	modifier contractOpen() {
		require(block.timestamp > launch, "contract not yet open !");
		_;
	}
	
    function RescueBNB() external onlyOwner { //return BNB accidentally send to contract
    uint contractBalance = usdt.balanceOf(address(this));
    require(contractBalance > 0, "No balance to withdraw");

    usdt.transfer(devAddress, contractBalance);
}

	
	function deposit(uint amount, address ref) public contractOpen {
		require(amount >= MINIMUM, "minimum deposit is 10 X !");
		
		if(!isActive()) {
			if(isActive(ref)) {
				referrals[msg.sender] = ref;
			} else {
				referrals[msg.sender] = devAddress;
			}
			totalUsers = totalUsers + 1;
		}
		
		uint fee = amount * FEE / PRECISION;
		amount = amount - fee;
		
		usdt.transferFrom(msg.sender, devAddress, fee);
		usdt.transferFrom(msg.sender, address(this), amount);
		
		uint bonus = amount * REF / PRECISION;
		claimable[referrals[msg.sender]] = claimable[referrals[msg.sender]] + bonus;
		
		claimable[msg.sender] = getUserClaimable();
		checkpoint[msg.sender] = block.timestamp;
		
		deposits[msg.sender] = deposits[msg.sender] + amount;
		totalDeposits = totalDeposits + amount;
		emit Deposit(msg.sender, amount);
	}
	
	function compound() public contractOpen {
		require(isActive(), "user is not active !");
		
		uint toCompound = getUserClaimable();
		
		claimable[msg.sender] = 0;
		checkpoint[msg.sender] = block.timestamp;
		deposits[msg.sender] = deposits[msg.sender] + toCompound;
	}
	
	function claim() public contractOpen {
		require(isActive(), "user is not active !");
		require(block.timestamp >= claimCheckpoint[msg.sender] + COOLDOWN, "24 hour cooldown not yet achieved !");
		
		uint toClaim = getUserClaimable();
		
		claimable[msg.sender] = 0;
		checkpoint[msg.sender] = block.timestamp;
		claimCheckpoint[msg.sender] = block.timestamp;
		
		if(usdt.balanceOf(address(this)) < toClaim) {
			usdt.transfer(msg.sender, usdt.balanceOf(address(this)));
		} else {
			usdt.transfer(msg.sender, toClaim);
		}
	}
	
	
	
	function getUserClaimable() public view returns(uint available) {
		if(isActive()) {
			uint passed = block.timestamp - checkpoint[msg.sender];
			uint daily = hasPass() ? DAILY + NFTBONUS : DAILY;
			
			available = deposits[msg.sender] * daily * passed / 1 days / PRECISION;
			available = claimable[msg.sender] + available;
		}
	}
	
	function getUserInfo() public view returns(uint totalDeposit, uint lastClaim) {
		totalDeposit = deposits[msg.sender];
		lastClaim = claimCheckpoint[msg.sender];
	}
	
	function getContractInfo() public view returns(uint contractBalance, uint totalUser, uint totalDeposit) {
		contractBalance = usdt.balanceOf(address(this));
		totalUser = totalUsers;
		totalDeposit = totalDeposits;
	}
	
	

	function hasPass() private view returns(bool) {
        return nftPass.balanceOf(msg.sender) > 0 ? true : false;
    }
	
	function isActive() private view returns(bool) {
		return isActive(msg.sender);
	}
	
	function isActive(address addr) private view returns(bool) {
		return deposits[addr] > 0 ? true : false;
	}
	
}