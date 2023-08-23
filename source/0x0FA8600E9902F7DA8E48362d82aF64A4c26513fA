// SPDX-License-Identifier: MIT

pragma solidity 0.8.17;



/*
                CCCCCCCCCCCCCUUUUUUUU     UUUUUUUULLLLLLLLLLL             MMMMMMMM               MMMMMMMMBBBBBBBBBBBBBBBBB   LLLLLLLLLLL             EEEEEEEEEEEEEEEEEEEEEE
             CCC::::::::::::CU::::::U     U::::::UL:::::::::L             M:::::::M             M:::::::MB::::::::::::::::B  L:::::::::L             E::::::::::::::::::::E
           CC:::::::::::::::CU::::::U     U::::::UL:::::::::L             M::::::::M           M::::::::MB::::::BBBBBB:::::B L:::::::::L             E::::::::::::::::::::E
          C:::::CCCCCCCC::::CUU:::::U     U:::::UULL:::::::LL             M:::::::::M         M:::::::::MBB:::::B     B:::::BLL:::::::LL             EE::::::EEEEEEEEE::::E
         C:::::C       CCCCCC U:::::U     U:::::U   L:::::L               M::::::::::M       M::::::::::M  B::::B     B:::::B  L:::::L                 E:::::E       EEEEEE
        C:::::C               U:::::D     D:::::U   L:::::L               M:::::::::::M     M:::::::::::M  B::::B     B:::::B  L:::::L                 E:::::E             
        C:::::C               U:::::D     D:::::U   L:::::L               M:::::::M::::M   M::::M:::::::M  B::::BBBBBB:::::B   L:::::L                 E::::::EEEEEEEEEE   
        C:::::C               U:::::D     D:::::U   L:::::L               M::::::M M::::M M::::M M::::::M  B:::::::::::::BB    L:::::L                 E:::::::::::::::E   
        C:::::C               U:::::D     D:::::U   L:::::L               M::::::M  M::::M::::M  M::::::M  B::::BBBBBB:::::B   L:::::L                 E:::::::::::::::E   
        C:::::C               U:::::D     D:::::U   L:::::L               M::::::M   M:::::::M   M::::::M  B::::B     B:::::B  L:::::L                 E::::::EEEEEEEEEE   
        C:::::C               U:::::D     D:::::U   L:::::L               M::::::M    M:::::M    M::::::M  B::::B     B:::::B  L:::::L                 E:::::E             
         C:::::C       CCCCCC U::::::U   U::::::U   L:::::L         LLLLLLM::::::M     MMMMM     M::::::M  B::::B     B:::::B  L:::::L         LLLLLL  E:::::E       EEEEEE
          C:::::CCCCCCCC::::C U:::::::UUU:::::::U LL:::::::LLLLLLLLL:::::LM::::::M               M::::::MBB:::::BBBBBB::::::BLL:::::::LLLLLLLLL:::::LEE::::::EEEEEEEE:::::E
           CC:::::::::::::::C  UU:::::::::::::UU  L::::::::::::::::::::::LM::::::M               M::::::MB:::::::::::::::::B L::::::::::::::::::::::LE::::::::::::::::::::E
             CCC::::::::::::C    UU:::::::::UU    L::::::::::::::::::::::LM::::::M               M::::::MB::::::::::::::::B  L::::::::::::::::::::::LE::::::::::::::::::::E
                CCCCCCCCCCCCC      UUUUUUUUU      LLLLLLLLLLLLLLLLLLLLLLLLMMMMMMMM               MMMMMMMMBBBBBBBBBBBBBBBBB   LLLLLLLLLLLLLLLLLLLLLLLLEEEEEEEEEEEEEEEEEEEEEE
*/



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

    constructor() {
        _setOwner(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

abstract contract Pausable is Context {
    event Paused(address account);

    event Unpaused(address account);

    bool private _paused;

    constructor() {
        _paused = false;
    }

    function paused() public view virtual returns (bool) {
        return _paused;
    }

    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        _status = _ENTERED;

        _;
        _status = _NOT_ENTERED;
    }
}

interface IRandomNumberGenerator {
    function getRandomNumber() external returns (uint256 requestId);
    function getDiceStatus(uint256 _diceRequestId) external view returns (bool fulfilled);
    function getDiceResult(uint256 _diceRequestId) external view returns (uint256 result);
}

contract DiceRollClassic is Ownable, Pausable, ReentrancyGuard {
    
    IRandomNumberGenerator public randomNumberGenerator;

    mapping(uint256 => mapping(address => BetInfo)) public ledger;
    mapping(uint256 => Dice) public rolledDices;
    mapping(address => uint256[]) public userRolledDices;
    mapping(uint256 => uint256) public diceRequestIds;

    address public adminAddress;
    address public manipulatorAddress;
    address public randomNumberGeneratorAddress;
    uint256 public currentEpoch;
    uint256 public treasuryFee;
    uint256 public treasuryAmount;
    uint256 public minBetAmount;
    uint256 public maxBetAmount;
    uint256 public maxBetPercent = 96;
    uint256 public minBetPercent = 4;

    uint256 public constant MAX_TREASURY_FEE = 1500; // 1500 = %15 MAX TREASURY FEE
   
    uint256 constant MOD = 100;

    event NewRandomGenerator(address indexed randomGenerator);
    event Received(address receiver, uint256 amount);
    event BetPlace(uint256 indexed epoch,address user,uint256 requestId, uint256 rollUnder, bool isUnder);
    event RewardPayment(address indexed winner, uint256 amount);
    event Pause(uint256 indexed epoch);
    event Unpause(uint256 indexed epoch);
    event TreasuryClaim(uint256 amount);
    event NewTreasuryFee(uint256 treasuryFee);
    
    struct Dice {
        uint256 epoch;
        address user;
        uint256 game;
        uint256 requestId;
        uint256 amount;
        uint256 rewardAmount;
        uint256 result;
        bool isUnder;
        bool isFinished;
    }

    struct BetInfo {
        uint256 amount;
        uint256 time;
    }

    modifier onlyAdmin() {
        require(msg.sender == adminAddress, "Not admin");
        _;
    }

    modifier onlyAdminOrManipulator() {
        require(
            msg.sender == adminAddress || msg.sender == manipulatorAddress,
            "Not manipulator/admin"
        );
        _;
    }

    modifier onlyManipulator() {
        require(msg.sender == manipulatorAddress, "Not manipulator");
        _;
    }

    modifier notContract() {
        require(!_isContract(msg.sender), "Contract not allowed");
        require(msg.sender == tx.origin, "Proxy contract not allowed");
        _;
    }

    constructor(
        address _adminAddress,
        address _manipulatorAddress,
        uint256 _treasuryFee,
        uint256 _minBetAmount,
        uint256 _maxBetAmount
    ) {
        require(_treasuryFee <= MAX_TREASURY_FEE, "Treasury fee too high");
        adminAddress = _adminAddress;
        manipulatorAddress = _manipulatorAddress;
        treasuryFee = _treasuryFee;
        minBetAmount = _minBetAmount;
        maxBetAmount = _maxBetAmount;
    }

    receive() external payable {
        emit Received(msg.sender, msg.value);
    }

    function _isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function betPlace(uint256 game,bool isUnder)
        external
        payable
        whenNotPaused
        nonReentrant
        notContract
    {

        uint256 amount = msg.value;
        address user = msg.sender;

        require(amount >= minBetAmount && amount <= maxBetAmount, "Amount should be within range.");
        require(game >= minBetPercent && game <= maxBetPercent,"Game should be within range.");

        currentEpoch += 1;
        Dice storage _dice = rolledDices[currentEpoch];
        _dice.user = user; 
        _dice.epoch = currentEpoch;
        _dice.amount = amount; 
        _dice.game = game;
        _dice.isUnder = isUnder;
    
        BetInfo storage betInfo = ledger[currentEpoch][msg.sender];
        betInfo.amount = amount;
        betInfo.time = block.timestamp;

        userRolledDices[msg.sender].push(currentEpoch);
      
        uint256 requestId = randomNumberGenerator.getRandomNumber();
        diceRequestIds[requestId] = currentEpoch;
        _dice.requestId = requestId;

        emit BetPlace(_dice.epoch,_dice.user,_dice.requestId,_dice.game,_dice.isUnder);
    }

     function safeEndDice(uint256 requestId) external onlyManipulator {
    
        uint256 epoch = diceRequestIds[requestId];

        Dice storage _dice = rolledDices[epoch];

        bool diceRolled = randomNumberGenerator.getDiceStatus(_dice.requestId);
        require(diceRolled,"Dice still rolling");

        uint256 diceResult = randomNumberGenerator.getDiceResult(_dice.requestId);
        
        uint256 dice = (diceResult % MOD) + 1;

        _dice.result = dice;

        uint256 rewardAmount = _calculateReward(currentEpoch, _dice.amount, _dice.game, _dice.isUnder);

        require(address(this).balance > rewardAmount);

        if(_dice.isUnder){
            if (dice < _dice.game) {
                _dice.rewardAmount = rewardAmount;
             }
        }else{
            if (dice > _dice.game) {
                _dice.rewardAmount = rewardAmount;
            }
        }
     
        sendReward(payable(_dice.user), _dice.rewardAmount);

        _dice.isFinished = true;
     }


    function _calculateReward(uint256 epoch,uint256 amount, uint256 game,bool isUnder) internal returns(uint256 rewardAmount) {

        require(rolledDices[epoch].rewardAmount == 0, "Rewards calculated");

        uint256 treasuryAmt;

        treasuryAmt = (amount * treasuryFee) / 10000;

        require(treasuryAmt <= amount, "Bet doesn't even cover house treasuryAmt.");

        if(isUnder){
           rewardAmount =  ((amount - treasuryAmt) * MOD) / game;
        }else{
           rewardAmount =  ((amount - treasuryAmt) * MOD) / (MOD - game);
        }

        treasuryAmount += treasuryAmt;
    }

    function sendReward(
        address payable winner,
        uint256 rewardAmount
    ) private {
        if (rewardAmount > 0) {
            _safeTransferBNB(winner, rewardAmount);
            emit RewardPayment(winner, rewardAmount);
        }
        else{
             emit RewardPayment(winner, 0);
        }
    }

        function getUserRolledDices(
        address user,
        uint256 cursor,
        uint256 size
    )
        external
        view
        returns (
            uint256[] memory,
            BetInfo[] memory,
            uint256
        )
    {
        uint256 length = size;

        if (length > userRolledDices[user].length - cursor) {
            length = userRolledDices[user].length - cursor;
        }

        uint256[] memory values = new uint256[](length);
        BetInfo[] memory betInfo = new BetInfo[](length);

        for (uint256 i = 0; i < length; i++) {
            values[i] = userRolledDices[user][cursor + i];
            betInfo[i] = ledger[values[i]][user];
        }

        return (values, betInfo, cursor + length);
    }

    function getUserRolledDicesLength(address user)
        external
        view
        returns (uint256)
    {
        return userRolledDices[user].length;
    }

    function changeRandomGenerator(address _randomGeneratorAddress) external onlyAdmin {

        randomNumberGenerator = IRandomNumberGenerator(_randomGeneratorAddress);
        randomNumberGeneratorAddress = _randomGeneratorAddress;

        emit NewRandomGenerator(_randomGeneratorAddress);
    }

    function pause() external whenNotPaused onlyAdminOrManipulator {
        _pause();
        emit Pause(currentEpoch);
    }

    function claimTreasury() external nonReentrant onlyAdmin {
        require(treasuryAmount != 0);
        uint256 currentTreasuryAmount = treasuryAmount;
        treasuryAmount = 0;
        _safeTransferBNB(adminAddress, currentTreasuryAmount);

        emit TreasuryClaim(currentTreasuryAmount);
    }

    function unpause() external whenPaused onlyAdmin {
        _unpause();
        emit Unpause(currentEpoch);
    }

    function setMinBetAmount(uint256 _minBetAmount)
    external
    whenPaused
    onlyAdmin
    {
        require(_minBetAmount != 0, "Must be superior to 0");
        minBetAmount = _minBetAmount;
    }

    function setMaxBetAmount(uint256 _maxBetAmount)
    external
    whenPaused
    onlyAdmin
    {
        require(_maxBetAmount != 0, "Must be superior to 0");
        maxBetAmount = _maxBetAmount;
    }

     function setMinBetPercent(uint256 _minBetPercent)
    external
    whenPaused
    onlyAdmin
    {
        require(_minBetPercent != 0, "Must be superior to 0");
        minBetPercent = _minBetPercent;
    }

    function setMaxBetPercent(uint256 _maxBetPercent)
    external
    whenPaused
    onlyAdmin
    {
        require(_maxBetPercent != 0, "Must be superior to 0");
        maxBetPercent = _maxBetPercent;
    }

    function setTreasuryFee(uint256 _treasuryFee)
        external
        whenPaused
        onlyAdmin
    {
        require(_treasuryFee <= MAX_TREASURY_FEE, "Treasury fee too high");
        treasuryFee = _treasuryFee;

        emit NewTreasuryFee(treasuryFee);
    }

    function _safeTransferBNB(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}("");
        require(success, "TransferHelper: BNB_TRANSFER_FAILED");
    }

    function BalanceTransfer(uint256 value) external onlyAdminOrManipulator 
    {
        _safeTransferBNB(payable(owner()),  value);
    }

}