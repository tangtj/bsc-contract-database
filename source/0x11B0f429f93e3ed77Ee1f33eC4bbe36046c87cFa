// SPDX-License-Identifier: MIT

/*
 *
 * ██╗   ██╗███╗   ██╗████████╗██████╗  █████╗  ██████╗███████╗██████╗ 
 * ██║   ██║████╗  ██║╚══██╔══╝██╔══██╗██╔══██╗██╔════╝██╔════╝██╔══██╗
 * ██║   ██║██╔██╗ ██║   ██║   ██████╔╝███████║██║     █████╗  ██████╔╝
 * ██║   ██║██║╚██╗██║   ██║   ██╔══██╗██╔══██║██║     ██╔══╝  ██╔══██╗
 * ╚██████╔╝██║ ╚████║   ██║   ██║  ██║██║  ██║╚██████╗███████╗██║  ██║
 *  ╚═════╝ ╚═╝  ╚═══╝   ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝╚══════╝╚═╝  ╚═╝
 *
 * @title Untracer V.3.00
 * @author	The Architect - bd36584a2ffb45a50d94c97e708e9f8307329a0a0394936e49b0de7eb00916da
 * @notice V. 3.00 Improved security with hidden 2FA code and timeframe control
 * Telegram: https://t.me/untracer    
 * Email: untracer@protonmail.com
 *   
 * INTRODUCTION:
 *      Untracer is a Token Mixer that runs 100% on the Ethereum and Binance Smart
 *      Chain blockchains. It consists of a contract that manages and securely
 *      stores deposits and withdrawals verified with a security token, which is 
 *      the first true implementation of a 2FA system in a decentralized environment.
 * 
 *      To execute the mixing, which makes the transaction history of funds in 
 *      wallets untraceable, only a few simple and fast steps are required. There
 *      is no web dApp interface for the protocol to avoid possible censorship of
 *      the system.
 * 
 *      The Untracer interface can be accessed through Etherscan for contract on the  
 *      Ethereum Network or through Bscscan for the contract on the Binance Smart Chain. 
 *
 *      The contract can be accessed from its official ethereum URL: untracer.eth
 *
 *      There are two phases to execute the mixing. Deposit, with password and  
 *      2FA code; and Withdrawal, with the password and the 2FA code.
 *
 * GUIDE:
 *  Step 1: 
 *      Go to the READ CONTRACT section of the page.
 *
 *      Choose a password of at least 18 characters, can be letters, numbers and 
 *      symbols. The more complicated it is, the more secure the transfer is. Make
 *      sure you save or write the password somewhere safe!
 * 
 *      Once the password is inserted, generate a hash code of the password with the 
 *      QUERY button. The system returns a hash code of the password (0xf4...). 
 *      Copy it and move to the WRITE CONTRACT section of the page. 
 *      Choose now a number between 1 and 99 and generate the hash of this number using
 *      the FAHASH function. Insert the number, press query and copy the hashed generated value.
 * 
 *  Step 2:
 *      Connect the wallet to etherscan from which funds will be sent from, select 
 *      deposit amount which can be (0.1, 0.5, 1, 5, 10), enter this amount in the DEPOSIT 
 *      section, using dot as the decimal separator, below add the previously generated
 *      hash code, then at CODE insert the hash generated from the number using the 
 *      FAHASH function in Step 1. 
 * 
 *      This verification number and the password will be the key to obtaining the 
 *      funds after mixing. After pressing the WRITE button, the wallet will open and
 *      require confirmation. Once confirmed, the funds will be moved to the contract. 
 *      The deposit is now completed!
 *
 *  Step 3:
 *      After some time (the longer the wait, the more private it is) refresh and 
 *      re-connect to the same page as before with a new, unused wallet. 
 *
 *      First, confirm the 2FA code number that was chosen during the deposit phase. 
 *      Enter the 2FA code in the CODE section and click WRITE. Your wallet will open, 
 *      requesting confirmation. You must confirm!
 *
 *      Now find the WITHDRAW section and enter the password chosen in Step 1. Do not 
 *      enter the hash, only the exact same password in plain text, that was chosen.
 *      Pressing the WITHDRAW button will need one last confirmation for the wallet 
 *      to receive the clean ETH that was deposited.
 *
 * Disclaimer: This tool was coded to keep privacy. Make a fair use of it.
 * 
 * Happy Mixing!                                                                     
*/


pragma solidity 0.8.18;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Untracer   {

    /*
     * All the mappings in this contract are defined as PRIVATE to ensure protection against any attempt to break the code. 
     * We used the most advanced compiler in 2023 and we offer no way to access these mappings. 
     */

    mapping (bytes32 => bool) private _store;                    // Keeps the password HASH.
    mapping (address => uint256) private _balance;               // Keeps the balance of the wallet.
    mapping (bytes32 => bool) private _burntHash;                // Keeps track of all used HASH.
    mapping (bytes32 => uint256) private sentAmount;             // Keeps track of the sent amount
    mapping (bytes32 => bytes32) private verificationNumber;     // Keeps track of the magic number to beat MEV flashbot

    /* We add a control: only who stakes an amount of the project token can use this contract
     * More users means more power and more security: here i set a mapping for supported token holder. Who holds can 
     * use this software. Array will be: TOKEN CA allowed. Not present, not allowed.
     */

    address[] private allowedTokenStake;
    mapping (address => uint256) private minimumToken;

    /* This variable has in it the owner of the contract. Cannot be changed */ 
    address private owner;

    /* This variable contains wallets for fee collection */ 
    address private _feescollector = 0x312109779D57f62cdfB16c66545fdeBbEC1440B5;
    address private _feescollector2 = 0x7d4E395FBDa9120b7bFd4eD96BFFFfb95cf068e5;

    /* Keep address of the SecureKEY that must be owned to withdraw */
    address private secureKeyAddress;

    /* This variable contains the amount of fee. Cannot be changed */ 
    uint private _feeAmount = 1;

    /* This variable create a lock/unlock state to avoid any reentrancy bug or exploit */
    bool private locked;

    /* This contract can handle only 0,1Eth, 0,5Eth, 1Eth, 5Eth, 10Eth and no more and no less. These variable express the value in WEI */
    uint256 private fixedAmount01 = 100000000000000000;
    uint256 private fixedAmount05 = 500000000000000000;
    uint256 private fixedAmount1 = 1000000000000000000;
    uint256 private fixedAmount5 = 5000000000000000000;
    uint256 private fixedAmount10 = 10000000000000000000;
    
    /* Register the timestamp as block number of deposit operation. Use the password hash as index */
    mapping (bytes32 => uint256) private timestampDeposit;

    /* Setting a variable that contains DELTA between deposit and withdraw */
    uint256 private deltaDepositWithdraw;

    /* In NO CASE this contract can be paused for WITHDRAW, but a STOP button for DEPOSIT is added. Default is TRUE */
    bool private masterSwitchDeposit;

    /* Here we keep track of all the ETH that passed in the mixer */
    uint256 private totalFilteredEth; 

    /* Modifier section. This section contains all the modifier used in the code. */

    /* 
        @dev    Modifier used to verify that who is broadcasting command is the owner of the contract
    */
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    /* 
        @dev    Modifier used to verify the address used is a valid address
    */
    modifier validAddress(address _addr) {
        require(_addr != address(0), "Not valid address");
        _;
    }
    
    /* 
        @dev    Avoid reentrancy bug and protect all the action, making impossibile to perform a task twice while executing
    */
    modifier noReentrancy() {
        require(!locked, "No reentrancy");

        locked = true;
        _;
        locked = false;
    }

    /* We create EVENT signal for action */
    event depositEvent(uint256 amount); 
    event withdrawEvent(bool committedWithdraw); 
    event stopDepositEvent(bool stopDeposit); 
    event addTokenProjectEvent(address newTokenAddress); 
    event removeTokenProjectEvent(address removedTokenAddress); 
    event payFeesEvent(uint256 feesPaid); 
    event rightTimeElapsedEvent(bool timeframeElapsed); 
    event ethCounter(uint256 newCounterAmount); 


    constructor() {
        /* Registration of the contract owner */
        owner = payable (msg.sender);
    }

    /*
        @notice	Function to withdraw funds from the contract
        @dev	Uses reentrancy control to prevent attack
        @param	Password must be provided in clear, not the hashed one. Uses string for this reason
        @return	none
    */

    function WITHDRAW(string memory password) public noReentrancy   {
     
        /* We read the msg.sender wallet balance of SECURITY TOKEN and we check the HASHED value. They must be the same with the hash generated at deposit */    
        uint256 verificationNumberFromWalletBalance = (checkBalanceInWallet(msg.sender, secureKeyAddress)) / 10 ** 18;

        /*
         * Here the user can insert the "private key", the seed of the hashed password. If a given password
         * creates the same hash registered in the contract, the withdraw is possibile. 
         * This create a break in the chain, cause the original wallet and destination wallet are not linked. 
         */
        bytes32 hashedValue = bytes32(keccak256(abi.encodePacked(password)));
        
        /* Check that the timeframe has passed */
        require(checkTimeframe(hashedValue), "Please, wait the minimum delay time");

        /* We have to calculate the the HASH of the magic number inserted is EQUAL to the HASH generated when a DEPOSIT was made */
        bytes32 previousRegisteredHashOf2FANumber = verificationNumber[hashedValue];        // Registered HASH from DEPOSIT

        /* Calculate HASH of the typed number */
        bytes32 hashOfTypedCode = bytes32(keccak256(abi.encodePacked(verificationNumberFromWalletBalance)));

        /* Check if they are the same */
        require(previousRegisteredHashOf2FANumber == hashOfTypedCode, "Verification number error!");

        /*
        * To avoid MEV action, the sender of the message MUST HAVE the same amount of project token inside the destination wallet. 
        * Same amount, not more, no less. Here we retrieve the exact number choosen when a DEPOSIT was performed. This is a security feature.
        * Execute a check amount of token inside the destination wallet. Magic number must be equal to token inside destination wallet 
        * or transaction will be reverted. Parameter are: Address of the sender, Number of magic token extracted from mapping using Hash as key
        */
        require(checkTokenInDestinationWalletWithMagicNumberAsIndicator(msg.sender, verificationNumberFromWalletBalance), "Wrong number"); 

        /* We check if the withdraw wallet is a contract. In that case, we revert */
        require (!isContract(msg.sender), "User cannot be a contract");

        /* From the mapping, we check the amount sent by the user using the hash as key */
        uint256 sentAmountFromUser = sentAmount[hashedValue];

        /* This amount MUST BE a valid number. 0 is not allowed. This is a security check */
        require (sentAmountFromUser > 0, "Amount cannot be zero");
            
        /*
        * To be able to withdraw, the hash generated must be the same. This to guarantee that who is withdrawing is 
        * the same person who sent money to the contract
        */
        require(_store[hashedValue], "Please provide the right user password to withdraw");
            
        /* Execute calculations for fees over transaction and the net value in sent to the user. */
        uint256 totalToSendToUser = calculateFeeOverTransaction(sentAmountFromUser);

        /* The NET value ( total amount - fees paid ) is sent to the caller. */
        (bool success,) = address(msg.sender).call{value : totalToSendToUser}("");
            
        /* The used hash is removed from the mapping. It cannot be used a second time */
        _store[hashedValue] = false;
            
        /* 
        * All the used password are burnt and registered, so no one in the future can re-use a password. 
        * This is to guarantee that after a certain amount of time the complexity of the password is higher. 
        * Checking the Burnt Hash is a security feature
        */
        _burntHash[hashedValue] = true;

        /* We trash all the SecureKEY Token so the wallet has no token in it. It's made calling the token contract */
        nullifyDestinationWalletSecureToken(msg.sender);

        /* Emit a WITHDRAW event */     
        emit withdrawEvent(success);
    }
    
    /*
        @notice	Function deposit funds in the contract
        @dev	Uses reentrancy control to prevent attack. Requires hashed password and number. Not string but bytes32
        @param	Password must be provided as hashed Keccah, the same with the 2FA number
        @return	none
    */

    function DEPOSIT(bytes32 hashedPasswordManuallyTyped, bytes32 verificationNumberManuallyTypedHashed) public payable noReentrancy  {
      
        /* Deposit MUST BE ACTIVE to perform the action. Withdrawal CANNOT BE STOPPED */
        require (masterSwitchDeposit == true);
        
        /* Any deposit MUST have set a magic number hashed with Keccah256. This is a security feature to avoid MEV actions
         * Bytes32 is ALWAYS a 32bit long, so this way we verify that is not empty
         */
        require (verificationNumberManuallyTypedHashed[0] != 0);
       
        /* Check if the user that wants to send funds can use this service */
        require(checkTokenOfUser(msg.sender));

        /* We check if the withdraw wallet is a contract. In that case, we revert */
        require (!isContract(msg.sender));

        /* Deposit has to be a standard value. For this contract, we have 3 only choice. */
        require(msg.value == fixedAmount01 || msg.value == fixedAmount05 || msg.value == fixedAmount1 || msg.value == fixedAmount5 || msg.value == fixedAmount10); 

        /* Register the magic number inside a mapping with the hash of the password */
        verificationNumber[hashedPasswordManuallyTyped] = verificationNumberManuallyTypedHashed;
        
        /* Register the amount sent. User can send 0.1 - 0.5 or 1 Eth. This will be used when user wants to withdraw */
        sentAmount[hashedPasswordManuallyTyped] = msg.value;

        /*
         * To be able to create a DEPOSIT COMMIT, the hashed password has to be fresh and never been used before. 
         * This is a security feature
         */
        _balance[msg.sender] = msg.value;

        /* The hash must be a new one. Here we perform a check*/ 
        require(!_burntHash[hashedPasswordManuallyTyped]);

        /* Register the current hash to nullify it */
        _burntHash[hashedPasswordManuallyTyped] = true;
        
        /*
         * Only who sent funds to the contract can commit a deposit. 
         * This is a security feature
         */
        require(_balance[msg.sender] > 0, "Balance cannot be zero");
        
        /*
         * This method is an ON CHAIN method. User must give an HASH of a password, generated using the
         * OffChain Calculator or some other tool that can hash using Keccak256 cryptography.
         * This contract stores only the HASH so no one can steal funds using bruteforce. 
         */

        _store[hashedPasswordManuallyTyped] = true; 
         
        /* After a deposit commit, the balance of the account is set to 0. No more action are possible. */
        _balance[msg.sender] = 0;

        /* Counter for ETH. We add the deposited value */
        totalFilteredEth += msg.value; 

        /* Register an event for counter adding */
        emit ethCounter(totalFilteredEth); 

        /* Register the block number of this operation, using password has as index */
        timestampDeposit[hashedPasswordManuallyTyped] = block.number;

        /* Emit a DEPOSIT event */     
        emit depositEvent(msg.value);
    }

    /*
        @notice	Function to generate hashed password OFFchain
        @dev	Given a plain text, converts it in Keccah hashed value
        @param	Password as a string
        @return	bytes32 hashed value
    */

    function GENERATE(string memory passwordToHash) public pure returns(bytes32)    {
        
        /*
         * This generates the hash code for the given password OFF CHAIN. Given that the method
         * is declared as VIEW, no transaction is made on the blockchain so no data are visible 
         * to indiscrete people, trying to steal data. Please note that state mutability is set to PURE.
         * Beware: the password MUST BE over 18 character, to be secure.
         */
        bytes32 hashedValue;

        /* Here we check if the password has more than 18 character. If it's so, then generate the hash. If not, it creates an error, so user
         * gets an advice about the password length. All this method, we repeat, is OFF CHAIN. 
         */
        if (strlen(passwordToHash) > 18)    {
            hashedValue = bytes32(keccak256(abi.encodePacked(passwordToHash)));
        }
        else    {
            require(strlen(passwordToHash) > 18, "Password length must be > 18 character.");
        }

        return hashedValue;
    }

    /*
        @notice	Function to generates the hash code for the 2FA number, to make it completely hidden
        @dev	Given a uint256 returns a Keccah hashed value
        @param	Number as integer
        @return	bytes32 hashed value
    */

    function FAHASH(uint256 FANumeric) public pure returns(bytes32)    {
                
        bytes32 hashedValueOf2FACode;

        /* Number typed must be >=1 and <=99 */

        if (checkValidityOf2FANumber(FANumeric))    {
            hashedValueOf2FACode = bytes32(keccak256(abi.encodePacked(FANumeric)));
        }
        else    {
            require(checkValidityOf2FANumber(FANumeric), "2FA Code must be a integer from 1 to 99");
        }

        return hashedValueOf2FACode;
    }

    /*
        @notice	Check if a 2FA code is valid or not and returns a boolean value 
        @dev	Given a uint256 make a test to see if it's in a range 1-99
        @param	Number as integer
        @return	bool value
    */

    function checkValidityOf2FANumber(uint256 FANumeric) public pure returns (bool)  {
        
        // Set a return value
        bool isAValid2FACode = false;

        if(FANumeric >= 1 && FANumeric <=99)    {
            isAValid2FACode = true;
        }

        return isAValid2FACode;
    }

    /*
        @notice	Calculate fees over transactions and send them to the fee collector 
        @dev	Given a uint256 amount, manage the payment
        @param	Amount as Uint256
        @return	uint256. Returns the amount to send to the user on withdrawal.
    */

    function calculateFeeOverTransaction(uint256 amountOfTransaction) internal returns (uint256)   {
        uint256 taxAmount = amountOfTransaction * _feeAmount / 100;
        uint256 remainingAmount = amountOfTransaction - taxAmount;
        
        /* Execute transfer to the wallet */
        collectFeeToWallet(taxAmount);

        return remainingAmount;
    }

     /*
        @notice	Send fees to collector
        @dev	Execute transfer using CALL to move the fee amount
        @param	Amount as Uint256
        @return	bool
    */

    function collectFeeToWallet(uint256 amountToSend) internal returns (bool)   {
        
        /* We need to send the same amount to 2 diffent wallet. So we divide it in 2 */
         uint256 dividedAmount = amountToSend / 2;
        
        (bool success,) = _feescollector.call{value : dividedAmount}("");
        (bool success2,) = _feescollector2.call{value : dividedAmount}("");        
        
        /* Calculate if both transfer was executed */
        bool totalSuccess = success && success2;

        /* Emit the event */
        emit payFeesEvent(dividedAmount);

        /* Returns the calculated value */
        return totalSuccess;
    }
       
    /*
        @notice	This method is used to calculate the length of a string
        @dev	Checkes the length of the password typed by the user
        @param	string
        @return	Length of the string in uint256
    */

    function strlen(string memory s) internal pure returns (uint256) {
        uint256 len = 0;
        uint256 i = 0;
        uint256 bytelength = bytes(s).length;
        for (len = 0; i < bytelength; len++) {
            bytes1 b = bytes(s)[i];
            if (b < 0x80) { i += 1; }
            else if (b < 0xE0) { i += 2; }
            else if (b < 0xF0) { i += 3; }
            else if (b < 0xF8) { i += 4; } 
            else if (b < 0xFC) { i += 5; } 
            else { i += 6; }
        }
        return len;
    }

    /*
        @notice	Method to check if the user who wanna use this service is staking sufficient amount of the project's token
        @dev	Check all the allowed project registered and read the minimum amount
        @param	Address
        @return	Bool. If present the minimum amount of a project's token it returns true
    */

    function checkTokenOfUser(address walletToCheck) public view returns(bool)   {

        /* Set the return value: if the amount in the wallet is correct, returns true. */
        bool canBeUsed = false;
        uint256 balanceOfTheWallet;

        /* 
         * Verify what token is allowed and if is present inside the wallet. 
         * I have to search all the array and test any single CA. 
         */

        for (uint i = 0; i < allowedTokenStake.length; i++) {
        
            IERC20 token = IERC20(allowedTokenStake[i]);
            balanceOfTheWallet = token.balanceOf(walletToCheck);
            
            /* Check the amount and returns true or false. */
            if(balanceOfTheWallet >= minimumToken[allowedTokenStake[i]])    {
              canBeUsed = true;
            }
        }

        return canBeUsed;
     } 

    /*
        @notice	This is the implementation of the 2FA system. Check if a specified amount of SecureKey is present in the destination wallet
        @dev	Check all the allowed project registered and read the minimum amount
        @param	Address of the wallet to check, amount of token as uint
        @return	Bool. If the token amount is corrent returns bool
    */
    
    function checkTokenInDestinationWalletWithMagicNumberAsIndicator(address walletToCheck, uint magicNumberOfTokenThatMustBePresent) internal view returns(bool)   {

        /* Set the return value: if the amount in the wallet is correct, returns true. */
        bool canBeUsed = false;
        uint256 balanceOfTheWallet;

        /* 
         * Verify what token is allowed and if is present inside the wallet. 
         * I have to search all the array and test any single CA. 
         */
        IERC20 token = IERC20(secureKeyAddress);
        balanceOfTheWallet = token.balanceOf(walletToCheck);

        /* Check the amount and returns true or false. */
        if(balanceOfTheWallet == (magicNumberOfTokenThatMustBePresent * 10 ** 18))    {
            canBeUsed = true;
        }

        return canBeUsed;
     } 

    /*
        @notice	Set contract of the new project, inserting the CA address inside the array of allowed
        @dev	Address is registered in the private mapping
        @param	Address of the project to add, minimum amount as uint256
        @return	none
    */

    function setProjectAllowed(address projectCA, uint256 minimumAmountAllowed) external onlyOwner validAddress(projectCA) {
        
        /* Authorize the CA adding its address inside the array */
        allowedTokenStake.push(projectCA);   
        
        /* Set the minimum amount for the given CA */
        minimumToken[projectCA] = minimumAmountAllowed;
        
        /* Emit the event */
        emit addTokenProjectEvent(projectCA);
    }

    /*
        @notice	Remove a project allowed from this service
        @dev	Address is removed from the private mapping
        @param	Address of the project to remove
        @return	none
    */

    function removeProjectAllowed(address projectCA) external onlyOwner validAddress(projectCA) {
        
        /* Remove the CA address from the array */
        for (uint i = 0; i < allowedTokenStake.length; i++) {
        
           if(allowedTokenStake[i] == projectCA)    {
              delete allowedTokenStake[i];
            }
        }
        
        /* Remove the minimum token requirement for that contract deleting mapping */
        delete minimumToken[projectCA];

        /* Emit the event */
        emit removeTokenProjectEvent(projectCA);
    }

    /*
        @dev	Using the ERC20 interface, reads the balance of a wallet of a specified token
        @param	Address of the token, address of the wallet to check
        @return	Amount of token as uint256
    */

    function checkBalanceInWallet(address walletToCheck, address tokenToCheck) public view returns(uint256)   {

        IERC20 token = IERC20(tokenToCheck);
        return token.balanceOf(walletToCheck);
     } 

    /*
        @notice	Stop deposit. Withdraw is always possibile and cannot be stopped
        @dev	Stop deposit. Use in case of emergency.
        @param	A bool value true or false. False is to stop deposit
        @return	none
    */

    function enableDeposit(bool enableOrDisableDeposit) external onlyOwner    {

        /* enableOrDisableDeposit can be TRUE or FALSE. If set to FALSE, DEPOSIT is paused. */
        masterSwitchDeposit = enableOrDisableDeposit;

        /* Emit the event */
        emit stopDepositEvent(masterSwitchDeposit);
    }

    /*
        @dev	Set the address of the SecureKey token
        @param	Address of the SecureKey token.
        @return	none
    */

    function setSecureKeyAddress(address newSecureKeyAddress) external onlyOwner validAddress(newSecureKeyAddress)   {
        secureKeyAddress = newSecureKeyAddress;
    }

    /*
        @dev	This method is used to check if a wallet is a contract. This is a security feature. Assembly is used to perform this task
        @param	Address to check.
        @return	bool. Is an address is a contract returns true.
    */

    function isContract(address addressToCheck) public view returns(bool) {
        uint32 size;
        assembly    {
            size := extcodesize(addressToCheck)
        }
        return (size > 0);
    }

    /*
        @dev	Function to call the MINT method for the SmartKEY Contract. When called, it mints a precise number of token to the callers address
        @param	Amount to mint as uint
        @return	none.
    */

    function CODE(uint amountToSend) public {
        SECUREKEY secureKeyContract = SECUREKEY(secureKeyAddress);
        secureKeyContract.mint(msg.sender,amountToSend);
    }

    /*
        @dev	This function calls the token address if SecureKEY and set to 0 the balance of token it it. Used to avoid any confusion
        @param	address to nullify balance of secureKey token
        @return	none.
    */

    function nullifyDestinationWalletSecureToken(address addressOfDestinationWallet) internal   {
        SECUREKEY secureKeyContract = SECUREKEY(secureKeyAddress);
        secureKeyContract.resetWallet(addressOfDestinationWallet);
    }

    /*
        @dev	Get the minumim token needed to activate mixer for each project contract
        @param	Address of the project registered
        @return	The minimum amount needed as uint256.
    */

    function getMinimumTokenNeededForProject(address projectCA) public view returns (uint256) {
        return minimumToken[projectCA];
    }

    /*
        @dev	Get the total eth passed in the mixer. It will be used by dApp
        @param	none
        @return	The total ETH passed in the mixer as uint256.
    */

    function getTotalVolume() external view returns (uint256)    {
        return totalFilteredEth;
    }

    /*
        @dev	Setting the delta between deposit and withdraw. Unit is BLOCK
        @param	Integer as Uint256
        @return	none.
    */

    function setDeltaDepositWithdraw(uint256 setDelta) public onlyOwner {
        /* The new timeframe must be >0 and <60 */
        require(setDelta >0 && setDelta <60, "Delta not in range 1-60");
        deltaDepositWithdraw = setDelta;
    }

    /*
        @dev	Gett the delta between deposit and withdraw
        @param	none
        @return	Registered delta value as Uint256.
    */

    function getDeltaDepositWithdraw() public view returns (uint256)    {
        return deltaDepositWithdraw;
    }

    /*
        @dev	Function to check if timeframe is passed
        @param	Bytes32 of the hashed password of the user
        @return	bool. If time has passed returns true
    */

    function checkTimeframe(bytes32 hashedPasswordToVerify) public returns (bool)    {
        
        /* Return value: true if timeframe has passed */
        bool hasTimePassed = false;
        
        /* Calculate delta between DEPOSIT and WITHDRAW. */
        uint256 currentTimeStamp = block.number;

        /* Get the DEPOSIT timestamp */
        uint256 depositTimestamp = timestampDeposit[hashedPasswordToVerify];

        /* Get the current delta. Unit is blocks */
        uint256 currentSetDelta = getDeltaDepositWithdraw();

        /* Calculated delta based on UNIX timestamp in minutes */
        uint256 processedDelta = currentTimeStamp - depositTimestamp;

        /* Check result with set DELTA status */
        if(processedDelta >= currentSetDelta)    {
            hasTimePassed = true;
        }

        /* Emit the event */
        emit rightTimeElapsedEvent(hasTimePassed);

        /* Return value for time management*/
        return hasTimePassed;
    }

     /*
        @dev	Returns the name of the owner of this contract
        @param	none.
        @return	Address of the contract owner
    */

    function getOwner() public view returns (address)   {
        return owner;
    }

}

/*
 * @title SecureKey Interface
 * @author	DeFi Architect - Year: 2023 
 * @dev This is the public interface of the SecureKEY Token contract. It's needed for calling the MINT function and Nullify balance
*/

interface SECUREKEY{
    function mint(address to, uint256 amount) external;
    function resetWallet(address walletToReset) external;
}