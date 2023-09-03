//SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.19;

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 * now has built in overflow checking.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

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
        return a + b;
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
        return a - b;
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
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
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
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
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
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
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
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
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
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}


/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain `call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Tool to verifies that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason using the provided one.
     *
     * _Available since v4.3._
     */
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

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
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

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
    function transfer(address recipient, uint256 amount) external returns (bool);

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
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}



/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}




contract TRRRR is Ownable {
    using Address for address;
    using SafeMath for uint256;
    
    // Player informations
    struct Player {
        uint32 score;
        string playerName;
        uint256 BNBWinning;
        uint256 BOOMMWinning;
        uint32 totalGame;
        uint32 numWin;
        uint32 numLoss;

        bool _isBlacklisted;
        bool _isPlaying;
        uint32 _gameId;
        address _referrer;
 
    }

    // Game Informations
    struct Game {
        Player player;
        uint32 id;
        bool finished;
        bool winner;
        bool useBOOMM;
        uint32 score;
        uint256 startBlock;
    }

    // Player Mapping / Ranking
    mapping (address => Player) public players;
    mapping (address => bool) public registeredPlayer;
    uint32 lastGameId = 1;
    Player[] public rankings;
    uint32 public highestScore = 0;

    uint256 public BOOMMEntryFee = 0;
    uint256 public BNBEntryFee = 10;

    IERC20 BNBToken = IERC20(address(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c));
    IERC20 BOOMMToken = IERC20(address(0x2c6A4566003A9fDD1aD2eF0BAFfdc82665f23911));

    uint8 public winnerFeePercentage = 40; // 40% of the pool rewards

    address public utilFeeReceiver = address(0x2174F59C609a3a07649Bf6b294741DF07644bF9b);
    uint8 public utilFeePercentage = 10; // 10% of the pool rewards  
    uint8 public referrerFeePercentage = 10; // 10% of the pool rewards  

    mapping (address => bool) public _team;



    // Game Details
    mapping (uint32 => Game) public _games; // Game ID -> Game
    bool _KABOOMMEnabled = true; // If we want to pause the game
    uint256 public _blockWaitTime = 2; // How many blocks we want to wait before finalizing a game
    mapping (address => mapping (address => uint32)) public _winnings;
 

    event GameStarted (uint32 gameId, string playerName, address indexed player, uint32 lastScore);
    event GameFinished(uint32 gameId, string playerName, address indexed player, uint32 lastScore, bool winner );
    event PayoutComplete(uint32 gameId, address indexed winner, string playerName, bool isboomm, uint256 winnings);


    modifier onlyTeam {
        _onlyTeam();
        _;
    }

    function _onlyTeam() private view {
        require(_team[_msgSender()], "Only a team member may perform this action");
    }

    constructor() 
    {
        _team[owner()] = true;
    }

    // To recieve BNB from anyone, including the router when swapping
    receive() external payable {}



    function enterKABOOMM(bool withBOOMM, address referrer) external payable {
        
        address _player = _msgSender();

        // Run basic safe Check about user
        require(_KABOOMMEnabled, "KABOOMM is currently paused");
        require(!players[_player]._isBlacklisted, "This player is blacklisted");
        require(!(_player.isContract()), "Contracts are not allowed to play KABOOMM");
        require(msg.sender == tx.origin, "Sender must be the same as the address that started the tx");


        // Get the fee amount and make sure is right ;)
        if (withBOOMM == true){
            BOOMMToken.transferFrom(_player, address(this), BOOMMEntryFee);
            require(msg.value == BOOMMEntryFee, "Must send same amount as entryFee");
        }
        else{
            BNBToken.transferFrom(_player, address(this), BNBEntryFee);
            require(msg.value == BNBEntryFee, "Must send same amount as entryFee");
        }
        
        // Check if player already registred
        if (!registeredPlayer[_player]){

            // Setup new user
            players[_player].score = 0;
            players[_player]._isBlacklisted = false;
            players[_player].totalGame = 0;
            players[_player].numWin = 0;
            players[_player].numLoss = 0;

            //Add referer
            if (referrer != address(0x0) && referrer != _player) {
                players[_player]._referrer = referrer;
            }
            registeredPlayer[_player] = true;
            rankings.push(players[_player]);

        }

        // Player in Game
        players[_player].totalGame += 1;
        players[_player]._isPlaying = true;
        
        lastGameId = lastGameId + 1;

    
        _games[lastGameId] = Game({
            player: players[_player],
            id: lastGameId,
            finished: false,
            winner: false,
            useBOOMM: withBOOMM,
            score:0,
            startBlock: block.number
        });
        
        // Assign game id to user
        players[_player]._gameId = lastGameId;

        // Log tht new game start
        emit GameStarted(lastGameId, players[_player].playerName, _player,  players[_player].score );
        
        
        // Make sure the ranking up to date
        updateRankings();

    }



    function sendScoreAndComplete(address _player, uint32 _newScore)  external onlyTeam {
        
        require(_KABOOMMEnabled, "KABOOMM is currently paused");
        require(!players[_player]._isBlacklisted, "This player is blacklisted");
        require(!(_player.isContract()), "Contracts are not allowed to play KABOOMM");
        require(registeredPlayer[_player], "this player didn't register");
        require(players[_player]._isPlaying, "this player not currently playing");

        //Get user gameID
        uint32 currentGameId = players[_player]._gameId;
        players[_player]._isPlaying = false;
        players[_player]._gameId = 0;
        
        // Make sure user is currently playing 
        if (currentGameId != 0 ){
            
            Game storage currentGame = _games[currentGameId];
            require(block.number >= currentGame.startBlock.add(_blockWaitTime), "game was too fast");
            
            if (currentGame.finished == false){
                _games[currentGameId].finished = true;
                bool isboomm = _games[currentGameId].useBOOMM;
                string memory playerName = players[_player].playerName;

                //Make sure ranking is good 
                updateRankings();

                if (_newScore > highestScore){
                    players[_player].numWin += 1;
                    _games[currentGameId].winner = true;
                    address referrer = players[_player]._referrer;
                    
                    //Lets send some $$$$$$$$
                    if (isboomm == true){
                        
                        //Get current BOOMM Pool 
                        uint256 BOOMMContractBalance =  BOOMMToken.balanceOf(address(this));
                        
                        uint256 feeToWinner = BOOMMContractBalance.mul(winnerFeePercentage).div(100);
                        uint256 feeToUtil = BOOMMContractBalance.mul(utilFeePercentage).div(100);
                        if (referrer != address(0x0)) {
                            uint256 feeToReferrer = BOOMMContractBalance.mul(referrerFeePercentage).div(100);
                            BOOMMToken.transfer(referrer, feeToReferrer);
                        }

                        BOOMMToken.transfer(_player, feeToWinner);
                        BOOMMToken.transfer(utilFeeReceiver, feeToUtil);
                        emit PayoutComplete(currentGameId ,_player, playerName, isboomm , feeToWinner);

                    }
                    else{
                        //Get current BNB Pool 
                        uint256 BNBContractBalance =  BNBToken.balanceOf(address(this));

                        uint256 feeToWinner = BNBContractBalance.mul(winnerFeePercentage).div(100);
                        uint256 feeToUtil = BNBContractBalance.mul(utilFeePercentage).div(100);
                        
                        if (referrer != address(0x0)) {
                            uint256 feeToReferrer = BNBContractBalance.mul(referrerFeePercentage).div(100);
                            BNBToken.transfer(referrer, feeToReferrer);
                        }

                        BNBToken.transfer(_player, feeToWinner);
                        BNBToken.transfer(utilFeeReceiver, feeToUtil);
                        emit PayoutComplete(currentGameId ,_player, playerName, isboomm , feeToWinner);
                    }

                }
                else{
                    players[_player].numLoss += 1;
                    _games[currentGameId].winner = false;
                }

                if (_newScore > players[_player].score){
                    players[_player].score = _newScore;
                }
                
            
                //update last ranking
                updateRankings();

                 emit GameFinished(currentGameId, playerName, _player, _newScore, _games[currentGameId].winner);

            }

        }

    }

    

    function updateRankings() private {

        uint32[] memory scores = new uint32[](rankings.length);
        Player[] memory tempRankings = rankings;
  
        // Get all the scores in list
        for (uint32 i = 0; i < tempRankings.length; i++) {
            scores[i] = tempRankings[i].score;
        }


        // sort the score list
        for (uint32 i = 1; i < tempRankings.length ; i++) {
            uint32 key = scores[i];
            uint32 j = uint32(i) - 1;
            while ((uint32(j) >= 0) && (scores[uint32(j)] > key)) {
                scores[uint32(j + 1)] = scores[uint32(j)];
                j--;
            }
            scores[uint(j + 1)] = key;
        }


        //Remap the ranking array
        for (uint32 i = 0; i < scores.length; i++) {
            uint32 score = scores[i];

            for (uint32 k = 0; k < tempRankings.length; k++) {
                if (score == tempRankings[k].score){
                    rankings[i] = tempRankings[k];
                }
            }

        }
      
        highestScore = rankings[0].score;
    }

 

    // Rescue Anythings from contract (BNB or BOOMM tokeken)
    function withdrawToken(address _tokenContract, uint256 _amount) external onlyOwner {
        IERC20 tokenContract = IERC20(_tokenContract);
        tokenContract.transfer(msg.sender, _amount);
    }

    // KA-BOOMM Config Logic 
    function setKABOOMMEnabled(bool enabled) external onlyTeam {
        require(enabled != _KABOOMMEnabled, "Must set a new value for KABOOMMEnabled");
        _KABOOMMEnabled = enabled;
    }

    function setEntryfee(uint256 _BOOMMEntryFee, uint256 _BNBEntryFee) external onlyOwner {
        BOOMMEntryFee = _BOOMMEntryFee;
        BNBEntryFee = _BNBEntryFee;
    }


    function setTeamMember(address member, bool isTeamMember) external onlyOwner {
        _team[member] = isTeamMember;
    }

    function setBlacklist(address wallet, bool isBlacklisted) external onlyTeam {
        players[wallet]._isBlacklisted = isBlacklisted;
    }


    function setWinnerFeePercentage(uint8 newPercentage) external onlyOwner {
        require(newPercentage <= 60, "Cannot set house fee percentage higher than 50 percent");
        winnerFeePercentage = newPercentage;
    }

    function setUtilFeeReceiver(address newReceiver) external onlyOwner {
        require(newReceiver != address(0x0), "Can't set the zero address as the receiver");
        utilFeeReceiver = newReceiver;
    }

    function setUtilFeePercentage(uint8 newPercentage) external onlyOwner {
        require(newPercentage <= 20, "Cannot set dev fee percentage higher than 20 percent");
        utilFeePercentage = newPercentage;
    }

    function setReferrerFeePercentage(uint8 newPercentage) external onlyOwner {
        require(newPercentage <= 20, "Cannot set dev fee percentage higher than 20 percent");
        referrerFeePercentage = newPercentage;
    }



}