/**
 *Submitted for verification at BscScan.com on 2023-07-07
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-20
*/

/**
 *Submitted for verification at BscScan.com on 2023-05-19
*/
// SPDX-License-Identifier: UNLICENSED

pragma solidity ^0.8.19;
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

interface IAlphaNFT{
    function getLines(address _addr) external view returns(uint);
}

interface IERC721 {
    function balanceOf(address owner) external view returns (uint256 balance);
    function ownerOf(uint256 tokenId) external view returns (address owner);
}

interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the token decimals.
     */
    function decimals() external view returns (uint8);

    /**
     * @dev Returns the token symbol.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the token name.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the bep token owner.
     */
    function getOwner() external view returns (address);

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
    function allowance(address _owner, address spender) external view returns (uint256);

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

// File: @openzeppelin\contracts\utils\Address.sol
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
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
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

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain`call` is an unsafe replacement for a function call: use this
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
    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
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
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: value }(data);
        return _verifyCallResult(success, returndata, errorMessage);
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
    function functionStaticCall(address target, bytes memory data, string memory errorMessage) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
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
    function functionDelegateCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(bool success, bytes memory returndata, string memory errorMessage) private pure returns(bytes memory) {
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


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}


abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor (){
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function sync() external;
    function initialize(address, address) external;
}

// pragma solidity >=0.6.2;

interface IUniswapV2Factory {
  function getPair(address token0, address token1) external returns (address);
}

interface IUniswapV2Router01 {
function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    
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

    constructor () {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}


contract VContract is Ownable, ReentrancyGuard{
    //using SafeMath for uint256;
    //using SafeBEP20 for IERC20;
    IERC20 public token;
    IERC20 public BUSD;
    IERC721 public NFT;
    //contract information
    string private _name = "Testing VAULT";
    string private _symbol = "TestV";
    uint8 private _decimals = 18;
    uint public rewardTimeInMinute = 5;
    uint public roundRobinMax = 15;

    //fees settings
    uint256 referralFee = 50;
    uint256 totalDepositFee = 10;
    uint256 totalSellFee = 12;
    uint256 totalCompoundFee = 10;
    uint256 totalWithdrawFee = 12;
    uint256 totalAirdropFee = 10;

    
    //fee address
    address public vaultGasAddress = 0xCa5C49f3C20D187BdC3D1EC16ce99b76CaFf880b;//alpha Sigma multisig
    address public dasAddress= 0x9224CAe52E0d36cd5875947d148a60443E70E56A;//Decentralize Account System Wallet
    address public sigmaAddress=0xF423860A73FD17DeF9a3029F84a79E7b226121a7;//Team multisigWallet //0xF423860A73FD17DeF9a3029F84a79E7b226121a7//0x319366e8d08b9DA18ac7103bb8cbC722C066335b
    address public rewardPoolAddress = 0xca6D3afAa080fE160b9Fe1791cd2470306dabF51; //reward nft Wallet
    address public liqWallet=  0xd99996fcA63F823b165a1Db4Ec0d67Dcce64942F; // liquidity wallet
    address public topWallet = 0x70092C16193c37152b279c862923f192c0c35eaB; // top wallet
    //deposit and withdraw variable settings
    uint256 public defaultROI = 150;
    uint256 vaultMax = 10;
    uint256 jailLimit = 10;
    uint256 maxSell = 250 ether;
    uint256 maxBuy = 1000 ether;
    uint256 minBuy = 10 ether;
    uint256 vGasPrice =  25 * 10 ** 16;

    uint256 _investmentCount; //counting the number of time investment occur
    IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E); // mainnet 0x10ED43C718714eb63d5aA57B78B54704E256024E//0xD99D1c33F9fC3444f8101754aBC46c52416550D1
    
    //mapping processes
    mapping (address => uint) public unClaimedAirdrop;
    mapping (uint => uint) public unClaimedReward;
    mapping(address => bool) public vaultWhitelist;
    mapping(address => address) private upline;
    mapping(address => uint256) public initialVaultID;
    mapping(uint256 => uint256) public vaultGas;


    mapping(uint256 => uint256) public isCompoundCount;//vaultNum
    mapping(address => uint256) public vaultNum;
    mapping(uint256 => uint256) public vaultCompounded;
    mapping(uint256 => uint256) public vaultClaimed;
    mapping(address => uint256) public totalVaultDeposited;
    mapping(uint256 => uint256) public AR;
    mapping(uint256 => bool) public ARJail;
    mapping(address=>mapping(uint256=>address)) public downlineReferrals;
    mapping(address=>uint) private uplineBonus;
    mapping(address=>uint) private airdropsSent;
    mapping(address=>uint) private airdropsReceived;
    mapping(uint=>uint) private vaultGasUnclaim;
    mapping(uint=>uint) private vaultGasClaim;
    //

    mapping(uint256 => uint256) public todayRewardClaimed;
    mapping(address => uint256) public todaySell;
    mapping(address => uint256) private lastSell;
    mapping (address => bool) public presaleAddress;
    mapping (address => address) public oldReferer;
    
    struct Invest{
    uint256 id;
    address investors;
    uint256 initialDeposit;
    uint256 amountTokenInvested;
    uint256 currentROI;
    uint256 vestedDate;
    uint256 lastTimeCompound;
    uint256 timePaidVaultGas;
    uint256 pendingEarn;
    uint256 totalTokenEarned;
    bool releaseStatus;
    bool expires;
    }
    struct Downlines{
        uint stakeId;
        address uplineAddress;
        address downlineAddress;
        uint256 position;
    }
    Invest[] public allInvestment;
    Downlines[] public downline;

    //log events
    event LogTokenBulkSent(address token, address from, uint256 total);
    event LogTokenApproval(address token, uint256 total);
    event investmentReport(address investorAddress, uint256 amount, uint256 currentROI, uint256 investmentDate);
    event claimInvestment(address from, address to, uint256 totalEarned, uint256 ctime); 
    event compoundInvestment(address from, uint256 amountCompounded, uint256 ctime); 
    event SomeoneWasFeelingGenerous(address investor, uint256 totalAirdropAmount);
    event roundRobin(address _upline, address _downline, uint256 amount);
    event AirdropsSent(address[] airdroppees, uint256[] amounts);
    event VaultAirdrop(uint[] airdropVault, uint256[] amounts);
    modifier onlyPresaleContract(){
        require(presaleAddress[msg.sender]==true, "You can't use this function");
        _;
    }
    constructor(){
        token = IERC20(0xB7EbEC4b254ec40bE26368bdC011D28B6d80a916);//0xB7EbEC4b254ec40bE26368bdC011D28B6d80a916;//0x42C47bDEe5Ff82FAc177402eFfc1306362491c83
        BUSD = IERC20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);//0xaB1a4d4f1D656d2450692D237fdD6C7f9146e814 //0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d
        NFT = IERC721(0xca6D3afAa080fE160b9Fe1791cd2470306dabF51);
        rewardPoolAddress = 0xca6D3afAa080fE160b9Fe1791cd2470306dabF51;
        uint ctime = block.timestamp;
        uint256 amt = 90 ether;
        uint256 sh = 10 ether;
        allInvestment.push(Invest(0, topWallet, amt, amt, 150, ctime, ctime, ctime, 0, 0, false,false ));
        uplineBonus[topWallet] += sh;
        upline[topWallet] = topWallet;
        downline.push(Downlines(0,topWallet,topWallet,0));
        isCompoundCount[0] = 0;
        vaultNum[topWallet] += 1;
        AR[0] = 5;
        _investmentCount++;
        initialVaultID[topWallet] = 0;
        vaultWhitelist[topWallet] = true;
        setPresaleAddress(0x7192A45367C6d82d2aA1486BfC06B3C6297dF4b0,true);
        emit investmentReport(topWallet, amt, sh, ctime);
    }

    //function to return values
    function name() external view returns (string memory) {
        return _name;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }
    
    function getAirdropPrice() public  view returns (uint){
        return vGasPrice;
    }

    function getVaultGasInfo(uint _id) public view returns (uint, uint, uint){
        return(vaultGas[_id],vaultGasUnclaim[_id], vaultGasClaim[_id]);
    }

    function getUpline(address _addr) public view returns (address) {
        return upline[_addr];
    }

    function getCurrentTime() public view returns (uint256) {
        return block.timestamp;
    }

    function getAllInvestmentRecord() public view returns(Invest[] memory){
        return allInvestment;
    }

    function getVaultUpline() public view returns(Downlines[] memory){
        return downline;
    }

    function getPendingEarn(uint256 _id) public view returns (uint256) {
        return allInvestment[_id].pendingEarn;
    }

    function getTotalCompoundedAndClaim(uint256 _id) public view returns (uint256, uint256, uint256, bool, uint256, uint256) {
        return (vaultCompounded[_id], vaultClaimed[_id], AR[_id], ARJail[_id], unClaimedReward[_id], vaultGas[_id]);
    }

    function getAirdropsSentAndRecieved(address _addr) public view returns (uint256, uint256, uint256, uint256, uint) {
        return (airdropsSent[_addr], airdropsReceived[_addr], uplineBonus[_addr], unClaimedAirdrop[_addr], totalVaultDeposited[_addr]);
    }

    function getDays(uint256 firstDate, uint256 secondDate) private view returns(uint256){
        return (firstDate - secondDate) / (60 * rewardTimeInMinute);
    }

    function getTodaySell(address _addr) public view returns (uint){
        return todaySell[_addr];
    }    

    function getNFTBalance(address _addr) public view returns (uint){
        return NFT.balanceOf(_addr);
    }

    function getNFTLines(address _addr) public view returns (uint){
        return IAlphaNFT(address(NFT)).getLines(_addr);
    }

    function getProfit(uint256 _id) private view returns(uint256) {
        uint256 ctime = block.timestamp;
        address investor = allInvestment[_id].investors;
        uint totalStake = allInvestment[_id].amountTokenInvested;
        uint256 dailyROI = allInvestment[_id].currentROI;
        uint256 vestedDate = allInvestment[_id].vestedDate;
        uint256 daysInvested = getDays(ctime, vestedDate);
        uint256 getIDays = getDays(ctime,allInvestment[_id].lastTimeCompound);
        uint256 qualifyForLast = getDays(allInvestment[_id].lastTimeCompound, vestedDate);
        if(daysInvested > 365 && vaultWhitelist[investor] == false){
            
            if(qualifyForLast < 365){
                return (((dailyROI * totalStake) / 100) / 100);
            }
            else{
                return 0;
            }
            
        }
        else{
            if(getIDays >= 1){
            return (((dailyROI * totalStake) / 100) / 100);
            }
            else{
            return 0;
            }
        }
        
    }

    //admins functions start
    function setForkRefer(address[] memory sender, address[] memory referer) public onlyOwner{
        require(sender.length==referer.length,"Length not match!");
        for (uint i = 0; i < sender.length; i++){
            oldReferer[sender[i]] = referer[i];
        }
    }

    function setPresaleAddress(address _addr, bool _flag) public  onlyOwner{
        presaleAddress[_addr] = _flag;
    }

    function vaultSettings(uint256 _rewardTimeInMinute, uint256 _roundRobinMax, uint256 _vaultMax, uint56 _vGasPrice, uint256 _jailLimit, uint256 _maxSell, uint256 _maxBuy, uint256 _minBuy) external onlyOwner{
        rewardTimeInMinute = _rewardTimeInMinute;
        roundRobinMax = _roundRobinMax;
        vaultMax = _vaultMax;
        jailLimit = _jailLimit;
        maxSell = _maxSell;
        maxBuy = _maxBuy;
        minBuy = _minBuy;
        vGasPrice = _vGasPrice;
    }

    function updateTokenAddress(address _token, address _nft) public onlyOwner{
        token = IERC20(_token);
        NFT = IERC721(_nft);
        rewardPoolAddress = _nft;
    }
    
    function addWhiteList(address _uddr, bool _status) external onlyOwner{
        if(vaultWhitelist[_uddr] != _status){
        vaultWhitelist[_uddr] = _status;
        }
    }

    function updateFeeAddr(address _dasAddress, address _sigmaAddress, address _topWallet, address _liqWallet, address _vaultGasAddress) external onlyOwner{
        dasAddress = _dasAddress;
        sigmaAddress = _sigmaAddress;
        topWallet = _topWallet;
        liqWallet = _liqWallet;
        vaultGasAddress = _vaultGasAddress;
    }
    
    function vaultsAirdrop(uint[] memory vaults, uint256[] memory amounts, uint256 sendAmount) external nonReentrant onlyOwner{
        address investor = msg.sender;
        require(vaults.length == amounts.length, "length no valid");
        require(vaults.length <= 250, "250 is the maximum to airdrop");

        for(uint i = 0; i < vaults.length; i++){
            uint256 amount = amounts[i];
            uint256 vaultId = vaults[i];
            address investors = allInvestment[vaultId].investors;
            airdropsReceived[investors] += amount;
            unClaimedAirdrop[investors] += amount;
        }

        airdropsSent[msg.sender] += sendAmount;
        token.transferFrom(investor,address(this),sendAmount);
    
        emit SomeoneWasFeelingGenerous(msg.sender, sendAmount);
        emit VaultAirdrop(vaults, amounts);
     }
    
    //admin functions end

    //users function and processes
    function computeDownline(uint _id, address _ref) private {
        uint i = 0;
        address currentUpLine;
        address oldDownline = _ref;
        while(i <= roundRobinMax){
           // oldDownline = _ref;
            currentUpLine = upline[oldDownline];
            if(currentUpLine == address(0) || oldDownline == topWallet){
                break;
            }
            downline.push(Downlines(_id,currentUpLine,_ref,i));
            downlineReferrals[_ref][i] = currentUpLine;  
            oldDownline =  currentUpLine;  
            i++;       
        }
        
    }

    function roundRobinSystem(uint _id, address _downline, uint256 pos, uint256 _share) private{
        //uint i = 0;
        address currentUpLine = downlineReferrals[_downline][pos];
       
        if(currentUpLine == address(0)){
            currentUpLine = topWallet;
        }
        uint vid = initialVaultID[currentUpLine];
        uint checkday = getDays(block.timestamp, allInvestment[vid].vestedDate);
        uint nftBalance = getNFTLines(currentUpLine); //NFT.balanceOf(currentUpLine);
        
        if(((checkday > 365) && vaultWhitelist[currentUpLine] == false) || (nftBalance == 0)){
            currentUpLine = topWallet;
            vid = initialVaultID[currentUpLine];
        }
        if(nftBalance != 0){
            if((nftBalance - 1) < pos){
            currentUpLine = topWallet;
            vid = initialVaultID[currentUpLine];
            }
        }

        if(vid < 1){
            currentUpLine = topWallet;
            vid = initialVaultID[currentUpLine];
        }
        
        if(pos==roundRobinMax){
            isCompoundCount[_id]=0;
        }
        else{
            isCompoundCount[_id]++;
        }
        unClaimedReward[vid] += _share;
        uplineBonus[currentUpLine] += _share;
        emit roundRobin(currentUpLine, _downline, _share);

    }

    function proxyRoundRobinSystem(uint _id, address _downline, uint256 pos, uint256 _share) internal{
        //uint i = 0;
        address currentUpLine = upline[_downline];
        
        if(currentUpLine == address(0)){
            currentUpLine = topWallet;
        }
        uint nftBalance = getNFTLines(currentUpLine); //NFT.balanceOf(currentUpLine);
        
        if(nftBalance == 0){
            currentUpLine = topWallet;
        }
        
        if(nftBalance != 0){
            if((nftBalance - 1) < pos){
            currentUpLine = topWallet;
            }
        }

        if(pos==roundRobinMax){
            isCompoundCount[_id]=0;
        }
        else{
            isCompoundCount[_id]++;
        }
        unClaimedAirdrop[currentUpLine] += _share;
        airdropsReceived[currentUpLine] += _share;
        emit roundRobin(currentUpLine, _downline, _share);
        
    } 
     

    function airdrop(address[] memory airdroppees, uint256[] memory amounts, uint256 sendAmount) external nonReentrant{
        address investor = msg.sender;
        require(airdroppees.length == amounts.length, "length no valid");
        require(airdroppees.length <= 250, "250 is the maximum to airdrop");

        uint256 getShare = (totalAirdropFee * sendAmount) / 100;
        uint256 amountToCharge = sendAmount - getShare;
        token.transferFrom(investor,address(this),sendAmount);
        distributeAirdropFee(sendAmount);
        uint256 amtSent = 0 ; 
        for(uint i = 0; i < airdroppees.length; i++){
            uint256 amount = amounts[i];
            address airdroppee = airdroppees[i];
            if(airdroppee == msg.sender) continue;
            uint vid = initialVaultID[airdroppee];
            
            uint256 rBal = amountToCharge - amtSent;
            if(rBal <= amount){
                amount = rBal;
            }
            amtSent += amount;
            if(vid > 0){
                unClaimedAirdrop[airdroppee] += amount;
                airdropsReceived[airdroppee] += amount;
            }
            else{
            allInvestment[0].amountTokenInvested += amount;
            allInvestment[vid].initialDeposit += amount;
            airdropsReceived[topWallet] += amount;
            }
            if(amtSent > sendAmount){
                break;
            }
        }
        airdropsSent[msg.sender] += sendAmount;
        
        emit SomeoneWasFeelingGenerous(msg.sender, sendAmount);
        emit AirdropsSent(airdroppees, amounts);
    }

    function changeROI(uint _id, uint256 _compoundShare) private{
        uint256 newRoi = allInvestment[_id].currentROI;
        uint256 currentEarning = (vaultClaimed[_id] + _compoundShare) / allInvestment[_id].initialDeposit;
        if(currentEarning >=1 && newRoi==150){
            newRoi = 100;
        }
        else if(currentEarning >=2 && newRoi==100){
            newRoi = 50;
        }
        allInvestment[_id].currentROI = newRoi;
    }

    function computeClaim(uint sid, uint _amount) private {
        vaultClaimed[sid] += _amount;  
        if(AR[sid] > 1 ){
            AR[sid] -= 2;
        }
        else{
            if(allInvestment[sid].currentROI==50){
                allInvestment[sid].currentROI = 25;
            }
            else if(allInvestment[sid].currentROI > 50){
            allInvestment[sid].currentROI = 50;
            }
            ARJail[sid] = true;
            AR[sid] = 0;
        }
          
    }

    function distributeBuyDepositFee(uint256 amount) private {
            uint256 balance = amount;
            //uint256 ref = (50 * balance) / 1000;
            uint256 sigma = (25 * balance) / 1000;
            uint256 das = (35 * balance) / 1000;
            uint256 nftReward = (20 * balance) / 1000;
            BUSD.transfer(dasAddress, das);
            BUSD.transfer(sigmaAddress, sigma);
            BUSD.transfer(rewardPoolAddress, nftReward);
            
    }

    function distributeDepositFee(uint256 amount) private {
            uint256 balance = amount;
            uint256 sigma = (20 * balance) / 1000;
            callSwap(sigma, sigmaAddress);
    }

    function distributeSellFee(uint256 amount) private {
            uint256 balance = amount;
            uint256 liq = (20 * balance) / 1000;
            uint256 sigma = (15 * balance) / 1000;
            uint256 nftReward = (15 * balance) / 1000;
            token.transfer(liqWallet, liq);
            callSwap(sigma, sigmaAddress);
            callSwap(nftReward, rewardPoolAddress);
    }

    function distributeAirdropFee(uint256 amount) private {
            uint256 balance = amount;
            uint256 liq = (20 * balance) / 1000;
            token.transfer(liqWallet, liq);
    }

    function distributeDumpTax(uint256 amount) private {

            uint256 balance = amount;
            uint256 liq = (30 * balance) / 100;
            uint256 sigma = (10 * balance) / 100;
            uint256 das = (10 * balance) / 100;
            callSwap(sigma, sigmaAddress);
            callSwap(das, dasAddress);
            token.transfer(liqWallet, liq);
            
    }

    function callSwap(uint256 amt, address addr) private{
        address[] memory path = new address[](2);
            path[0] = address(token);
            path[1] = address(BUSD);
                _uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                amt,
                0,
                path,
                addr, 
                block.timestamp
            );
    }

    function callSwapToken(uint bamt, address addr) private{
            address[] memory path = new address[](2);
            path[0] = address(BUSD);
            path[1] = address(token);
            
                // make the swap
                _uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                bamt,
                0,
                path,
                addr,
                block.timestamp
            );
    }

    function payVaultGas (uint256 _id) public nonReentrant{
        uint ctime = block.timestamp;
        require((allInvestment[_id].timePaidVaultGas + (60 * rewardTimeInMinute * 7)) < ctime,"You can't pay for gas fee now!");
        uint vaultAmount;
        uint gasPrice = 0;
        if(allInvestment[_id].amountTokenInvested < 150 ether){
            vaultAmount = 2 ether;
            gasPrice = 2 * vGasPrice;
        }
        else if(allInvestment[_id].amountTokenInvested < 500 ether){
            vaultAmount = 5 ether;
            gasPrice = 5 * vGasPrice;
        }
        else{
            vaultAmount = 10 ether;
            gasPrice = 10 * vGasPrice;
        }    

        BUSD.transferFrom(msg.sender, vaultGasAddress, vaultAmount);
        vaultGas[_id] += vaultAmount;
        vaultGasUnclaim[_id] += gasPrice;
        allInvestment[_id].timePaidVaultGas = block.timestamp;

    }

    function claimVaultGas(uint256 _id) public {
        uint vaultGasAmount = vaultGasUnclaim[_id];
        require( vaultGasAmount > 0, "You have not gas to claim");
        allInvestment[_id].amountTokenInvested += vaultGasAmount; 
        allInvestment[_id].initialDeposit += vaultGasAmount; 
        vaultGasClaim[_id] += vaultGasAmount; 
        vaultGasUnclaim[_id] = 0;
    }

    function compound(uint256 _id) public nonReentrant{
        uint256 ctime = block.timestamp;
        require(getDays(ctime,todayRewardClaimed[_id]) >= 1,'You can not compound again today!');
        require(getDays(ctime,allInvestment[_id].timePaidVaultGas) < 7,"You have not vault gas!");
        uint256 todayProfit = getProfit(_id);
        todayRewardClaimed[_id] = ctime;
        //uint pendingEarn = todayProfit; //allInvestment[_id].pendingEarn;
        address investor = allInvestment[_id].investors;
        uint256 getShare = (totalCompoundFee * todayProfit) / 100;
        uint referralShare = (referralFee * todayProfit) / 1000;
        uint256 compoundShare = todayProfit - getShare;

        //change dailyROI
        changeROI(_id,compoundShare);
        require(todayProfit > 0, 'You have no fund to compound yet!');
        require(AR[_id] < 10, 'You cannot compound again, you can claim only!');
        
        uint position = isCompoundCount[_id];
        roundRobinSystem(_id,investor,position,referralShare);
        //updateStakes
        if(AR[_id] < 10 ){
            AR[_id] += 1;
        }
        if(AR[_id]==jailLimit){
            ARJail[_id] = false;
        }
                
        vaultCompounded[_id] += compoundShare;
        allInvestment[_id].amountTokenInvested += compoundShare;
        allInvestment[_id].lastTimeCompound = ctime;
        allInvestment[_id].totalTokenEarned += compoundShare;
        
        uint256 unclaimAir = unClaimedAirdrop[investor];
        uint256 claimAir = unClaimedReward[_id];
        if(unclaimAir > 0){
            allInvestment[_id].amountTokenInvested += unclaimAir;
            allInvestment[_id].initialDeposit += unclaimAir;  
            totalVaultDeposited[investor] += unclaimAir;
            unClaimedAirdrop[investor] = 0;
        }
        if(claimAir > 0){
            allInvestment[_id].amountTokenInvested += claimAir;
            allInvestment[_id].initialDeposit += claimAir;  
            totalVaultDeposited[investor] += claimAir;
            unClaimedReward[_id] = 0;
        }
        emit compoundInvestment(investor, compoundShare , ctime);
    }
    
    function claim(uint256 _id) public nonReentrant{
        address investor = msg.sender;
        uint256 ctime = block.timestamp;
        //I uncommented this for a while
        require(getDays(ctime,todayRewardClaimed[_id]) >= 1,"You can not claim again today!");
        require(getDays(ctime,allInvestment[_id].timePaidVaultGas) < 7,"You have not vault gas!");
        uint256 todayProfit = getProfit(_id);
        allInvestment[_id].pendingEarn = todayProfit;
        todayRewardClaimed[_id] = ctime;
        //I uncommented this for a while
        require(ARJail[_id]==false, 'you are in jail, compound until you reach 10 AR');
        require(todayProfit > 0, 'You have no fund to claim yet!');
        
        uint pendingEarn = todayProfit; //allInvestment[_id].pendingEarn;
        address staker = allInvestment[_id].investors;
        require(staker==investor, "You are not authorize you claim this investment.");
        uint256 getShare = (totalWithdrawFee * pendingEarn) / 100;
        uint256 amountToWithdraw = pendingEarn - getShare;
        //change dailyROI
        changeROI(_id,amountToWithdraw);
        token.transfer(staker,amountToWithdraw);
        //updateStakes
        if(getDays(ctime, allInvestment[_id].vestedDate) > 365){
            allInvestment[_id].expires = true;
            totalVaultDeposited[investor] -= allInvestment[_id].initialDeposit;
        }
        computeClaim(_id,amountToWithdraw);
        allInvestment[_id].lastTimeCompound = ctime;
        allInvestment[_id].pendingEarn = 0;
        allInvestment[_id].totalTokenEarned += amountToWithdraw;
        if(unClaimedAirdrop[investor] > 0){
            allInvestment[_id].amountTokenInvested += unClaimedAirdrop[investor];
            allInvestment[_id].initialDeposit += unClaimedAirdrop[investor];  
            unClaimedAirdrop[investor] = 0;
            totalVaultDeposited[investor] += unClaimedAirdrop[investor];
        }
        if(unClaimedReward[_id] > 0){
            allInvestment[_id].amountTokenInvested += unClaimedReward[_id];
            allInvestment[_id].initialDeposit += unClaimedReward[_id];  
            totalVaultDeposited[investor] += unClaimedReward[_id];
            unClaimedReward[_id] = 0;
        }
        emit claimInvestment(address(this), investor, amountToWithdraw, ctime);
    }

    function depositVault(uint256 amount, address referer) external nonReentrant{
        address investor = msg.sender;
        uint256 investedTime = block.timestamp;
        require(amount >= minBuy && amount <= maxBuy,"You can't buy more than 1000 alpha");
        require(vaultNum[investor] < vaultMax && referer != investor, 'Vault Max reached: You can not deposit to vault again!');
        
        if(referer == address(0)){
            referer = sigmaAddress;
        }
        uint256 getShare = (totalDepositFee * amount) / 100;
        uint referralShare = (referralFee * amount) / 1000;
        uint256 amountToDeposit = amount - getShare;
        token.transferFrom(investor,address(this),amount);
        token.approve(address(_uniswapV2Router), amount);
        allInvestment.push(Invest(_investmentCount, investor, amountToDeposit, amountToDeposit, defaultROI, investedTime, investedTime, investedTime, 0, 0, false,false ));
        initialVaultID[investor] =_investmentCount;
        //uplineBonus[referer] += referralShare;
        // if(upline[investor] == address(0)){
            
        // }
        
        if(vaultNum[investor] < 1){
            upline[investor] = referer;
            totalVaultDeposited[investor] = amountToDeposit;
            computeDownline(_investmentCount,investor);
            roundRobinSystem(_investmentCount,investor,vaultNum[investor],referralShare);
            }
            else{
            totalVaultDeposited[investor] += amountToDeposit;
            roundRobinSystem(_investmentCount,investor,vaultNum[investor],referralShare);
            }
        
        isCompoundCount[_investmentCount] = 0;
        vaultNum[investor] += 1;
        AR[_investmentCount] = 5;
        _investmentCount++;
        distributeDepositFee(amount);
        emit investmentReport(investor, amountToDeposit, defaultROI, investedTime);
    }

    function proxyDepositVault(address _sender, uint256 amount, address referer) external nonReentrant onlyPresaleContract{
        address investor = _sender;
        uint256 investedTime = block.timestamp;
        require(vaultNum[investor] < vaultMax && referer != investor, 'Vault Max reached: You can not deposit to vault again!');
        
        if(referer == address(0)){
            referer = sigmaAddress;
        }
        address forkReferer = 0x6a705DD24522230A428E186F946065101CE833AE;
        if(referer== forkReferer){
                referer = oldReferer[_sender];
        }
        uint256 getShare = (totalDepositFee * amount) / 100;
        uint referralShare = (referralFee * amount) / 1000;
        uint256 amountToDeposit = amount - getShare;
        //token.transferFrom(msg.sender, address(this),amount);
        token.approve(address(_uniswapV2Router), amount);
        allInvestment.push(Invest(_investmentCount, investor, amountToDeposit, amountToDeposit, defaultROI, investedTime, investedTime, investedTime, 0, 0, false,false ));
        initialVaultID[investor] =_investmentCount;
        //uplineBonus[referer] += referralShare;
        
        //upline[investor] = referer;
        // if(upline[investor] == address(0)){
        //     upline[investor] = referer;
        // }

        if(vaultNum[investor] < 1){
            
            upline[investor] = referer;
            totalVaultDeposited[investor] = amountToDeposit;
            computeDownline(_investmentCount,investor);
            proxyRoundRobinSystem(_investmentCount,investor,vaultNum[investor],referralShare);
            }
            else{
            totalVaultDeposited[investor] += amountToDeposit;
            proxyRoundRobinSystem(_investmentCount,investor,vaultNum[investor],referralShare);
            }
        
        isCompoundCount[_investmentCount] = 0;
        vaultNum[investor] += 1;
        AR[_investmentCount] = 5;
        _investmentCount++;
        distributeDepositFee(amount);
        emit investmentReport(investor, amountToDeposit, defaultROI, investedTime);
    }

    function buyBUSDForVaultDeposit(uint256 busdAmount, address referer) external nonReentrant{
            address investor = msg.sender;  
            require(busdAmount > 0 && msg.sender != referer, "amount check fail");
            require(vaultNum[investor] < vaultMax, 'Vault Max reached: You can not deposit to vault again!');
            
            if(referer == address(0)){
            referer = sigmaAddress;
            }
            uint256 investedTime = block.timestamp;
            // get BUSD from user
            uint256 balanceBefore = token.balanceOf(address(this));
            uint256 busdBalanceBefore = BUSD.balanceOf(address(this));
            BUSD.transferFrom(investor, address(this), busdAmount);
            BUSD.approve(address(_uniswapV2Router), busdAmount);
            token.approve(address(_uniswapV2Router), token.totalSupply());

            uint256 newBusdBalance = BUSD.balanceOf(address(this));
            uint256 Bbalance = newBusdBalance - busdBalanceBefore;
            uint256 getShare = (8 * Bbalance) / 100;
            uint256 busdDeposit = Bbalance - (getShare);
            callSwapToken(busdDeposit, address(this));
            
            uint256 newBalance = token.balanceOf(address(this)) - balanceBefore;
            uint referralShare = (5 * newBalance) / 100;
            uint256 amountToDeposit = newBalance - referralShare;
            require(newBalance >= minBuy && newBalance <= maxBuy,"You can't buy more than 1000 alpha");
            distributeBuyDepositFee(Bbalance);
            allInvestment.push(Invest(_investmentCount, investor, amountToDeposit, amountToDeposit, defaultROI, investedTime, investedTime, investedTime, 0, 0, false,false ));
            
            initialVaultID[investor] =_investmentCount;
            upline[investor] = referer;
            if(vaultNum[investor] < 1){
            computeDownline(_investmentCount,investor);
            roundRobinSystem(_investmentCount,investor,vaultNum[investor],referralShare);
            totalVaultDeposited[investor] = amountToDeposit;
            }
            else{
            roundRobinSystem(_investmentCount,investor,vaultNum[investor],referralShare);
            totalVaultDeposited[investor] += amountToDeposit;
            }
            
            isCompoundCount[_investmentCount] = 0;
            vaultNum[investor] += 1;
            AR[_investmentCount] = 5;
            _investmentCount++;
            emit investmentReport(investor, amountToDeposit, defaultROI, investedTime); 
    }


    function sellAlphaForBUSD(uint256 tokenAmount) external  nonReentrant{
            uint256 ctime = block.timestamp;
            uint256 allVaultBalance = totalVaultDeposited[msg.sender];
            uint256 halfBalance = (50 * allVaultBalance) / 100;
            require(tokenAmount > 0 && tokenAmount <= maxSell, "amount check fail");
            require(todaySell[msg.sender] < ctime || ((lastSell[msg.sender] + tokenAmount) <= maxSell) ,"You can not claim again today!");
            
            uint256 balanceBefore = token.balanceOf(address(this));
            uint vid = initialVaultID[msg.sender];
            
            token.transferFrom(address(msg.sender), address(this), tokenAmount);
            uint256 tokenBalance = token.balanceOf(address(this)); 
            uint256 toSell = tokenBalance;
            token.approve(address(_uniswapV2Router), toSell); // approve
            uint256 newBalance = token.balanceOf(address(this));
            uint256 balance = newBalance - balanceBefore;
            uint256 getShare = 0;
            uint256 amountToPay = 0;
            if((vid <= 0 || allInvestment[vid].expires == true) || tokenAmount > halfBalance){
            getShare = (90 * balance) / 100;
            amountToPay = balance - getShare;
            callSwap(amountToPay, msg.sender);
            distributeDumpTax(balance);
            }
            else{
            getShare = (totalSellFee * balance) / 100;
            amountToPay = balance - getShare;
            callSwap(amountToPay, msg.sender);
            distributeSellFee(balance);
            }
            if(lastSell[msg.sender] > 0 && ctime < todaySell[msg.sender]){
                lastSell[msg.sender] += tokenAmount;
            }
            else{
                lastSell[msg.sender]  = tokenAmount;
                todaySell[msg.sender] = ctime + 600;
            }
            
    }

    // Withdraw ERC20 tokens that are potentially stuck
    
    function recoverETHfromContract() external onlyOwner {
        payable(msg.sender).transfer(address(this).balance);
    }
    //referer box
    
}