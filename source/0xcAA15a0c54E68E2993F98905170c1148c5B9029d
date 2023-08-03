// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;


// ##   ##    ##     ###  ##   ## ##    ## ##    ## ##            
//  ## ##      ##      ## ##  ##   ##  ##   ##  ##   ##           
// # ### #   ## ##    # ## #  ##       ##   ##  ####              
// ## # ##   ##  ##   ## ##   ##  ###  ##   ##   #####            
// ##   ##   ## ###   ##  ##  ##   ##  ##   ##      ###           
// ##   ##   ##  ##   ##  ##  ##   ##  ##   ##  ##   ##           
// ##   ##  ###  ##  ###  ##   ## ##    ## ##    ## ##            
                                                               
//  ## ##   ##   ##    ##     ### ##   #### ##            ## ##    ## ##   ###  ##  #### ##  ### ##     ##      ## ##   #### ##  
// ##   ##   ## ##      ##     ##  ##  # ## ##           ##   ##  ##   ##    ## ##  # ## ##   ##  ##     ##    ##   ##  # ## ##  
// ####     # ### #   ## ##    ##  ##    ##              ##       ##   ##   # ## #    ##      ##  ##   ## ##   ##         ##     
//  #####   ## # ##   ##  ##   ## ##     ##              ##       ##   ##   ## ##     ##      ## ##    ##  ##  ##         ##     
//     ###  ##   ##   ## ###   ## ##     ##              ##       ##   ##   ##  ##    ##      ## ##    ## ###  ##         ##     
// ##   ##  ##   ##   ##  ##   ##  ##    ##              ##   ##  ##   ##   ##  ##    ##      ##  ##   ##  ##  ##   ##    ##     
//  ## ##   ##   ##  ###  ##  #### ##   ####              ## ##    ## ##   ###  ##   ####    #### ##  ###  ##   ## ##    ####    

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

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

abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    /**
     * @dev Unauthorized reentrant call.
     */
    error ReentrancyGuardReentrantCall();

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        if (_status == _ENTERED) {
            revert ReentrancyGuardReentrantCall();
        }

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}
  

contract MangosApp is Ownable, ReentrancyGuard {

    address public usdt = 0x55d398326f99059fF775485246999027B3197955;
    IERC20 public token;

    uint256 private INIT_SECOND_PERCENT = 48611111;
    uint256 public INIT_MIN_DEPOSIT = 10;
    uint256 public INIT_MAX_DEPOSIT = 1000;
    uint256 public INIT_MIN_WITHDRAWAL = 1;
    uint256 public INIT_TRADE_FAIR = 0;
    uint128 private INIT_REF_INCREASED = 940;
    uint256 private INIT_REF_LIMIT = 116000;
    uint256[2] private AFFILIATE_PERCENTS_FERTILIZER = [80, 15];
    uint256[2] private AFFILIATE_PERCENTS_USDT = [60, 10];

    uint256 public totalInvested;
    uint256 public totalInvestors;
    uint256 public totalFairs;

    struct User {
        uint256 deposit;
        uint256 reinvested;
        uint256 earned;
        uint256 withdrawn;
        uint256 mangos;
        uint256 fertilizer;
        uint256 timestamp;
        address partner;
        uint256 refsTotal;
        uint256 refs1level;
        uint256 refearnUSDT;
        uint256 refearnFertilizer; 
        uint256 percentage;
        uint256 leaderBonus;
    }

    struct Fairs {
        uint256 wins;
        uint256 loses;
    }

    struct TradeFair {
        address player1;
        address player2;
        uint256 mangos;
        uint256 timestamp;
        address winner;
        uint256 roll;
    }

    mapping(address => User) public user; //user records
    mapping(address => mapping(uint256 => TradeFair)) public tradefair; //trade fair records
    mapping(address => Fairs) public fairs; // fair records
    mapping(uint256 => address) public waitingFairs; 
    address deadAddress = address(0); // Replace with your specific dead address if different


    constructor() {
        token = IERC20(usdt);
    }

    event ChangeUser(address indexed user, address indexed partner, uint256 amount);

    receive() external payable onlyOwner {}

    function PlantMango(uint amount, address partner) external nonReentrant {
    require(_msgSender() == tx.origin, "Function can only be called by a user account");
    require(amount >= (INIT_MIN_DEPOSIT * 1e18), "Min deposit is 10 USDT");
    require((user[_msgSender()].deposit + amount) < (INIT_MAX_DEPOSIT*1e18), "Max deposit limit has been exceeded");
    _updateprePayment(_msgSender());
    totalInvested += amount;
    totalInvestors += 1;
    user[_msgSender()].deposit += amount;

    if (user[_msgSender()].percentage == 0) {
        require(partner != _msgSender(), "Cannot set your own address as partner");
        address ref = user[partner].deposit == 0 ? deadAddress : partner;
        if (partner == deadAddress) {
            ref = deadAddress; // If the partner is the dead address, set ref as the dead address
        }
        user[ref].refs1level++;
        user[ref].refsTotal++;
        user[user[ref].partner].refsTotal++;
        user[_msgSender()].partner = ref;
        user[_msgSender()].percentage = INIT_SECOND_PERCENT;
        _updatePercentage(ref);
    }

    token.transferFrom(_msgSender(), address(this), amount);
    emit ChangeUser(_msgSender(), user[_msgSender()].partner, user[_msgSender()].deposit);

    // REF
    _traverseTree(user[_msgSender()].partner, amount);

    uint256 feeUSDT = (amount * 5) / 100;
    token.transfer(owner(), feeUSDT);
    }



    function Reinvest(uint256 amount) external nonReentrant {
        require(_msgSender() == tx.origin, "Function can only be called by a user account");
        require(amount >= (INIT_MIN_DEPOSIT * 1000000000000000000), "Min reinvest is 10 Fertilizer");
        _updateprePayment(_msgSender());
        require(amount <= user[_msgSender()].fertilizer, "Insufficient funds");
        user[_msgSender()].fertilizer -= amount;
        user[_msgSender()].deposit += amount / 10;
        user[_msgSender()].reinvested += amount / 10;
        emit ChangeUser(_msgSender(), user[_msgSender()].partner, user[_msgSender()].deposit);
    }

    function ReinvestMango(uint256 amount) external nonReentrant {
        require(_msgSender() == tx.origin, "Function can only be called by a user account");
        require(amount >= (INIT_MIN_DEPOSIT * 1000000000000000000), "Min reinvest is 10 Mangos");
        _updateprePayment(_msgSender());
        require(amount <= user[_msgSender()].mangos, "Insufficient funds");
        user[_msgSender()].mangos -= amount;
        user[_msgSender()].deposit += amount;
        user[_msgSender()].reinvested += amount;
        emit ChangeUser(_msgSender(), user[_msgSender()].partner, user[_msgSender()].deposit);
    }

    function Withdraw(uint256 amount) external nonReentrant {
        require(_msgSender() == tx.origin, "Function can only be called by a user account");
        require(amount > (INIT_MIN_WITHDRAWAL * 1000000000000000000), "Min withdrawal is 1 USDT");
        _updateprePayment(_msgSender());
        require(amount <= user[_msgSender()].mangos, "Insufficient funds");
        user[_msgSender()].mangos -= amount;
        user[_msgSender()].withdrawn += amount;
        token.transfer(_msgSender(), amount);
    }

    function checkReward(address account) public view returns(uint256) {
        uint256 RewardTime = block.timestamp - user[account].timestamp;
        RewardTime = (RewardTime >= 86400) ? 86400 : RewardTime;
        return (((user[account].deposit / 100 * user[account].percentage) / 10000000000) * RewardTime);
    }

    function _updateprePayment(address account) internal {
        uint256 pending = checkReward(_msgSender());
        user[account].timestamp = block.timestamp;
        user[account].mangos += pending;
        user[account].earned += pending;
    }

    function _traverseTree(address account, uint256 value) internal {
        if (value != 0) {
            for (uint8 i; i < 2; i++) {

                uint256 feeUSDT = ((value * AFFILIATE_PERCENTS_USDT[i]) / 1000);
                uint256 feeFertilizer = ((value * AFFILIATE_PERCENTS_FERTILIZER[i]) / 100);
                uint256 leaderBonus = (user[account].refs1level >= 2**i) ? ((feeFertilizer * 10) / 100) : 0;

                user[account].fertilizer += (feeFertilizer - leaderBonus);
                user[account].mangos += feeUSDT;
                user[account].leaderBonus += leaderBonus;
                user[account].refearnFertilizer += (feeFertilizer - leaderBonus);
                user[account].refearnUSDT += feeUSDT;

                if (account == deadAddress) {
                    return;
                }
                account = user[account].partner;
            }
        }
    }

    function _updatePercentage(address account) internal {
        uint256 percentage = INIT_SECOND_PERCENT + (user[account].refsTotal * INIT_REF_INCREASED);
        if (percentage > INIT_REF_LIMIT) {
            user[account].percentage = INIT_REF_LIMIT;
        } else {
            user[account].percentage = percentage;
        }
    }

    function changeMaxDeposit(uint256 amount) external onlyOwner {
        INIT_MAX_DEPOSIT = amount;
    }

    function changeFairStatus(uint256 status) external onlyOwner {
        INIT_TRADE_FAIR = status;
    }

    function createFair(uint256 fairType) public {
        require(INIT_TRADE_FAIR == 1, "Trade fair has been stopped temporarily by the owner");
        require(totalFairs < 2, "Maximum 2 fairs are allowed at a time");
        require(user[_msgSender()].deposit >= 1000000000000000000, "Minimum deposit of 1 USDT required to create a fair");
        require(fairType <= 2, "Invalid fair type");
        waitingFairs[getFairType(fairType)] = _msgSender();
        totalFairs++;
    }

        function getFairType(uint256 fairType) internal pure returns (uint256) {
        return [5000000000000000000, 50000000000000000000, 250000000000000000000][fairType];
    }

    function _randomNumber() internal view returns (uint256) {
        uint256 randomnumber = uint256(
            keccak256(
                abi.encodePacked(
                    block.timestamp +
                        block.difficulty +
                        ((
                            uint256(keccak256(abi.encodePacked(block.coinbase)))
                        ) / (block.timestamp)) +
                        block.gaslimit +
                        ((uint256(keccak256(abi.encodePacked(_msgSender())))) /
                            (block.timestamp)) +
                        block.number
                )
            )
        );

        return randomnumber % 100;
    }

    function _createFair(uint256 fairType) internal {
        tradefair[_msgSender()][fairType].timestamp = block.timestamp;
        tradefair[_msgSender()][fairType].player1 = _msgSender();
        tradefair[_msgSender()][fairType].player2 = address(0);
        tradefair[_msgSender()][fairType].winner = address(0);
        tradefair[_msgSender()][fairType].mangos = getFairType(fairType);
        tradefair[_msgSender()][fairType].roll = 0;
        user[_msgSender()].mangos -= tradefair[_msgSender()][fairType].mangos;
        waitingFairs[fairType] = _msgSender();
    }

    function _joinFair(uint256 fairType, address fairCreator) internal {
        tradefair[fairCreator][fairType].timestamp = block.timestamp;
        tradefair[fairCreator][fairType].player2 = _msgSender();
        user[_msgSender()].mangos -= tradefair[fairCreator][fairType].mangos;
        waitingFairs[fairType] = address(0);
    }

    function _fightFair(uint256 fairType, address fairCreator) internal {
        uint256 random = _randomNumber();
        uint256 wAmount = tradefair[fairCreator][fairType].mangos * 2;
        uint256 fee = (wAmount * 10) / 100;

        address winner = random < 50
            ? tradefair[fairCreator][fairType].player1
            : tradefair[fairCreator][fairType].player2;
        address loser = random >= 50
            ? tradefair[fairCreator][fairType].player1
            : tradefair[fairCreator][fairType].player2;

        user[winner].mangos += wAmount - fee;
        tradefair[fairCreator][fairType].winner = winner;
        tradefair[fairCreator][fairType].roll = random;

        fairs[winner].wins++;
        fairs[loser].loses++;
        totalFairs++;

        user[owner()].mangos += fee;
    }
}