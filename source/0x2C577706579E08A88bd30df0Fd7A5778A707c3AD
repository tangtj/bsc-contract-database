// File: iSYNTHVAULT.sol


pragma solidity 0.8.3;
interface iSYNTHVAULT{
   function depositForMember(address synth, address member) external;
   function setReserveClaim(uint256 _setSynthClaim) external;
}
// File: iSYNTHFACTORY.sol


pragma solidity 0.8.3;
interface iSYNTHFACTORY {
    function isSynth(address) external view returns (bool);
    function getSynth(address) external view returns (address);
    function removeSynth(address _token) external;
    function synthCount() external returns(uint);
}
// File: iSYNTH.sol


pragma solidity 0.8.3;
interface iSYNTH {
    function genesis() external view returns(uint);
    function TOKEN() external view returns(address);
    function POOL() external view returns(address);
    function mintSynth(address, uint) external returns(uint256);
    function burnSynth(uint) external returns(uint);
    function realise() external;
}

// File: iROUTER.sol


pragma solidity 0.8.3;
interface iROUTER {
    function addLiquidityForMember(uint, uint, address, address) external payable returns (uint);
      function synthMinting() external view returns (bool);
}
// File: iDAOVAULT.sol


pragma solidity 0.8.3;
interface iDAOVAULT{
  function getMemberWeight(address) external view returns (uint256);
  function getMemberPoolBalance(address, address) external view returns(uint);
  function getMemberLPWeight(address) external view returns(uint, uint);
  function depositLP(address, uint, address) external;
  function withdraw(address, address) external returns (bool);
  function totalWeight() external view returns (uint);
  function mapTotalPool_balance(address) external view returns (uint);
}
// File: iBASE.sol


pragma solidity 0.8.3;

interface iBASE {
    function DAO() external view returns (iDAO);
    function secondsPerEra() external view returns (uint256);
    function changeDAO(address) external;
    function setParams(uint256, uint256) external;
    function flipEmissions() external;
    function mintFromDAO(uint256, address) external; 
    function burn(uint256) external; 
}
// File: iUTILS.sol

//SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.3;
interface iUTILS {
    function calcShare(uint, uint, uint) external pure returns (uint);
    function getFeeOnTransfer(uint256, uint256) external view returns(uint);
    function getPoolShareWeight(address, uint)external view returns(uint);
    function calcLiquidityUnits(uint, uint, uint, uint, uint) external pure returns (uint);
    function calcLiquidityHoldings(uint, address, address) external pure returns (uint);
    function calcSwapOutput(uint, uint, uint) external pure returns (uint);
    function calcSwapFee(uint, uint, uint) external pure returns (uint);
    function calcSwapValueInBase(address, uint) external view returns (uint);
    function calcSwapValueInToken(address, uint) external view returns (uint);
    function calcSpotValueInBaseWithSynth(address, uint) external view returns (uint);
    function calcSpotValueInBase(address, uint) external view returns (uint);
    function calcPart(uint, uint) external pure returns (uint);
    function calcLiquidityUnitsAsym(uint, address)external pure returns (uint);
    function calcActualSynthUnits(address, uint) external view returns (uint);
}
// File: iBEP20.sol


pragma solidity 0.8.3;
interface iBEP20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address) external view returns (uint256);
    function transfer(address, uint256) external returns (bool);
    function allowance(address, address) external view returns (uint256);
    function approve(address, uint256) external returns (bool);
    function transferFrom(address, address, uint256) external returns (bool);
    function burn(uint) external;
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: Pool.sol


pragma solidity 0.8.3;










contract Pool is iBEP20 {  
    address public immutable BASE;  // Address of SPARTA base token contract
    address public immutable TOKEN; // Address of the layer1 TOKEN represented in this pool
    uint256 public synthCap;    // Basis points hard cap of synths that can be minted vs tokenDepth
    uint256 public baseCap;     // Cap on the depth of the pool (in SPARTA)
    uint256 public oldRate;    // Pool ratio from last period
    uint256 private period;     // Timestamp of next period
    uint256 private freezePoint; // Basis points change to trigger a freeze
    bool public freeze;         // Freeze status of the pool
    uint256 private oneWeek;

    uint256 public stirRate; // Rate of steaming
    uint public lastStirred; // timestamp of last steamed
    uint public stirStamp; // timestamp of last time stir rate was adjusted
    bytes4 private constant SELECTOR = bytes4(keccak256(bytes('transfer(address,uint256)')));

    string private _name;
    string private _symbol;
    uint8 public override immutable decimals;
    uint256 public override totalSupply;
    mapping(address => uint) private _balances;
    mapping(address => mapping(address => uint)) private _allowances;

    uint256 public baseAmount;  // SPARTA amount that should be in the pool
    uint256 public tokenAmount; // TOKEN amount that should be in the pool

    uint private lastMonth;          // Timestamp of the start of current metric period (For UI)
    uint public immutable genesis;  // Timestamp from when the pool was first deployed (For UI)

    uint256 public map30DPoolRevenue;       // Tally of revenue during current incomplete metric period (for UI)
    uint256 public mapPast30DPoolRevenue;   // Tally of revenue from last full metric period (for UI)
    uint256 [] private revenueArray;         // Array of the last two full metric periods (For UI)
    
    event AddLiquidity(address indexed member, address indexed tokenAddress, uint inputBase, uint inputToken, uint unitsIssued);
    event RemoveLiquidity(address indexed member, address indexed tokenAddress, uint outputBase, uint outputToken, uint unitsClaimed);
    event Swapped(address indexed tokenFrom, address indexed tokenTo, address indexed recipient, uint inputAmount, uint outputAmount, uint fee);
    event MintSynth(address indexed member, address indexed synthAddress, uint256 baseAmount, uint256 liqUnits, uint256 synthAmount);
    event BurnSynth(address indexed member, address indexed synthAddress, uint256 baseAmount, uint256 liqUnits, uint256 synthAmount);

    function SYNTH() public view returns(address) {
        return iSYNTHFACTORY(_DAO().SYNTHFACTORY()).getSynth(TOKEN); // Get the relevant synth address
    }

    function _DAO() internal view returns(iDAO) {
        return iBASE(BASE).DAO(); // Get the DAO address reported by Sparta base contract
    }

    // Restrict access
    modifier onlyPROTOCOL() {
        require(msg.sender == _DAO().ROUTER() || msg.sender == _DAO().SYNTHVAULT() || msg.sender == _DAO().POOLFACTORY()); 
        _;
    }
    modifier onlyDAO() {
        require(msg.sender == _DAO().DAO() || msg.sender == _DAO().ROUTER() || msg.sender == _DAO().SYNTHVAULT());
        _;
    }
     modifier onlySYNTH() {
        require(msg.sender == SYNTH());
        require(iSYNTHFACTORY(_DAO().SYNTHFACTORY()).isSynth(SYNTH()));
        _;
    }

    constructor (address _base, address _token) {
        BASE = _base;
        TOKEN = _token;
        string memory poolName = "-SpartanProtocolPool";
        string memory poolSymbol = "-SPP";
        _name = string(abi.encodePacked(iBEP20(_token).name(), poolName));
        _symbol = string(abi.encodePacked(iBEP20(_token).symbol(), poolSymbol));
        decimals = 18;
        genesis = block.timestamp;
        period = block.timestamp + 60; 
        lastMonth = block.timestamp;
        synthCap = 2500; //25%
        freezePoint = 3000; //30%
        baseCap = 100000*10**18; //RAISE THE CAPS
        lastStirred = 0;
        oneWeek = 604800;//604800 mainnet
    }

    //========================================iBEP20=========================================//

    function name() external view override returns (string memory) {
        return _name;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function transfer(address recipient, uint256 amount) external virtual override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) external virtual override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) external virtual returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) external virtual returns (bool) {
        uint256 currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "!approval");
        _approve(msg.sender, spender, currentAllowance - subtractedValue);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "!owner");
        require(spender != address(0), "!spender");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) external virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "!approval");
        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "!sender");
        require(recipient != address(0), '!BURN');
        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "!balance");
        _balances[sender] = senderBalance - amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "!account");
        totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function burn(uint256 amount) external onlySYNTH virtual override {
        _burn(msg.sender, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "!account");
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "!balance");
        _balances[account] = accountBalance - amount;
        totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    }

    //====================================POOL FUNCTIONS =================================//

    // Contract adds liquidity for user 
    function addForMember(address member) external onlyPROTOCOL returns (uint liquidityUnits){
        uint256 _actualInputBase = _getAddedBaseAmount(); // Get the received SPARTA amount
        uint256 _actualInputToken = _getAddedTokenAmount(); // Get the received TOKEN amount
        liquidityUnits = iUTILS(_DAO().UTILS()).calcLiquidityUnits(_actualInputBase, baseAmount, _actualInputToken, tokenAmount, totalSupply); // Calculate LP tokens to mint
        if(baseAmount == 0 || tokenAmount == 0){
            uint createFee = 100 * liquidityUnits / 10000; // First liqAdd charges 1% fee to the base contract (permanently removed from circulation; prevent 1wei rounding)
            liquidityUnits -= createFee; // Remove fee from the calculated LP units
            _mint(BASE, createFee); // Mint the fee portion of the LP units to the BASE contract (permanently removed from circulation)
            oldRate = (10**18 * _actualInputBase) / _actualInputToken; // Set the first / initial rate
        }
        _incrementPoolBalances(_actualInputBase, _actualInputToken); // Update recorded BASE and TOKEN amounts
        _mint(member, liquidityUnits); // Mint the remaining LP tokens directly to the user
        emit AddLiquidity(member, TOKEN, _actualInputBase, _actualInputToken, liquidityUnits);
        return liquidityUnits;
    }

    // Contract removes liquidity for the user
    function removeForMember(address recipient, address actualMember) external onlyPROTOCOL returns (uint outputBase, uint outputToken) {
        require(block.timestamp > (genesis + oneWeek)); // Can not remove liquidity until 7 days after pool's creation
        uint256 _actualInputUnits = balanceOf(address(this)); // Get the received LP units amount
        iUTILS _utils = iUTILS(_DAO().UTILS()); // Interface the UTILS contract
        outputBase = _utils.calcLiquidityHoldings(_actualInputUnits, BASE, address(this)); // Get the SPARTA value of LP units
        outputToken = _utils.calcLiquidityHoldings(_actualInputUnits, TOKEN, address(this)); // Get the TOKEN value of LP units
        _decrementPoolBalances(outputBase, outputToken); // Update recorded SPARTA and TOKEN amounts
        _burn(address(this), _actualInputUnits); // Burn the LP tokens
        _safeTransfer(BASE, recipient,outputBase);        
        _safeTransfer(TOKEN, recipient,outputToken);
        emit RemoveLiquidity(actualMember, TOKEN, outputBase, outputToken, _actualInputUnits);
        return (outputBase, outputToken);
    }

    // Contract swaps tokens for the member
    function swapTo(address token, address member) external onlyPROTOCOL returns (uint outputAmount, uint fee) {
        require((token == BASE || token == TOKEN)); // Must be SPARTA or the pool's relevant TOKEN
        address _fromToken; uint _amount;
        if(token == BASE){
            _fromToken = TOKEN; // If SPARTA is selected; swap from TOKEN
            _amount = _getAddedTokenAmount(); // Get the received TOKEN amount
            (outputAmount, fee) = _swapTokenToBase(_amount); // Calculate the SPARTA output from the swap
        } else {
            _fromToken = BASE; // If TOKEN is selected; swap from SPARTA
            _amount = _getAddedBaseAmount(); // Get the received SPARTA amount
            (outputAmount, fee) = _swapBaseToToken(_amount); // Calculate the TOKEN output from the swap
        }
        emit Swapped(_fromToken, token, member, _amount, outputAmount, fee);
        _safeTransfer(token, member,outputAmount);  
        return (outputAmount, fee);
    }

    // Swap SPARTA for Synths
    function mintSynth(address member) external onlyPROTOCOL returns(uint outputAmount, uint fee) {
        address synthOut = SYNTH(); // Get the synth address
        require(iROUTER(_DAO().ROUTER()).synthMinting());  
        require(iSYNTHFACTORY(_DAO().SYNTHFACTORY()).isSynth(synthOut)); // Must be a valid Synth
        iUTILS _utils = iUTILS(_DAO().UTILS()); // Interface the UTILS contract
        uint256 _actualInputBase = _getAddedBaseAmount(); // Get received SPARTA amount
        uint256 steamedSynths = stirCauldron(synthOut);
        uint256 output = _utils.calcSwapOutput(_actualInputBase, baseAmount, tokenAmount); // Calculate value of swapping SPARTA to the relevant underlying TOKEN (virtualised)
        lastStirred = block.timestamp;
        outputAmount = output * 9900 / 10000; //1% fee reduction
        require(outputAmount <= steamedSynths); //steam synths
        uint _liquidityUnits = _utils.calcLiquidityUnitsAsym(_actualInputBase, address(this)); // Calculate LP tokens to be minted
        _incrementPoolBalances(_actualInputBase, 0); // Update recorded SPARTA amount
        uint _fee = _utils.calcSwapFee(_actualInputBase, baseAmount, tokenAmount); // Calc slip fee in TOKEN (virtualised)
        fee = _utils.calcSpotValueInBase(TOKEN, _fee); // Convert TOKEN fee to SPARTA
        _mint(synthOut, _liquidityUnits); // Mint the LP tokens directly to the Synth contract to hold
        iSYNTH(synthOut).mintSynth(address(_DAO().SYNTHVAULT()), outputAmount); // Mint the Synth tokens directly to the synthVault
        iSYNTHVAULT(_DAO().SYNTHVAULT()).depositForMember(synthOut, member); //Deposit on behalf of member
        _addPoolMetrics(fee); // Add slip fee to the revenue metrics
        emit MintSynth(member,synthOut, _actualInputBase, _liquidityUnits, outputAmount);
        return (outputAmount, fee);
    }
    
    // Swap Synths for SPARTA
    function burnSynth(address recipient, address actualMember) external onlyPROTOCOL returns(uint outputAmount, uint fee) {
        address synthIN = SYNTH(); // Get the synth address
        require(synthIN != address(0)); // Must be a valid Synth
        iUTILS _utils = iUTILS(_DAO().UTILS()); // Interface the UTILS contract
        uint _actualInputSynth = iBEP20(synthIN).balanceOf(address(this)); // Get received SYNTH amount
        uint output = _utils.calcSwapOutput(_actualInputSynth, tokenAmount, baseAmount); // Calculate value of swapping relevant underlying TOKEN to SPARTA (virtualised)
        uint outputBase = output * 9500 / 10000; //5% reduction
        fee = _utils.calcSwapFee(_actualInputSynth, tokenAmount, baseAmount); // Calc slip fee in SPARTA (virtualised)
        _decrementPoolBalances(outputBase, 0); // Update recorded SPARTA amount
        _addPoolMetrics(fee); // Add slip fee to the revenue metrics
        uint liqUnits = iSYNTH(synthIN).burnSynth(_actualInputSynth); // Burn the input SYNTH units 
        _burn(synthIN, liqUnits); // Burn the synth-held LP units
        _safeTransfer(BASE, recipient,outputBase);  
        emit BurnSynth(actualMember,synthIN, outputBase, liqUnits, _actualInputSynth);
        return (outputBase, fee);
    }

    function stirCauldron(address synth) public returns (uint256 steamedSynths){ 
          uint256 synthsCap = tokenAmount * synthCap / 10000;
          uint256 liquidSynths; uint256 totalSup = iBEP20(synth).totalSupply();
          steamedSynths = 0;
         if(synthsCap >= totalSup){
             liquidSynths = synthsCap - totalSup; 
         }
         if(lastStirred != 0){ 
            uint secondsSinceStirred = block.timestamp - lastStirred; //Get last time since stirred
            steamedSynths = secondsSinceStirred * stirRate; //time since last minted
         }else{
            lastStirred = block.timestamp;
            stirStamp = block.timestamp;
            stirRate = liquidSynths / oneWeek;
            steamedSynths = 14400 * stirRate;  //4hrs
         }
         if(block.timestamp > (stirStamp + oneWeek)){
            stirRate = liquidSynths / oneWeek;
            stirStamp = block.timestamp;
         }
    }

    //=======================================INTERNAL MATHS======================================//

    // Check the SPARTA amount received by this Pool
    function _getAddedBaseAmount() internal view returns(uint256 _actual){
        uint _baseBalance = iBEP20(BASE).balanceOf(address(this));
        if(_baseBalance > baseAmount){
            _actual = _baseBalance - baseAmount;
        } else {
            _actual = 0;
        }
        return _actual;
    }
  
    // Check the TOKEN amount received by this Pool
    function _getAddedTokenAmount() internal view returns(uint256 _actual){
        uint _tokenBalance = iBEP20(TOKEN).balanceOf(address(this)); 
        if(_tokenBalance > tokenAmount){
            _actual = _tokenBalance - tokenAmount;
        } else {
            _actual = 0;
        }
        return _actual;
    }

    // Calculate output of swapping SPARTA for TOKEN & update recorded amounts
    function _swapBaseToToken(uint256 _x) internal returns (uint256 _y, uint256 _fee){
        uint256 _X = baseAmount;
        uint256 _Y = tokenAmount;
        iUTILS _utils = iUTILS(_DAO().UTILS());
        _y =  _utils.calcSwapOutput(_x, _X, _Y); // Calc TOKEN output 
        uint fee = _utils.calcSwapFee(_x, _X, _Y); // Calc TOKEN fee 
        _fee = _utils.calcSpotValueInBase(TOKEN, fee); // Convert TOKEN fee to SPARTA
        _setPoolAmounts(_X + _x, _Y - _y); // Update recorded BASE and TOKEN amounts
        _addPoolMetrics(_fee); // Add slip fee to the revenue metrics
        return (_y, _fee);
    }

    // Calculate output of swapping TOKEN for SPARTA & update recorded amounts
    function _swapTokenToBase(uint256 _x) internal returns (uint256 _y, uint256 _fee){
        uint256 _X = tokenAmount;
        uint256 _Y = baseAmount;
        iUTILS _utils = iUTILS(_DAO().UTILS());
        _y = _utils.calcSwapOutput(_x, _X, _Y); // Calc SPARTA output 
        _fee = _utils.calcSwapFee(_x, _X, _Y); // Calc SPARTA fee 
        _setPoolAmounts(_Y - _y, _X + _x); // Update recorded BASE and TOKEN amounts
        _addPoolMetrics(_fee); // Add slip fee to the revenue metrics
        return (_y, _fee);
    }

    //=======================================BALANCES=========================================//

    // Sync internal balances to actual
    function sync() external onlyDAO  {
        baseAmount = iBEP20(BASE).balanceOf(address(this));
        tokenAmount = iBEP20(TOKEN).balanceOf(address(this));
    }

    // Set internal balances
    function _setPoolAmounts(uint256 _baseAmount, uint256 _tokenAmount) internal {
        baseAmount = _baseAmount;
        tokenAmount = _tokenAmount; 
        _safetyCheck();
    }

    // Increment internal balances
    function _incrementPoolBalances(uint _baseAmount, uint _tokenAmount) internal {
        require((baseAmount + _baseAmount) < baseCap); // SPARTA input must not push TVL over the cap
        baseAmount += _baseAmount;
        tokenAmount += _tokenAmount;
        _safetyCheck();
    }

    // Decrement internal balances
    function _decrementPoolBalances(uint _baseAmount, uint _tokenAmount) internal {
        baseAmount -= _baseAmount;
        tokenAmount -= _tokenAmount; 
        _safetyCheck();
    }

    function _safeTransfer(address token, address to, uint value) private {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(SELECTOR, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TRANSFER_FAILED');
    }

    function _safetyCheck() internal {
            uint currentRate = (10**18 * baseAmount) / tokenAmount; // Get current rate
            uint rateDiff; uint256 rateDiffBP;
            if (currentRate > oldRate) {
                rateDiff = currentRate - oldRate; // Get absolute rate diff
                rateDiffBP = rateDiff * 10000 / currentRate; // Get basispoints difference
            } else {
                rateDiff = oldRate - currentRate; // Get absolute rate diff
                rateDiffBP = rateDiff * 10000 / oldRate; // Get basispoints difference
            }
            if (rateDiffBP >= freezePoint) {
                freeze = true; // If exceeding; flip freeze to true
            } else if (block.timestamp > period) {
                period = block.timestamp + 3600; // Set new period
                oldRate = currentRate; // Update the stored ratio
            }
            if(freeze){
                if(rateDiffBP <= freezePoint){
                    freeze = false; // If exceeding; flip freeze to false
                }
                if (block.timestamp > period) {
                    period = block.timestamp + 3600; // Set new period
                    oldRate = (currentRate + oldRate ) / 2; //Smooth rate increase
                }
            }  
    }
  
    //===========================================POOL FEE ROI=================================//

    function _addPoolMetrics(uint256 _fee) internal {
        if (block.timestamp <= (lastMonth + 2592000)) {
            map30DPoolRevenue = map30DPoolRevenue + _fee;
        } else {
            lastMonth = block.timestamp;
            mapPast30DPoolRevenue = map30DPoolRevenue;
            _archiveRevenue(mapPast30DPoolRevenue);
            map30DPoolRevenue = _fee;
        }
    }

    function _archiveRevenue(uint _totalRev) internal {
        if (revenueArray.length == 2) {
            revenueArray[0] = revenueArray[1]; // Shift previous value to start of array
            revenueArray[1] = _totalRev; // Replace end of array with new value
        } else {
            revenueArray.push(_totalRev);
        }
    }

    //=========================================== SYNTH CAPS =================================//
    
    function setSynthCap(uint256 _synthCap) external onlyPROTOCOL {
        require(_synthCap <= 3000);
        synthCap = _synthCap;
    }

    function RTC(uint256 _newRTC) external onlyPROTOCOL {
        require(_newRTC <= (baseCap * 2));
        baseCap = _newRTC;
    }

    function setFreezePoint(uint256 _newFreezePoint) external onlyPROTOCOL {
        freezePoint = _newFreezePoint;
    }
}

// File: iBONDVAULT.sol


pragma solidity 0.8.3;
interface iBONDVAULT{
  function depositForMember(address asset, address member, uint liquidityUnits) external;
  function claimForMember(address listedAsset, address member) external;
  function calcBondedLP(address bondedMember, address asset) external view returns(uint);
  function getMemberPoolBalance(address, address) external view returns (uint256);
  function totalWeight() external view returns (uint);
  function isListed(address) external view returns (bool);
  function listBondAsset(address) external;
  function delistBondAsset(address) external;
  function claim(address, address) external;
  function getMemberLPWeight( address) external view returns(uint, uint);
  function mapTotalPool_balance(address) external view returns (uint);
}
// File: iDAO.sol


pragma solidity 0.8.3;
interface iDAO {
    function ROUTER() external view returns(address);
    function BASE() external view returns(address);
    function LEND() external view returns(address);
    function UTILS() external view returns(address);
    function DAO() external view returns (address);
    function RESERVE() external view returns(address);
    function SYNTHVAULT() external view returns(address);
    function BONDVAULT() external view returns(address);
    function SYNTHFACTORY() external view returns(address);
    function POOLFACTORY() external view returns(address);
    function depositForMember(address pool, uint256 amount, address member) external;
    function currentProposal() external view returns (uint);
    function mapPID_open(uint) external view returns (bool);
    function isListed(address) external view returns (bool);
}
// File: poolFactory.sol


pragma solidity 0.8.3;




contract PoolFactory  { 
    address private immutable BASE;  // Address of SPARTA base token contract
    address private immutable WBNB;  // Address of WBNB
    address private DEPLOYER;        // Address that deployed the contract | can be purged to address(0)
    uint private curatedPoolSize;    // Max amount of pools that can be curated status
    uint public curatedPoolCount;   // Current count of pools that are curated status
    address[] private arrayPools;    // Array of all deployed pools
    address[] private arrayTokens;   // Array of all listed tokens
    address[] private vaultAssets;   // Array of all vault-enabled assets (curated pools)
    uint256 private minBASE;

    mapping(address=>address) private mapToken_Pool;
    mapping(address=>bool) public isPool;
    mapping(address=>bool) public isCuratedPool;
 
    event CreatePool(address indexed token, address indexed pool);
    event AddCuratePool(address indexed pool);
    event RemoveCuratePool(address indexed pool);

    // Restrict access
    modifier onlyDAO() {
        require(msg.sender == DEPLOYER || msg.sender == _DAO().DAO());
        _;
    }

    constructor (address _base, address _wbnb) {
        BASE = _base;
        WBNB = _wbnb;
        curatedPoolSize = 10;
        minBASE = 1000*10**18; //default
        DEPLOYER = msg.sender;
    }

    function _DAO() internal view returns(iDAO) {
        return iBASE(BASE).DAO();
    }

    // Can purge deployer once DAO is stable and final
    function purgeDeployer() external onlyDAO {
        DEPLOYER = address(0);
    }

    //Set Curated Pool Size
    function setParams(uint256 newSize, uint256 _minBASE) external onlyDAO {
        require(newSize > 0);
        curatedPoolSize = newSize;
        minBASE = _minBASE;
    }

    // Anyone can create a pool and add liquidity at the same time
    function createPoolADD(uint256 inputBase, uint256 inputToken, address token) external payable returns(address pool){
        require(token != BASE); // Token must not be SPARTA
        require(getPool(token) == address(0)); // Must not have a valid pool address yet
        require((inputToken > 100000 && inputBase >= minBASE)); // User must add at least 10,000 SPARTA liquidity & ratio must be finite
        Pool newPool; address _token = token;
        if(token == address(0)){
            _token = WBNB; // Handle BNB -> WBNB
        } else {
            require(token != BASE && iBEP20(token).decimals() == 18); // Token must not be SPARTA & it's decimals must be 18
        }
        newPool = new Pool(BASE, _token); // Deploy new pool
        pool = address(newPool); // Get address of new pool
        mapToken_Pool[_token] = pool; // Record the new pool address in PoolFactory
        _handleTransferIn(BASE, inputBase, pool); // Transfer SPARTA liquidity to new pool
        _handleTransferIn(token, inputToken, pool); // Transfer TOKEN / BNB (not WBNB) liquidity to new pool
        arrayPools.push(pool); // Add pool address to the pool array
        arrayTokens.push(_token); // Add token to the listed array
        isPool[pool] = true; // Record pool as currently listed
        emit CreatePool(_token, pool); // Emit CreatePool before the AddLiquidity event for subgraph
        Pool(pool).addForMember(msg.sender); // Perform the liquidity-add for the user
        return pool;
    }

    // Add pool to the Curated list, enabling it's synths, dividends & dao/vault weight
    function addCuratedPool(address token) external onlyDAO {
        require(token != BASE); // Token must not be SPARTA
        uint _currentProposal = iDAO(_DAO().DAO()).currentProposal(); // Get current proposal ID
        require(iDAO(_DAO().DAO()).mapPID_open(_currentProposal) == false); // Must not be an open proposal (de-syncs proposal votes)
        address _pool = getPool(token); // Get pool address
        require(isPool[_pool] == true); // Pool must be valid
        require(isCuratedPool[_pool] == false); // Pool must not be curated yet
        require(curatedPoolCount < curatedPoolSize); // Must be room in the Curated list
        isCuratedPool[_pool] = true; // Record pool as Curated
        curatedPoolCount = curatedPoolCount + 1; // Increase the curated pool count
        vaultAssets.push(_pool); // Push pool address into the curated array
        emit AddCuratePool(_pool);
    }

    // Remove pool from the Curated list
    function removeCuratedPool(address token) external onlyDAO {
        require(token != BASE); // Token must not be SPARTA
        address _pool = getPool(token); // Get pool address
        require(iBONDVAULT(_DAO().BONDVAULT()).isListed(_pool) == false);   
        require(isCuratedPool[_pool] == true); // Pool must be Curated
        isCuratedPool[_pool] = false; // Record pool as not curated
        curatedPoolCount = curatedPoolCount - 1; // Decrease the curated pool count
        for(uint i = 0 ; i < vaultAssets.length; i++){
            if(vaultAssets[i] == _pool){
                vaultAssets[i] = vaultAssets[vaultAssets.length - 1]; // Move the last element into the place to delete
                vaultAssets.pop(); // Remove the last element
            }
        }
        iSYNTHFACTORY(_DAO().SYNTHFACTORY()).removeSynth(token); // Remove synth from 'isSynth'
        emit RemoveCuratePool(_pool);
    }

    function _handleTransferIn(address _token, uint256 _amount, address _pool) internal  {
        require(_amount > 0);
        if(_token == address(0)){
            require(_amount == msg.value);
            (bool success, ) = payable(WBNB).call{value: _amount}(""); // Wrap BNB
            require(success);
            iBEP20(WBNB).transfer(_pool, _amount); // Tsf WBNB (PoolFactory -> Pool)
        } else {
            require(iBEP20(_token).transferFrom(msg.sender, _pool, _amount)); // Tsf TOKEN (User -> Pool)
        }
    }
    function getPool(address token) public view returns(address pool){
        if(token == address(0)){
            pool = mapToken_Pool[WBNB];   // Handle BNB
        } else {
            pool = mapToken_Pool[token];  // Handle normal token
        } 
        return pool;
    }
    function getVaultAssets() external view returns(address [] memory assets){
        return vaultAssets;
    }
    function getPoolAssets() external view returns(address [] memory pools){
        return arrayPools;
    }
    function getTokenAssets() external view returns(address [] memory tokens){
        return arrayTokens;
    }
}