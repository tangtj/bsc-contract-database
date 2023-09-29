// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;


interface IPancakeRouter02 {
    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function WETH() external pure returns (address);
    // Add other functions from the PancakeSwap Router contract if needed
}


abstract contract FeeMechanism {
    address public owner;
    uint256 public transferFeePercentage;

    event TransferFeePercentageUpdated(uint256 newFeePercentage);

    constructor() {
        owner = msg.sender;
        transferFeePercentage = 5; // Default transfer fee percentage (1%)
    }

    modifier onlyOwner() {

        // Modifier to restrict access to the owner only

        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    function updateTransferFeePercentage(uint256 newFeePercentage) public onlyOwner {

        // For update transfer fee percentage

        require(newFeePercentage <= 100, "Fee percentage must be between 0 and 100");
        transferFeePercentage = newFeePercentage;

        emit TransferFeePercentageUpdated(newFeePercentage);
    }

    function calculateTransferFee(uint256 amount) public view returns (uint256) {

        // Calculate fee

        return (amount * transferFeePercentage) / 100;
    }
}


contract BitIntelligence is FeeMechanism{
    string public name;
    string public symbol;
    uint8 public decimal; // 10^9
    uint256 public totalSupply; // 21M
    bool public paused;
    address public burner;
    bool public burnerPaused;
    bool public swapEnabled; // Flag to enable/disable token swapping
    address public taxation_account_dao_and_nft;
    address public taxation_account_marketing;
    address public taxation_account_liquidity;
    bool public lockingPaused;
    bool public vestingPaused;
    uint256 public dailyLimit;
    uint256 public dailyLimitPercentage;
    address public pancakeRouterAddress = 0x05fF2B0DB69458A0750badebc4f9e13aDd608C7F;
    IPancakeRouter02 public pancakeRouter;
    uint256 public feePercentage;

    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimal,
        uint256 _totalSupply,
        bool _paused,
        bool _burnerPaused,
        bool _lockingPaused,
        uint256 _feePercentage,
        address _taxation_account_dao_and_nft,
        address _taxation_account_marketing,
        address _taxation_account_liquidity
    ){
        name = _name;
        symbol = _symbol;
        decimal = _decimal;
        totalSupply = _totalSupply * 10**_decimal;
        balanceOf[msg.sender] = totalSupply;
        paused = _paused;
        burnerPaused = _burnerPaused;
        swapEnabled = true;
        lockingPaused = _lockingPaused;
        taxation_account_dao_and_nft = _taxation_account_dao_and_nft;
        taxation_account_marketing = _taxation_account_marketing;
        taxation_account_liquidity = _taxation_account_liquidity;
        pancakeRouter = IPancakeRouter02(pancakeRouterAddress);
        tokenomicsAccounts.push(msg.sender);
        vestingPaused = false;
        feePercentage = _feePercentage;
        dailyLimitPercentage  = 10;
    }

    mapping (address => uint256) public balanceOf;
    mapping (address => mapping (address => uint256)) public allowance; 
    mapping(address => bool) public frozenAccounts;
    address[] public  tokenomicsAccounts;
    mapping(address => uint256) public lockTimestamps;
    mapping (address => uint256) public lockedAmount;
    address[] public lockedUsers;
    mapping(address => uint256) public lastTransferTimestamps;
    mapping(address => uint256) public transferredToday;

    event Transfer(address indexed  from, address indexed to, uint256 value);
    event Approval(address indexed  owner, address indexed spender, uint256 value);
    event Burn(address indexed from, uint256 amount);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event Paused(bool isPaused);
    event BurnerUpdated(address indexed previousBurner, address indexed newBurner);
    event BurnerPaused(bool isPaused);
    event AccountFrozen(address indexed account);
    event AccountUnfrozen(address indexed account);
    event TokenSwapped(address indexed user, uint256 amount);
    event Staked(address indexed staker, uint256 amount);
    event Withdrawn(address indexed staker, uint256 amount);
    event RewardsClaimed(address indexed staker, uint256 amount);
    event RewardsAdded(uint256 amount);
    event TokensReleased(address indexed beneficiary, uint256 amount);
    event TokensLocked(address indexed account, uint256 untilTimestamp);
    event TokensUnlocked(address indexed lockedUser, uint256 unlockedAmount);
    event LockingPaused(bool isPaused);

    modifier whenNotPaused() {

        // Modifier to check is contract pused or not

        require(!paused, "Contract is paused");
        _;
    }

    modifier onlyBurner() {

        // Modifier to restrict access to the burner only

        require(msg.sender == burner, "Only the burner role can call this function");
        _;
    }

    modifier whenBurnerNotPaused() {

        // Modifier to check is Burner pused or not

        require(!burnerPaused, "Burner role is paused. Cannot burn tokens.");
        _;
    }

    modifier notFrozen(address account) {
        require(!frozenAccounts[account], "Account is frozen");
        _;
    }

    
    function transfer(address to, uint256 value) public whenNotPaused notFrozen(msg.sender) notFrozen(to) returns (bool success){

        // Allows users to transfer tokens from their own account to another address.
        require(block.timestamp >= lockTimestamps[msg.sender], "Tokens are still locked");

        if (lockingPaused) {
            if(!vestingPaused){
                dailyLimit = (balanceOf[msg.sender] * dailyLimitPercentage) / 100;
                require(value <= dailyLimit, "Transfer amount exceeds daily limit");
                if (lastTransferTimestamps[msg.sender] + 1 days <= block.timestamp) {
                    lastTransferTimestamps[msg.sender] = block.timestamp;
                    transferredToday[msg.sender] = value;
                } else {
                    // Check if the transferred amount plus the new amount exceeds the daily limit
                    require(transferredToday[msg.sender] + value <= dailyLimit, "Daily limit exceeded");
                    transferredToday[msg.sender] += value;
                }
            }
        }

        if (!lockingPaused) {
            if (!isTokenomicsAccount(to)){
                lockTokens(block.timestamp * 540 days, to);
                lockedAmount[to] += value;
            }
        }

        require(to != address(0), "Invalid Address");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public whenNotPaused notFrozen(spender) returns (bool success) {

        // Grants approval to another address to spend tokens on behalf of the sender.

        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function _allowance(address owner, address spender) public view returns (uint256) {
        return allowance[owner][spender];
    }

    function transferFrom(address from, address to, uint256 value) public whenNotPaused notFrozen(from) notFrozen(to) returns (bool success) {

        // Allows a third-party address (approved by the token holder) to transfer tokens from the token holder's account to another address.

        require(from != address(0), "Invalid address");
        require(to != address(0), "Invalid address");
        require(value > 0, "Transfer amount must be greater than 0");
        require(block.timestamp >= lockTimestamps[from], "Tokens are still locked");
        
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");

        if (lockingPaused) {
            if(!vestingPaused){
                dailyLimit = (balanceOf[msg.sender] * dailyLimitPercentage) / 100;
                require(value <= dailyLimit, "Transfer amount exceeds daily limit");
                if (lastTransferTimestamps[msg.sender] + 1 days <= block.timestamp) {
                    lastTransferTimestamps[msg.sender] = block.timestamp;
                    transferredToday[msg.sender] = value;
                } else {
                    // Check if the transferred amount plus the new amount exceeds the daily limit
                    require(transferredToday[msg.sender] + value <= dailyLimit, "Daily limit exceeded");
                    transferredToday[msg.sender] += value;
                }
            }
        }
        
        if (!lockingPaused) {
            if (!isTokenomicsAccount(to)){
                lockTokens(block.timestamp * 540 days, to);
                lockedAmount[to] += value;
            }
        }

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }

    function checkbalanceOf(address account) public view returns (uint256)  {

        // Retrieves the token balance of a given address.

        return balanceOf[account];
    }

    function checkTaxationAccounts() public view  onlyOwner returns (address, address, address)  {

            // Retrieves the Taxation address.

            return (taxation_account_dao_and_nft, taxation_account_marketing, taxation_account_liquidity);
        }

    function checkTotalSupply() public view returns (uint256) {

        // Returns the total supply of the token.

        return totalSupply;
    }

    function checkAllowance(address spender) public view returns (uint256) {

        // Returns the current approved allowance for a specific spender and owner address pair.

        return allowance[owner][spender];
    }

    function burn(uint256 amount) public whenNotPaused onlyBurner whenBurnerNotPaused returns (bool success) {

        // Allows token holders to burn (destroy) a specific amount of their tokens, reducing the total supply.

        require(amount > 0, "Amount must be greater than zero");
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        require(amount >= 5000, "Amount should be greater then 5000");

        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;

        emit Burn(msg.sender, amount);
        return true;
    }


    function transferOwnership(address newOwner) public onlyOwner notFrozen(newOwner) returns (address, bool){

        // Transfers ownership of the token contract to another address.

        require(newOwner != address(0), "Invalid new owner address");

        address previousOwner = owner;
        owner = newOwner;

        emit OwnershipTransferred(previousOwner, newOwner);
        return (owner, true);

    }


    function pause() public onlyOwner returns (bool) {

        // Temporarily halts certain functionalities in the contract.

        paused = true;
        emit Paused(true);
        return true;
    }

    function unpause() public onlyOwner returns (bool) {

        // Resumes functionalities in the contract after it has been paused.

        paused = false;
        emit Paused(false);
        return true;
    }

    function decimals() public view returns (uint8) {

        // Returns the number of decimal places used by the token.

        return decimal;
    }


    function checkTokenName() public view returns (string memory){

        // Returns the name of the token.

        return name;
    }

    function checkTokenSymbol() public view returns (string memory){

        // Returns the symbol (ticker) of the token.

        return symbol;
    }

    function setBurner(address newBurner) public onlyOwner whenNotPaused notFrozen(newBurner) returns (address, bool) {

        // Allows the contract owner to set a new address as the burner, allowing them to burn tokens.

        require(newBurner != address(0), "Invalid burner address");

        address previousBurner = burner;
        burner = newBurner;

        emit BurnerUpdated(previousBurner, newBurner);

        return (burner, true);
    }

    function pauseBurner() public onlyOwner returns (bool){

        // Temporarily halts the token burning process.

        burnerPaused = true;

        emit BurnerPaused(burnerPaused);
        return true;
    }

    function unPauseBurner() public onlyOwner returns (bool) {

        // Resumes the token burning process after it has been paused.

        burnerPaused = false;

        emit BurnerPaused(burnerPaused);
        return true;
    }

    function freezeAccount(address account) public onlyOwner returns (bool) {

        // Allows the contract owner to freeze specific account.

        require(account != address(0), "Invalid account address");

        frozenAccounts[account] = true;

        emit AccountFrozen(account);
        return true;
    }

    function unfreezeAccount(address account) public onlyOwner returns (bool) {

        // Allows the contract owner to unfreeze specific account.

        require(account != address(0), "Invalid account address");

        frozenAccounts[account] = false;

        emit AccountUnfrozen(account);
        return true;
    }

    function isOwner(address account) public view returns (bool) {
        return account == owner;
    }

    function pauseLocking() public onlyOwner returns (bool){

        lockingPaused = true;

        emit LockingPaused(lockingPaused);
        return true;
    }

    function unPauseLocking() public onlyOwner returns (bool){

        lockingPaused = false;

        emit LockingPaused(lockingPaused);
        return true;
    }

    function lockTokens(uint256 lockDuration, address lockedUser) private {
        require(lockDuration > 0, "Lock duration must be greater than 0");

        lockTimestamps[lockedUser] = lockDuration;
        lockedUsers.push(lockedUser);
        emit TokensLocked(lockedUser, lockTimestamps[lockedUser]);
    }

    function addToTokenomicsAccounts(address account) public onlyOwner {
        tokenomicsAccounts.push(account);
    }

    function isTokenomicsAccount(address account) public view returns (bool) {
        for (uint256 i = 0; i < tokenomicsAccounts.length; i++) {
            if (tokenomicsAccounts[i] == account) {
                return true;
            }
        }
        return false;
    }

    function unlockSpecificUserToken(address userAddress) public onlyOwner {
        uint256 unlockedamount = lockedAmount[userAddress];
        lockTimestamps[userAddress] = 0; // Reset lock timestamp
        lockedAmount[userAddress] = 0;
        lockingPaused = true;

        emit TokensUnlocked(userAddress, unlockedamount);

    }

    function unlockAllTokens() public onlyOwner {
        for (uint256 i = 0; i < lockedUsers.length; i++) {
            address lockedUser = lockedUsers[i];

            uint256 unlockedamount = lockedAmount[lockedUser];

            lockTimestamps[lockedUser] = 0; // Reset lock timestamp
            lockedAmount[lockedUser] = 0;
            lockingPaused = true;

            emit TokensUnlocked(lockedUser, unlockedamount);
            
        }
    }
    
    function _lockedAmount(address account) public view returns(uint256) {
        return lockedAmount[account];
    }  

    function getTokenomicsAccount() public view onlyOwner returns (address[] memory){
        return tokenomicsAccounts;
    }

    function setDailyLimit(uint256 _newLimitPercentage) public onlyOwner {
        dailyLimitPercentage = _newLimitPercentage;
    }

    function pusedVesting() public onlyOwner {
        vestingPaused = true;
    }

    function unPusedVesting() public onlyOwner {
        vestingPaused = false;
    }

    function _approve(address owner, address spender, uint256 amount) public {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        allowance[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

     function swapTokensForBNB(uint256 tokenAmount) internal whenNotPaused notFrozen(msg.sender) {
        require(swapEnabled, "Token swapping is disabled");

        uint256 feeAmount = (tokenAmount * feePercentage) / 100;

        require(address(this).balance >= feeAmount, "Insufficient BNB balance for fee");

        uint256 adjustedTokenAmount = tokenAmount - feeAmount;

        _approve(address(this), address(pancakeRouter), adjustedTokenAmount);

        address[] memory path = new address[](2);
        path[0] = address(this); 
        path[1] = pancakeRouter.WETH();

        pancakeRouter.swapExactTokensForTokens(
            adjustedTokenAmount,
            0, 
            path,
            address(this), 
            block.timestamp + 600
        );

        payable(taxation_account_dao_and_nft).transfer((feeAmount * 40) / 100);
        payable(taxation_account_marketing).transfer((feeAmount * 20) / 100);
        payable(taxation_account_liquidity).transfer((feeAmount * 40) / 100);

    }
    
    function allocateTokensToOwner() public onlyOwner {
        require(msg.sender == owner, "Only the owner can allocate tokens");
        // Transfer the total supply to the owner
        balanceOf[owner] = totalSupply;
    }
}