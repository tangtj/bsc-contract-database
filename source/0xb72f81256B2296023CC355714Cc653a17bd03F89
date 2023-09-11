/**
 *Submitted for verification at BscScan.com on 2023-09-10
*/

// SPDX-License-Identifier: MIT
pragma solidity =0.8.8;

// import "@openzeppelin/contracts/token/ERC721/IERC721Receiver.sol";

interface IERC721 {
    function ownerOf(uint256 tokenId) external view returns (address owner);
    function safeTransferFrom(address from, address to, uint256 tokenId) external;
}

// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC721/IERC721Receiver.sol)

/**
 * @title ERC721 token receiver interface
 * @dev Interface for any contract that wants to support safeTransfers
 * from ERC721 asset contracts.
 */
interface IERC721Receiver {
    /**
     * @dev Whenever an {IERC721} `tokenId` token is transferred to this contract via {IERC721-safeTransferFrom}
     * by `operator` from `from`, this function is called.
     *
     * It must return its Solidity selector to confirm the token transfer.
     * If any other value is returned or the interface is not implemented by the recipient, the transfer will be reverted.
     *
     * The selector can be obtained in Solidity with `IERC721Receiver.onERC721Received.selector`.
     */
    function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external returns (bytes4);
}

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    
    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address _owner, address spender) external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Returns the token decimals.
     */
    function decimals() external view returns (uint8);

    function approve(address spender, uint256 amount) external returns (bool);

}

contract Context {
    // Empty internal constructor, to prevent people from mistakenly deploying
    // an instance of this contract, which should be used via inheritance.
    constructor() {}

    function _msgSender() internal view returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), 'Ownable: caller is not the owner');
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     */
    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, 'SafeMath: addition overflow');

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, 'SafeMath: subtraction overflow');
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, 'SafeMath: multiplication overflow');

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, 'SafeMath: division by zero');
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, 'SafeMath: modulo by zero');
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }

    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }

    // babylonian method (https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method)
    function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

contract BBXStakingPool is Ownable, IERC721Receiver {

    using SafeMath for uint256;

    IERC20 public bbxToken;  // BBX token contract
    IERC721 public nftContract;  // NFT contract

    uint256 public totalStaked;
    uint256 public rewardRate;
    uint256 public rewardDuration;
    uint256 public lastUpdateTime;
    uint256 public tokensLeft;

    struct User {
        bool staked;
        uint256 stakedNFTId;
        uint256 rewards;
        uint256 totalEarned;
        uint lastWithdrawTime;
    }

    mapping(address => User) public users;

    constructor(address _bbxToken, address _nftContract) {
        bbxToken = IERC20(_bbxToken);
        nftContract = IERC721(_nftContract);
        rewardDuration = 7 * 1 days;
        tokensLeft = 0;
    }


    function stake(uint256 nftId) external {
        require(nftContract.ownerOf(nftId) == msg.sender, "You do not own this NFT");
        require(!users[msg.sender].staked, "You have already staked an NFT");
        
        // Transfer NFT ownership to this contract
        nftContract.safeTransferFrom(msg.sender, address(this), nftId);
        
        // Update user's staked NFTs
        users[msg.sender].stakedNFTId = nftId;
        users[msg.sender].staked = true;
        users[msg.sender].lastWithdrawTime = block.timestamp;
        totalStaked = totalStaked.add(1);
        setRewardAmount();
    }


    function withdraw() external {
        require(users[msg.sender].staked, "You haven't staked any NFTs");
        
        // Withdraw user pending rewards
        _getReward(msg.sender);
        
        // Update user's staked NFTs
        totalStaked = totalStaked.sub(1);
        users[msg.sender].staked = false;
        setRewardAmount();

        // Transfer NFT back to the user
        nftContract.safeTransferFrom(address(this), msg.sender, users[msg.sender].stakedNFTId);
        
    }


    function earned(address account) public view returns (uint256) {
        uint256 rewardRatePerToken = rewardPerToken();
        uint _curTime = block.timestamp;
        uint lastWithdrawTime = users[account].lastWithdrawTime;
        
        if(_curTime == lastWithdrawTime) {
            return 0;
        }

        uint _earned = _curTime.sub(lastWithdrawTime).mul(rewardRatePerToken);
        return _earned;
    }
    

    function getReward() external returns(bool) {
        require(users[msg.sender].staked, "You haven't staked any NFTs");
        return _getReward(msg.sender);
    }


    function _getReward(address account) internal returns(bool) {
        uint256 reward = earned(account);

        if (reward > 0) {
            users[msg.sender].totalEarned += reward;
            users[msg.sender].rewards = users[msg.sender].rewards.add(reward);
            users[msg.sender].lastWithdrawTime = block.timestamp;
            tokensLeft = tokensLeft.sub(reward);

            bbxToken.transfer(msg.sender, reward);   
        }
        return true;
    }
    

    function setRewardAmount() public {
        require(rewardDuration != 0, "Reward duration not set");
        lastUpdateTime = block.timestamp;
        uint256 reward = bbxToken.balanceOf(address(this));
        rewardRate = reward.div(rewardDuration);
        tokensLeft = reward;   
    }


    function rewardPerToken() public view returns (uint256) {
        if (totalStaked == 0) {
            return rewardRate;
        }
        return rewardRate / totalStaked;
    }


    function getDailyRewards() public view returns (uint256) {
        if (totalStaked == 0 || rewardDuration == 0) {
            return 0;
        }
        uint rewardPer = rewardPerToken();
        uint256 tokensPerDay = rewardPer.mul(rewardDuration / 1 days);
        return tokensPerDay;
    }
    
    
    function setRewardTime(uint256 newRewardTime) external onlyOwner {
        require(newRewardTime > 0, "Cannot set reward time to 0 days.");
        rewardDuration = newRewardTime * 1 days;
        // rewardRate = 
    }

    // Set a new Token address for the IERC20 reward token 
    function setRewardTokenAddress(address newRewardTokenAddress) external onlyOwner {
        require(newRewardTokenAddress != address(0), "Invalid address");
        bbxToken = IERC20(newRewardTokenAddress);
    }

    // Withdraw all reward tokens from the pool
    function withdrawAllRewards() external onlyOwner {
        uint256 balance = bbxToken.balanceOf(address(this));
        require(balance > 0, "No rewards to withdraw");
        setRewardAmount();
        tokensLeft = 0;
        bbxToken.transfer(owner(), balance);
    }

     function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external override returns (bytes4) {
        return IERC721Receiver.onERC721Received.selector;
    }
}