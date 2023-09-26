/**
 *Submitted for verification at BscScan.com on 2023-09-25
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
interface IAlphaVault{
    function depositVault(uint256 amount, address referer) external;
    function proxyDepositVault(address _sender, uint amount, address _addr) external;
    function buyBUSDForVaultDeposit(uint256 busdAmount, address referer) external;
    function sellAlphaForBUSD(uint256 tokenAmount) external;
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


contract ALPHA_SWAP is Ownable, ReentrancyGuard{
    //using SafeMath for uint256;
    //using SafeBEP20 for IERC20;
    IERC20 public token;
    IERC20 public BUSD;
    //contract information
    string private _name = "ALPHA SWAP";
    string private _symbol = "A-SWAP";
    uint8 private _decimals = 18;
    
    //fees settings
    uint256 public referralFee = 50;
    //uint256 public totalDepositFee = 10;
    uint256 public totalSellFee = 12;
    bool public useVault = true;
    
    //fee address
    address public vaultAddress;
    address public vaultGasAddress = 0xCa5C49f3C20D187BdC3D1EC16ce99b76CaFf880b;//alpha Sigma multisig
    address public dasAddress= 0x9224CAe52E0d36cd5875947d148a60443E70E56A;//Decentralize Account System Wallet
    address public sigmaAddress=0xF423860A73FD17DeF9a3029F84a79E7b226121a7;//Team multisigWallet //0xF423860A73FD17DeF9a3029F84a79E7b226121a7//0x319366e8d08b9DA18ac7103bb8cbC722C066335b
    address public rewardPoolAddress = 0x303eB2Fa35E1d1FdFCe8d7CCA6d671516f73f555; //reward nft Wallet
    address public liqWallet=  0xCa5C49f3C20D187BdC3D1EC16ce99b76CaFf880b; // liquidity wallet
    //deposit and withdraw variable settings
    uint256 public maxSell = 200 ether;
    uint256 public maxBuy = 1000 ether;
    uint256 public minBuy = 1 ether;
    uint public tokenPriceInBusd = 500;//5busd per alpha
    uint public busdPriceInToken = 500;//1alpha per 5busd
    
    mapping(address => uint256) public todaySell;
    mapping(address => uint256) private lastSell;

    event LogTokenApproval(address token, uint256 total);
    event investmentReport(address investorAddress, uint256 amount, uint256 currentROI, uint256 investmentDate);
    event claimInvestment(address from, address to, uint256 totalEarned, uint256 ctime); 
    
    constructor(){
        token = IERC20(0x5d75675E9DA82524B5DfBe3439Fe3a6E29f2b967);//0xB7EbEC4b254ec40bE26368bdC011D28B6d80a916;//0x42C47bDEe5Ff82FAc177402eFfc1306362491c83
        BUSD = IERC20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d);//0xaB1a4d4f1D656d2450692D237fdD6C7f9146e814 //0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d
        rewardPoolAddress = 0x303eB2Fa35E1d1FdFCe8d7CCA6d671516f73f555;
        vaultAddress = 0xF5c27FaD680Ea584dc9973F80920D74aCc1290af;
    }

    //function to return values
    function name() external view returns (string memory) {
        return _name;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }
    
    function getCurrentTime() external view returns (uint256) {
        return block.timestamp;
    }

    function getBUSDValue(uint _busdAmount) public view returns (uint){
        require(_busdAmount > 0, "no value to calculate");
            return (_busdAmount / tokenPriceInBusd) * 100;
    }
    function getTokenValue(uint _tokenAmount) public view returns (uint){
        require(_tokenAmount > 0, "no value to calculate");
            return (_tokenAmount * busdPriceInToken) / 100;
    }

    function vaultSettings(uint256 _maxSell, uint256 _maxBuy, uint256 _minBuy) external onlyOwner{
        maxSell = _maxSell;
        maxBuy = _maxBuy;
        minBuy = _minBuy;
    }

    function updateTokenAddress(address _token, address _nft, address _vaultAddress) external onlyOwner{
        token = IERC20(_token);
        rewardPoolAddress = _nft;
        vaultAddress = _vaultAddress;
    }

    function updateTokenPrice(uint256 _tokenPriceInBusd, uint256 _busdPriceInToken) external onlyOwner{
        tokenPriceInBusd = _tokenPriceInBusd;
        busdPriceInToken = _busdPriceInToken;
    }

    function updateUseSwap(bool _flag) external onlyOwner{
        useVault = _flag;
    }

    function updateFeeAddr(address _dasAddress, address _sigmaAddress, address _liqWallet, address _vaultGasAddress) external onlyOwner{
        dasAddress = _dasAddress;
        sigmaAddress = _sigmaAddress;
        liqWallet = _liqWallet;
        vaultGasAddress = _vaultGasAddress;
    }
    
    function depositPresaleToVault(uint256 tokenToSend,address referBy) external nonReentrant{
        //uint _busdVal = getTokenValue(tokenToSend);
        token.transferFrom(msg.sender, address(this), tokenToSend);
        require(tokenToSend >= minBuy && tokenToSend <= maxBuy,"You can't buy less than 10alpha and more than 1000 alpha");
        token.transfer(vaultAddress,tokenToSend);
        IAlphaVault(vaultAddress).proxyDepositVault(msg.sender,tokenToSend,referBy);
    }

    function depositTokenToVault(uint256 tokenToSend,address referBy) external nonReentrant{
        //uint _busdVal = getTokenValue(tokenToSend);
        token.transferFrom(msg.sender, address(this), tokenToSend);
        require(tokenToSend >= minBuy && tokenToSend <= maxBuy,"You can't buy less than 10alpha and more than 1000 alpha");
        token.transfer(vaultAddress,tokenToSend);
        if(useVault==true){
        IAlphaVault(vaultAddress).depositVault(tokenToSend,referBy);
        }
    }

    function buyBUSDForVaultDeposit(uint256 busdAmount, address referer) external nonReentrant{
            address investor = msg.sender;  
            require(busdAmount > 0 && msg.sender != referer, "amount check fail");
            
            if(referer == address(0)){
            referer = sigmaAddress;
            }
            BUSD.transferFrom(investor, address(this), busdAmount);
            uint _tokenVal = getBUSDValue(busdAmount);
            
            require(_tokenVal >= minBuy && _tokenVal <= maxBuy,"You can't buy more than 1000 alpha");
            distributeBuyDepositFee(busdAmount);
            if(useVault==true){
                IAlphaVault(vaultAddress).proxyDepositVault(msg.sender,_tokenVal,referer);
            //IAlphaVault(vaultAddress).buyBUSDForVaultDeposit(_tokenVal,referer); 
            }       
    }

    function sellAlphaForBUSD(uint256 tokenAmount, uint256 _halfBalance) external  nonReentrant{
            uint256 ctime = block.timestamp;
            uint256 halfBalance = _halfBalance; //(50 * allVaultBalance) / 100;
            require(tokenAmount > 0 && tokenAmount <= maxSell, "amount check fail");
            require(todaySell[msg.sender] < ctime || ((lastSell[msg.sender] + tokenAmount) <= maxSell) ,"You can not claim again today!");
            
            token.transferFrom(msg.sender, address(this), tokenAmount);
            uint256 busdAmount = getTokenValue(tokenAmount);
            uint256 taxes = (totalSellFee * busdAmount) / 100;
            uint256 amountTosend = busdAmount - taxes;  
            if(tokenAmount > halfBalance){
                amountTosend = (10 * busdAmount) / 100;
                distributeDumpTax(busdAmount);
            }
            else{
            distributeSellFee(busdAmount);
            }

            if(lastSell[msg.sender] > 0 && ctime < todaySell[msg.sender]){
                lastSell[msg.sender] += tokenAmount;
            }
            else{
                lastSell[msg.sender]  = tokenAmount;
                todaySell[msg.sender] = ctime + 86400;
            }
            
            BUSD.transfer(msg.sender, amountTosend);
            
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
            token.transfer(sigmaAddress,sigma );
    }

    function distributeSellFee(uint256 amount) private {
            uint256 balance = amount;
            uint256 liq = (20 * balance) / 1000;
            uint256 sigma = (15 * balance) / 1000;
            uint256 nftReward = (15 * balance) / 1000;
            BUSD.transfer(liqWallet, liq);
            BUSD.transfer(sigmaAddress, sigma);
            BUSD.transfer(rewardPoolAddress, nftReward);
    }

    function distributeDumpTax(uint256 amount) private {
            uint256 balance = amount;
            uint256 liq = (30 * balance) / 100;
            uint256 sigma = (10 * balance) / 100;
            uint256 das = (10 * balance) / 100;
            BUSD.transfer(sigmaAddress, sigma);
            BUSD.transfer(dasAddress, das);
            BUSD.transfer(liqWallet, liq);    
    }

    // Withdraw ERC20 tokens that are potentially stuck
    function recoverETHfromContract() external onlyOwner {
        payable(msg.sender).transfer(address(this).balance);
    }
    function withdraw_busd (uint amount, address _addr) external onlyOwner {
        BUSD.transfer(_addr, amount);
    }
    function withdraw_token (uint amount, address _addr) external onlyOwner {
        token.transfer(_addr, amount);
    }
    //referer box
    
}