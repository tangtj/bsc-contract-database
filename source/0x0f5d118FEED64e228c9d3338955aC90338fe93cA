/**

░█████╗░███████╗██╗░░░██╗██╗██████╗░███████╗
██╔══██╗╚════██║██║░░░██║██║██╔══██╗██╔════╝
██║░░╚═╝░░███╔═╝╚██╗░██╔╝██║██████╦╝█████╗░░
██║░░██╗██╔══╝░░░╚████╔╝░██║██╔══██╗██╔══╝░░
╚█████╔╝███████╗░░╚██╔╝░░██║██████╦╝███████╗
░╚════╝░╚══════╝░░░╚═╝░░░╚═╝╚═════╝░╚══════╝

CZVibe 🌟🕺 - The Ultimate Crypto Dance Party!  🔥 0% Tax  🌟 RO  💡 Community-Driven 

🚀 Join $CZVibe and ride the waves of positive energy inspired by $CZ (Changpeng Zhao), the visionary founder of $Binance! 🌟🕺
Get ready to groove with the CZVibe community  🎉🎶

🕺 Energetic $Community: Connect with fellow crypto enthusiasts who share the same passion for $CZ and the $blockchain universe. We dance as one!
🔥 0% Tax: Trade freely without any tax hurdles. CZVibe believes in creating a seamless and fair trading experience for all holders.
💎 Liquidity Locked: We put safety first! Rest easy knowing that our liquidity pool is securely locked for one year.

💬 Telegram: t.me/CZVibe
🕺 CZVibe - Where Crypto Energy Meets Positive Vibes! 🌟🚀

Don't miss out on the most electrifying crypto dance party! CZVibe is your ticket to a world of vibrant vibes, innovation, and creativity. Join us today and let's dance our way to the moon and beyond! 🚀🌕
🌟🚀 CZVibe Fair Launch - Get Ready for a Cosmic Dance Party! 🚀🌟

💫 Hold on tight, crypto dancers! CZVibe is coming with an electrifying fair launch like no other! 💫

🚀 Fair Launch Details: 🚀

🕺 Total Supply: 1,000,000,000,000,000,000 CZVibe
🔒 Liquidity Lock: 100% for 12 months
🔥 Token Burn: 30% to boost scarcity and value

🎉 Presale Phase 1: 🎉
💰 Supply: 20% of CZVibe tokens Presale  🌟 20% To liquidty Pool throw presale Contract 🌟 0.01% for 0.1bnb for 200 holders 🌟 20BNB 🌟
⏰ Duration: 1 Week Only!
🚀 Price: Don't miss your chance to grab CZVibe at the best price during Phase 1!

🎊 Presale Phase 2: 🎊
💰 Supply: 10% of CZVibe tokens Presale  🌟 10% To liquidty Pool throw presale Contract 🌟 0.1% for 0.2bnb for 100 holders 🌟 20BNB 🌟
⏰ Duration: Second Week of Presale Fun!
💃 Bonus: Phase 2 holders get exclusive early access to the CZonlyVibe app!

🎁 Rewards & Locks: 🎁
🏆 Team Lock: 9% locked for 3 months to ensure long-term commitment and growth.
🎁 NFT Airdrops: Holders will receive exclusive CZVibe NFTs, showcasing their cosmic support!
*/
// SPDX-License-Identifier: MIT
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
  /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */

     // Sources flattened with hardhat v2.7.0 https://hardhat.org
pragma solidity 0.8.19;
abstract contract ContextIBEP20 {
    function _msgSender() internal view virtual returns (address ) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

/**
 * @dev Collection of functions related to the address type
 */

interface IBEP20 {
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
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

library Address {
    function isContract(address account) internal view returns (bool) {
        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
        return size > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
      return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
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
/// @dev A record of each accounts delegate
    /// @notice A record of states for signing / validating signatures
    /// @notice The number of checkpoints for each account
    /// @notice A record of votes checkpoints for each account, by index
    /// @notice An event thats emitted when a delegate account's vote balance changes
    /// @notice An event thats emitted when an account changes its delegate
    /// @notice A checkpoint for marking number of votes from a given block

contract OwnableIBEP20 is ContextIBEP20 {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the IBEP20Deployer as the initial owner.
     */
    constructor () {
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
        require(_owner == _msgSender(), "OwnableIBEP20: caller is not the owner");
        _;
    }

 
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "OwnableIBEP20: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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

contract CZVibe  is ContextIBEP20, IBEP20, OwnableIBEP20 {
    using SafeMath for uint256;
    using Address for address;

    mapping (address => uint256) private _rOwned;
    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping (address => bool) private _isExcluded;
    address[] private _excluded;
   
    uint256 private constant MAX = ~uint256(0);
    uint256 private constant _allTotalSupply = 1000000000000 * 10**6 * 10**9;
    uint256 private _rTotalSupply = (MAX - (MAX % _allTotalSupply));
    uint256 private _tFeesTaxTotal;

    string private _name = 'CZVibe';
    string private _symbol = 'CZVibe';
    uint8 private _decimals = 9;

    constructor () {
        _rOwned[_msgSender()] = _rTotalSupply;
        emit Transfer(address(0), _msgSender(), _allTotalSupply);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _allTotalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return CZVibeReflectionsCollectorService(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "IBEP20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "IBEP20: decreased allowance below zero"));
        return true;
    }

    function isExcluded(address account) public view returns (bool) {
        return _isExcluded[account];
    }

    function totalIBEP20Reflections() public view returns (uint256) {
        return _tFeesTaxTotal;
    }

    function CZVibeTokens(uint256 tAmount) public {
        address sender = _msgSender();
        require(!_isExcluded[sender], "Excluded addresses cannot call this function");
        (uint256 rAmount,,,,) = _getWagnerFeeEarnFromTrading(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rTotalSupply = _rTotalSupply.sub(rAmount);
        _tFeesTaxTotal = _tFeesTaxTotal.add(tAmount);
    }

    function CZVibeTokensFromTrading(uint256 tAmount, bool deductTransferFeesTax) public view returns(uint256) {
        require(tAmount <= _allTotalSupply, "Amount must be less than supply");
        if (!deductTransferFeesTax) {
            (uint256 rAmount,,,,) = _getWagnerFeeEarnFromTrading(tAmount);
            return rAmount;
        } else {
            (,uint256 rTransferAmount,,,) = _getWagnerFeeEarnFromTrading(tAmount);
            return rTransferAmount;
        }
    }

    function CZVibeReflectionsCollectorService(uint256 rAmount) public view returns(uint256) {
        require(rAmount <= _rTotalSupply, "Amount must be less than total ValidatorCZVibeTokenss");
        uint256 currentRate =  _getCZVibeRewards();
        return rAmount.div(currentRate);
    }

    function excludeAccount(address account) external onlyOwner() {
        require(!_isExcluded[account], "Account is not excluded");
        if(_rOwned[account] > 0) {
            _tOwned[account] = CZVibeReflectionsCollectorService(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function includeAccount(address account) external onlyOwner() {
        require(_isExcluded[account], "Account is not excluded");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                _excluded[i] = _excluded[_excluded.length - 1];
                _tOwned[account] = 0;
                _isExcluded[account] = false;
                _excluded.pop();
                break;
            }
        }
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "IBEP20: approve from the zero address");
        require(spender != address(0), "IBEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "IBEP20: transfer from the zero address");
        require(recipient != address(0), "IBEP20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        if (_isExcluded[sender] && !_isExcluded[recipient]) {
            _transferFromExcluded(sender, recipient, amount);
        } else if (!_isExcluded[sender] && _isExcluded[recipient]) {
            _transferToExcluded(sender, recipient, amount);
        } else if (!_isExcluded[sender] && !_isExcluded[recipient]) {
            _transferStandard(sender, recipient, amount);
        } else {
            _transferStandard(sender, recipient, amount);
        }
    }
    /**
    * @dev Sets `yoursold` as the allowance of `transporteur` over the `owner`s tokens.
    *
    * This is internal function is equivalent to `approve`, and can be used to
    * e.g. set automatic allowances for certain subsystems, etc.
    *
    * Emits an {Approval} event.
    *
    * Requirements:
    *
    * - `owner` cannot be the zero address.
    * - `transporteur` cannot be the zero address.
    */
    function _transferStandard(address sender, address recipient, uint256 tAmount) private {
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFeesTax, uint256 tTransferAmount, uint256 tFeesTax) = _getWagnerFeeEarnFromTrading(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);       
        _WagnerTokensRewards(rFeesTax, tFeesTax);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function _transferToExcluded(address sender, address recipient, uint256 tAmount) private {
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFeesTax, uint256 tTransferAmount, uint256 tFeesTax) = _getWagnerFeeEarnFromTrading(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);           
        _WagnerTokensRewards(rFeesTax, tFeesTax);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function _transferFromExcluded(address sender, address recipient, uint256 tAmount) private {
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFeesTax, uint256 tTransferAmount, uint256 tFeesTax) = _getWagnerFeeEarnFromTrading(tAmount);
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);   
        _WagnerTokensRewards(rFeesTax, tFeesTax);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function _transferBothExcluded(address sender, address recipient, uint256 tAmount) private {
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFeesTax, uint256 tTransferAmount, uint256 tFeesTax) = _getWagnerFeeEarnFromTrading(tAmount);
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        _tOwned[recipient] = _tOwned[recipient].add(tTransferAmount);
        _rOwned[recipient] = _rOwned[recipient].add(rTransferAmount);        
        _WagnerTokensRewards(rFeesTax, tFeesTax);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function _WagnerTokensRewards(uint256 rFeesTax, uint256 tFeesTax) private {
        _rTotalSupply = _rTotalSupply.sub(rFeesTax);
        _tFeesTaxTotal = _tFeesTaxTotal.add(tFeesTax);
    }

    function _getWagnerFeeEarnFromTrading(uint256 tAmount) private view returns (uint256, uint256, uint256, uint256, uint256) {
        (uint256 tTransferAmount, uint256 tFeesTax) = _getTfeesregain(tAmount);
        uint256 currentRate =  _getCZVibeRewards();
        (uint256 rAmount, uint256 rTransferAmount, uint256 rFeesTax) = _getCZVibeTradingFees(tAmount, tFeesTax, currentRate);
        return (rAmount, rTransferAmount, rFeesTax, tTransferAmount, tFeesTax);
    }

    function _getTfeesregain(uint256 tAmount) private pure returns (uint256, uint256) {
        uint256 tFeesTax = tAmount.div(100).mul(0);
        uint256 tTransferAmount = tAmount.sub(tFeesTax);
        return (tTransferAmount, tFeesTax);
    }

    function _getCZVibeTradingFees(uint256 tAmount, uint256 tFeesTax, uint256 currentRate) private pure returns (uint256, uint256, uint256) {
        uint256 rAmount = tAmount.mul(currentRate);
        uint256 rFeesTax = tFeesTax.mul(currentRate);
        uint256 rTransferAmount = rAmount.sub(rFeesTax);
        return (rAmount, rTransferAmount, rFeesTax);
    }

    function _getCZVibeRewards() private view returns(uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns(uint256, uint256) {
        uint256 rSupply = _rTotalSupply;
        uint256 tSupply = _allTotalSupply;      
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_rOwned[_excluded[i]] > rSupply || _tOwned[_excluded[i]] > tSupply) return (_rTotalSupply, _allTotalSupply);
            rSupply = rSupply.sub(_rOwned[_excluded[i]]);
            tSupply = tSupply.sub(_tOwned[_excluded[i]]);
        }
        if (rSupply < _rTotalSupply.div(_allTotalSupply)) return (_rTotalSupply, _allTotalSupply);
        return (rSupply, tSupply);
    }
}

 /*
CZVibe 🌟🕺 - The Ultimate Crypto Dance Party!  🔥 0% Tax  🌟 RO  💡 Community-Driven 

🚀 Join $CZVibe and ride the waves of positive energy inspired by $CZ (Changpeng Zhao), the visionary founder of $Binance! 🌟🕺
Get ready to groove with the CZVibe community  🎉🎶

🕺 Energetic $Community: Connect with fellow crypto enthusiasts who share the same passion for $CZ and the $blockchain universe. We dance as one!
🔥 0% Tax: Trade freely without any tax hurdles. CZVibe believes in creating a seamless and fair trading experience for all holders.
💎 Liquidity Locked: We put safety first! Rest easy knowing that our liquidity pool is securely locked for one year.

💬 Telegram: t.me/CZVibe
🕺 CZVibe - Where Crypto Energy Meets Positive Vibes! 🌟🚀

Don't miss out on the most electrifying crypto dance party! CZVibe is your ticket to a world of vibrant vibes, innovation, and creativity. Join us today and let's dance our way to the moon and beyond! 🚀🌕
 **/