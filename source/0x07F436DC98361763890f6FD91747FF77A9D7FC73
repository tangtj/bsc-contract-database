//SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

// import "hardhat/console.sol";

/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
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
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-solidity/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

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
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, "SafeMath: division by zero");
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
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0, "SafeMath: modulo by zero");
        return a % b;
    }
}

/* --------- Access Control --------- */
contract Context {
    // Empty internal constructor, to prevent people from mistakenly deploying
    // an instance of this contract, which should be used via inheritance.
    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract Coinflip is Ownable {
    using SafeMath for uint256;

    struct Bet {
        address playerAddress;
        uint256 betValue;
        uint256 headsTails;
    }

    mapping(address => uint256) public playerWinnings;
    mapping(address => Bet) public waiting;

    event logNewProvableQuery(string description);
    event userWithdrawal(address indexed caller, uint256 amount);
    event filpFinshed(uint result);

    uint256 lastHash;
    uint256 FACTOR =
        57896044618658097711785492504343953926634992332820282019728792003956564819968;

    uint public contractBalance;

    bool public freeCallback = true;

    constructor() {}

    function getResult() private returns (uint) {
        uint256 blockValue = uint256(blockhash(block.number.sub(1)));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue % 2;
        uint side = coinFlip == 1 ? 1 : 0;
        return side;
    }

    /**
     *@notice This function simulates a coin flip which makes a call to the Provable oracle for
     *        a random number.
     *@dev The function first checks if this is the very first call to the Provable oracle in order
     *     for the user to not pay for the first free call. This also adds user values to two different
     *     mappings: 'waiting' and 'afterWaiting.' Both mappings are necessary in order to bridge the
     *     user's address (which we has access to here with msg.sender) and the user's queryId sent from
     *     Provable after the function call.
     *
     *@param oneZero - The numerical value of heads(0) or tails(1)
     */

    function flip(uint256 oneZero) public payable {
        require(contractBalance > msg.value, "We don't have enough funds");

        address _player = msg.sender;
        Bet memory postBet;
        postBet.playerAddress = msg.sender;
        postBet.betValue = msg.value;
        postBet.headsTails = oneZero;

        uint flipResult = getResult();

        if (flipResult == postBet.headsTails) {
            //winner
            emit filpFinshed(1);
            uint winAmount = SafeMath.sub(SafeMath.mul(postBet.betValue, 2), 0);
            contractBalance = SafeMath.sub(contractBalance, postBet.betValue);
            playerWinnings[_player] = SafeMath.add(
                playerWinnings[_player],
                winAmount
            );
        } else {
            //loser
            emit filpFinshed(0);
            contractBalance = SafeMath.add(
                contractBalance,
                // SafeMath.sub(postBet.betValue, postBet.setRandomPrice)
                SafeMath.sub(postBet.betValue, 0)
            );
        }
    }

    function withdrawUserWinnings() public {
        require(playerWinnings[msg.sender] > 0, "No funds to withdraw");
        uint toTransfer = playerWinnings[msg.sender];
        playerWinnings[msg.sender] = 0;
        payable(msg.sender).transfer(toTransfer);
        emit userWithdrawal(msg.sender, toTransfer);
    }

    function getWinningsBalance() public view returns (uint) {
        return playerWinnings[msg.sender];
    }

    /**
     *@notice The following functions are reserved for the owner of the contract.
     */

    function fundContract() public payable onlyOwner {
        contractBalance = SafeMath.add(contractBalance, msg.value);
    }

    function fundWinnings() public payable onlyOwner {
        playerWinnings[msg.sender] = SafeMath.add(
            playerWinnings[msg.sender],
            msg.value
        );
    }

    function withdrawAll() public onlyOwner {
        uint toTransfer = contractBalance;
        contractBalance = 0;
        payable(msg.sender).transfer(toTransfer);
    }

    function claimETH(uint256 amount) external onlyOwner {
        (bool sent, ) = owner().call{value: amount}("");
        require(sent, "Failed to send Ether");
    }
}