// SPDX-License-Identifier: MIT
pragma experimental ABIEncoderV2;
pragma solidity 0.6.12;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function mint(address account, uint amount) external;
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}


library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly {codehash := extcodehash(account)}
        return (codehash != accountHash && codehash != 0x0);
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success,) = recipient.call{value : amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }


    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }


    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{value : weiValue}(data);
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}


/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for IERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {// Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}


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

    function name() external view returns (string memory);

    /**
     * @dev Returns the token collection symbol.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the Uniform Resource Identifier (URI) for `tokenId` token.
     */
    function tokenURI(uint256 tokenId) external view returns (string memory);

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
	function creator(uint256 tokenId) external view returns (address owner);
	
	function isOfficialNFT(uint256 tokenId) external view returns (bool status);
	
	
    function ownerOf(uint256 tokenId) external view returns (address owner);

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be have been allowed to move this token by either {approve} or {setApprovalForAll}.
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
     * WARNING: Usage of this method is discouraged, use {safeTransferFrom} whenever possible.
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
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId) external view returns (address operator);

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
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);

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
}


interface IERC721Receiver {
    /**
     * @dev Whenever an {IERC721} `tokenId` token is transferred to this contract via {IERC721-safeTransferFrom}
     * by `operator` from `from`, this function is called.
     *
     * It must return its Solidity selector to confirm the token transfer.
     * If any other value is returned or the interface is not implemented by the recipient, the transfer will be reverted.
     *
     * The selector can be obtained in Solidity with `IERC721.onERC721Received.selector`.
     */
    function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external returns (bytes4);
}

contract ERC721Holder is IERC721Receiver {
    /**
     * @dev See {IERC721Receiver-onERC721Received}.
     *
     * Always returns `IERC721Receiver.onERC721Received.selector`.
     */
    function onERC721Received(
        address,
        address,
        uint256,
        bytes memory
    ) public virtual override returns (bytes4) {
        return this.onERC721Received.selector;
    }
}

contract Pool is ERC721Holder{
    using SafeMath for uint256;
    using SafeERC20 for IERC20;


    // Info of each user.
    struct UserInfo {
        uint256 rewardDebt; 
        uint256 allstake;
		uint256 nftAmount;
        uint256 nftAddition;

    }
    // Info of each pool.
    struct PoolInfo {
	    uint256 startBlock;
		uint256 endBlock;
		uint256 stakeSupply;
        uint256 nftAddition;
		uint256 lastRewardBlock;
        uint256 accPerShare; 
		uint256 nftWeights;
		uint256 maxNftAmount;
        uint256 nftSupply;
        
    }
    

    

    uint256 public perBlock;

    PoolInfo public poolInfo;
    mapping(address => UserInfo) public userInfo;
	mapping (address => uint[]) public userNft;

	IERC20 public pledgeAddress;
    IERC20 public profitToken;
	IERC721 public nftContract;
    uint256 public withdrawalFee;
    address public wallet;

    uint256 public withdrawAmount;

    uint256 public rePledgeRate;
    uint256 public withdrawRate;
    address public rateAddress;

    mapping(address => bool) public manager;
	bool public paused = false;
	uint256 public startTime ;
	mapping(address => bool ) public whitelist;


    event Pledge(address user,  uint256 amount);
	event RePledge(address user,  uint256 amount);
    event Withdraw(address user, uint256 amount);
    event StakeNft(address user, uint256 tokenId);
    event UnStakeNft(address user, uint256 tokenId);
    event SetPause( bool paused);
    event RePledgeFee(address user, uint256 amount);
    event withdrawFee(address user, uint256 amount);

    constructor() public {
        startTime = 1686024000;
		perBlock = 142694063927000000;
        manager[msg.sender] = true; 
        wallet = 0x978489611B7508E8515ee039d3F62612E128F50f; 
        rateAddress = 0x978489611B7508E8515ee039d3F62612E128F50f;
        rePledgeRate = 3;
        withdrawRate = 3;
        withdrawalFee = 8000000000000000;
		setStartAndEnd(block.number,block.number + 31536000); 
        poolInfo.lastRewardBlock = block.number;
        nftContract = IERC721(0x8EE0C2709a34E9FDa43f2bD5179FA4c112bEd89A);     
        setNftWeights(3,5);
		setTokenAddress(0xa4dBc813F7E1bf5827859e278594B1E0Ec1F710F,0xa4dBc813F7E1bf5827859e278594B1E0Ec1F710F);
		
    }

	function setNftWeights(uint256 _nftWeights, uint256 _maxNftAmount) public onlyOwner {
       
		poolInfo.nftWeights = _nftWeights;
		poolInfo.maxNftAmount = _maxNftAmount;
    }

    function getMultiplier(uint256 _from, uint256 _to) public pure returns(uint256){
            return _to.sub(_from);
    }

    function pendingFit(address _user) public view returns(uint256){
        uint256 curBlock = poolInfo.endBlock < block.number ? poolInfo.endBlock : block.number;
        UserInfo storage user = userInfo[_user];
        uint256 accPerShare = poolInfo.accPerShare;

        if (curBlock > poolInfo.lastRewardBlock && poolInfo.stakeSupply != 0) {
            uint256 multiplier =
                getMultiplier(poolInfo.lastRewardBlock, curBlock);
            uint256 fitReward =
                multiplier.mul(perBlock);
            accPerShare = accPerShare.add(
                fitReward.mul(1e12).div(poolInfo.stakeSupply.add(poolInfo.nftAddition))
            );
        }

        uint256 userreward = (user.allstake.add(user.nftAddition)).mul(accPerShare).div(1e12).sub(user.rewardDebt);
        return userreward;
    }




    function updatePool() public {

        uint256 curBlock = poolInfo.endBlock < block.number ? poolInfo.endBlock : block.number;
        
        if (poolInfo.stakeSupply == 0) {
            poolInfo.lastRewardBlock = curBlock;
            return;
        }
        if(userInfo[msg.sender].nftAddition != 0){
            poolInfo.nftAddition = poolInfo.nftAddition.sub(userInfo[msg.sender].nftAddition);
        }
        userInfo[msg.sender].nftAddition = userInfo[msg.sender].allstake.mul(userInfo[msg.sender].nftAmount).mul(poolInfo.nftWeights).div(100);
        poolInfo.nftAddition = poolInfo.nftAddition.add(userInfo[msg.sender].nftAddition);

        uint256 multiplier = getMultiplier(poolInfo.lastRewardBlock, curBlock);
        uint256 fitReward = multiplier.mul(perBlock);
        poolInfo.accPerShare = poolInfo.accPerShare.add(
            fitReward.mul(1e12).div(poolInfo.stakeSupply.add(poolInfo.nftAddition))
        );
        poolInfo.lastRewardBlock = curBlock;
        
    }

    
    function pledge(uint256 _stakeAmount) public  notPause payable{
		if(block.timestamp < startTime){
			require(whitelist[msg.sender],"Not yet started");
		}
		require(msg.value >= withdrawalFee,"Please pay the withdrawal fee");
        uint256 pending = pendingFit(msg.sender);
        if(pending > 0){            
            payable(wallet).transfer(msg.value);        
            safeGoodTransfer(msg.sender,pending);
            emit Withdraw(msg.sender, pending);
        }else{
			payable(wallet).transfer(msg.value); 
		}
		 
        if( userInfo[msg.sender].nftAmount == 0){
			uint256 fee = _stakeAmount.mul(rePledgeRate).div(100);
			pledgeAddress.transferFrom(msg.sender,rateAddress, fee);
			_stakeAmount = _stakeAmount.sub(fee);
		}
		
        pledgeAddress.transferFrom(msg.sender,address(this),_stakeAmount);
        userInfo[msg.sender].allstake= userInfo[msg.sender].allstake.add(_stakeAmount);
		poolInfo.stakeSupply = poolInfo.stakeSupply.add(_stakeAmount);
        
        updatePool();
        userInfo[msg.sender].rewardDebt = (userInfo[msg.sender].allstake.add(userInfo[msg.sender].nftAddition)).mul(poolInfo.accPerShare).div(1e12);

        
        emit Pledge(msg.sender, _stakeAmount);
    }

    function rePledge(uint256 _stakeAmount) public notPause payable{

        require(_stakeAmount > 0, "Must be greater than 0");

        require(userInfo[msg.sender].allstake >= _stakeAmount, "Insufficient amount of pledge");           
		require(msg.value >= withdrawalFee,"Please pay the withdrawal fee");
		
		uint256 pending = pendingFit(msg.sender);
        if(pending > 0){            
            payable(wallet).transfer(msg.value);        
            safeGoodTransfer(msg.sender,pending);
            emit Withdraw(msg.sender, pending);
        }else{
			payable(wallet).transfer(msg.value); 
		}
		
        userInfo[msg.sender].allstake = userInfo[msg.sender].allstake.sub(_stakeAmount);		
		poolInfo.stakeSupply = poolInfo.stakeSupply.sub(_stakeAmount);
        
        if( userInfo[msg.sender].nftAmount == 0){
           uint256 fee = _stakeAmount.mul(rePledgeRate).div(100);
           pledgeAddress.safeTransfer(rateAddress, fee);
           emit RePledgeFee(rateAddress, fee);
           _stakeAmount = _stakeAmount.sub(fee);
        }

        pledgeAddress.safeTransfer(address(msg.sender), _stakeAmount);
        updatePool();
        userInfo[msg.sender].rewardDebt = (userInfo[msg.sender].allstake.add(userInfo[msg.sender].nftAddition)).mul(poolInfo.accPerShare).div(1e12);

        

        emit RePledge(msg.sender, _stakeAmount);
        
    }

    
    function withdraw() public  payable notPause {
        require(msg.value >= withdrawalFee,"Please pay the withdrawal fee");        
        updatePool();
        uint256 pending = pendingFit(msg.sender);
        userInfo[msg.sender].rewardDebt = (userInfo[msg.sender].allstake.add(userInfo[msg.sender].nftAddition)).mul(poolInfo.accPerShare).div(1e12);
        if( userInfo[msg.sender].nftAmount == 0){
           uint256 fee = pending.mul(withdrawRate).div(100);
           pledgeAddress.safeTransfer(rateAddress, fee);
           emit withdrawFee(rateAddress, fee);
           pending = pending.sub(fee);
           payable(wallet).transfer(msg.value);
        }else{
		   payable(wallet).transfer(msg.value); 
		}

        safeGoodTransfer(msg.sender,pending);  
        withdrawAmount = withdrawAmount.add(pending);     
		emit Withdraw(msg.sender, pending);

    }
    
    function emergencyWithdraw() public {

        pledgeAddress.safeTransfer(address(msg.sender), userInfo[msg.sender].allstake);
        userInfo[msg.sender].allstake = 0;
        userInfo[msg.sender].rewardDebt = 0;
    }
    
	
	 function stakeNft(uint[] memory tokenIds) public notPause payable{
		require(msg.value >= withdrawalFee,"Please pay the withdrawal fee");
        uint256 pending = pendingFit(msg.sender);
        if(pending > 0){           
            payable(wallet).transfer(msg.value);        
            safeGoodTransfer(msg.sender,pending);
            emit Withdraw(msg.sender, pending);
        }else{
			payable(wallet).transfer(msg.value); 
		}

        require(tokenIds.length > 0, "At least one token ID must be provided");
        for (uint i = 0; i < tokenIds.length; i++) {
			require(nftContract.isOfficialNFT(tokenIds[i]),"Not OfficialNFT");
            require(nftContract.ownerOf(tokenIds[i]) == msg.sender, "You don't own this NFT");
            userInfo[msg.sender].nftAmount += 1; 
            nftContract.safeTransferFrom(msg.sender, address(this), tokenIds[i]);
			
			(bool isIn,uint256 index) = firstIndexOf(userNft[msg.sender],0);
			if(!isIn){
				userNft[msg.sender].push(tokenIds[i]);        
			}else{
				userNft[msg.sender][index] = tokenIds[i];
			}
            
            poolInfo.nftSupply = poolInfo.nftSupply.add(1);
            emit StakeNft(msg.sender,tokenIds[i]);
        }
        require(userInfo[msg.sender].nftAmount <= poolInfo.maxNftAmount,"Too many NFTs");
        updatePool();
        userInfo[msg.sender].rewardDebt = (userInfo[msg.sender].allstake.add(userInfo[msg.sender].nftAddition)).mul(poolInfo.accPerShare).div(1e12);   
        

    }
    
    function unstakeNft(uint[] memory tokenIds) public payable notPause{
		require(msg.value >= withdrawalFee,"Please pay the withdrawal fee");
        uint256 pending = pendingFit(msg.sender);
        if(pending > 0){           
            payable(wallet).transfer(msg.value);        
            safeGoodTransfer(msg.sender,pending);
            emit Withdraw(msg.sender, pending);
        }else{
			payable(wallet).transfer(msg.value); 
		}
		

        require(tokenIds.length > 0, "At least one token ID must be provided");
        for (uint i = 0; i < tokenIds.length; i++) {
            require(nftContract.ownerOf(tokenIds[i]) == address(this), "There is no such NFT on the contract");
			
			(bool isIn, uint256 index) = firstIndexOf(userNft[msg.sender],tokenIds[i]);
				require(isIn,"This is not your NFT");
				removeByIndex(msg.sender, index);
				userInfo[msg.sender].nftAmount = userInfo[msg.sender].nftAmount.sub(1);
				nftContract.safeTransferFrom(address(this), msg.sender, tokenIds[i]);   

                poolInfo.nftSupply = poolInfo.nftSupply.sub(1);
                emit UnStakeNft(msg.sender,tokenIds[i]);         
        }
       
        updatePool();
        userInfo[msg.sender].rewardDebt = (userInfo[msg.sender].allstake.add(userInfo[msg.sender].nftAddition)).mul(poolInfo.accPerShare).div(1e12);   

       
    }
	
	function firstIndexOf(uint256[] memory array, uint256 key) internal pure returns (bool, uint256) {

    	if(array.length == 0){
    		return (false, 0);
    	}

    	for(uint256 i = 0; i < array.length; i++){
    		if(array[i] == key){
    			return (true, i);
    		}
    	}
    	return (false, 0);
    }
	
	function removeByIndex(address _user, uint256 index) internal{
    	require(index < userNft[_user].length, "ArrayForUint256: index out of bounds");
        uint256 length = userInfo[_user].nftAmount;
        userNft[_user][index] = userNft [_user][length - 1];
        delete userNft[_user][length - 1] ;
		
    }

	function getUserNft(address user) public view returns (uint[] memory) {
        return userNft[user];
    }
    	
	function getUserNftAndUri(address user) public view returns (uint[] memory,string[] memory) {
        uint256 amount = userInfo[msg.sender].nftAmount;
		uint256[] memory nftIds = new uint256[](amount);
		string[] memory nftUri = new string[](amount);
		for(uint256 i = 0; i< amount; i++){          
			nftIds[i] = userNft [user][i];      
			nftUri[i] = nftContract.tokenURI(nftIds[i]);  
		}
		return (nftIds,nftUri);
    }
    
    function safeGoodTransfer(address _to, uint256 _amount) internal {
       
        uint256 bal = profitToken.balanceOf(address(this));
        if (_amount > bal) {
            IERC20(profitToken).transfer( _to, bal);
        } else {
            IERC20(profitToken).transfer( _to, _amount);
        }
    }


    function setPerBlock(uint256 _perBlock) public onlyOwner  {
        updatePool();
        perBlock = _perBlock;
    }
    
    function setPause() public onlyOwner {
        paused = !paused;
        emit SetPause(paused);
    }

	function setStartTime(uint256 _startTime) public onlyOwner {
        startTime = _startTime;
    }
	
	function setWhitelist(address _user, bool _status) public onlyOwner {
		whitelist[_user] = _status;
	}

    
    function setStartAndEnd(uint256 _start , uint256 _end) public onlyOwner{
        poolInfo.startBlock = _start;
        poolInfo.endBlock = _end;
    }


    function setEnd(uint256 _end) public onlyOwner{
        poolInfo.endBlock = _end;
    }

    function setNftContract(address _nftContract) public onlyOwner  {
        nftContract = IERC721(_nftContract);
    }

	
	function setTokenAddress(address _pledgeAddress, address _profitToken)public onlyOwner{
		pledgeAddress = IERC20(_pledgeAddress);
		profitToken = IERC20(_profitToken);
	}
    
    function setWalletAndFee(uint256 _withdrawalFee, address _wallet)public onlyOwner{
        withdrawalFee =_withdrawalFee;
        wallet = _wallet;
    }
    
    function setRateAndAddress(uint256 _rePledgeRate, uint256 _withdrawRate, address _rateAddress)public onlyOwner{
        rePledgeRate = _rePledgeRate;
        withdrawRate = _withdrawRate;
        rateAddress = _rateAddress;
    }

    function withdrawStuckEth(address payable recipient) public onlyOwner {
		recipient.transfer(address(this).balance);
	}
	
	function withdrawStuckTokens(address _token, uint256 _amount) public onlyOwner {
		IERC20(_token).transfer(msg.sender, _amount);
	}
	
	function withdrawStuckNFT(address _token, uint256 _tokenId) public onlyOwner {
		IERC721(_token).transferFrom(address(this), msg.sender, _tokenId);
	}

    function setOwner(address newOwner,bool isManager) public  onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        manager[newOwner] = isManager;
    }

  
    modifier notPause() {
        require(paused == false, "Mining has been suspended");
        _;
    }

    modifier onlyOwner() {
        require(manager[msg.sender], "Ownable: caller is not the owner");
        _;
    }
}