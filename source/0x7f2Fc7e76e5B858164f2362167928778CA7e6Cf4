/**
 *Submitted for verification at BscScan.com on 2023-06-24
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.6.12;

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
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

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
 */

interface IST20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function decimals() external view returns (uint8);
    event Transfer(address indexed from, address indexed to, uint256 value);
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

contract Sale_Contract {
    using SafeMath for uint256;
    IST20 public token;
    IERC20 public usdt;
    uint256 public rate;
    uint256 public weiRaised;
    uint256 public weiMaxPurchaseBnb;
    uint256 public referralBonusRate = 10; // 10% referral bonus
    address payable private admin;
    mapping(address => uint256) public purchasedBnb;
    event TokenPurchase(address indexed purchaser, address indexed referrer, uint256 value, uint256 amount, uint256 referralBonus);
    event TokenPurchaseWithUSDT(address indexed purchaser, address indexed referrer, uint256 usdtAmount, uint256 amount, uint256 referralBonus);

    constructor(uint256 _rate, IST20 _token, uint256 _max, address _usdtAddress) public {
        require(_rate > 0);
        require(_max > 0);
        require(_token != IST20(address(0)));
        usdt = IERC20(_usdtAddress);
        rate = _rate;
        token = _token;
        weiMaxPurchaseBnb = _max;
        admin = msg.sender;
    }
  
    fallback() external payable {
        revert();
    }
  
    receive() external payable {
        revert();
    }
  
    function buy(address _referrer) public payable {
        if (_referrer == address(0)) {
            _referrer = 0xEC11772f73c02FB4d25377820EfE9CBCecDA7c19; // Default referrer address
        }
        
        uint256 maxBnbAmount = maxBnb(msg.sender);
        uint256 weiAmount = msg.value > maxBnbAmount ? maxBnbAmount : msg.value;
        weiAmount = _preValidatePurchase(msg.sender, weiAmount);
        uint256 tokens = _getTokenAmount(weiAmount);
        uint256 referralBonus = weiAmount.mul(referralBonusRate).div(100); // Calculate referral bonus in BNB
        weiRaised = weiRaised.add(weiAmount);
        _processPurchase(msg.sender, _referrer, tokens, referralBonus);
        emit TokenPurchase(msg.sender, _referrer, weiAmount, tokens, referralBonus);
        _updatePurchasingState(msg.sender, weiAmount);
        if (msg.value > weiAmount) {
            uint256 refundAmount = msg.value.sub(weiAmount);
            msg.sender.transfer(refundAmount);
        }
    }

    function buyWithUSDT(address _referrer, uint256 _usdtAmount) public {
        require(_usdtAmount > 0, "USDT amount must be greater than 0");
        require(_referrer != address(0), "Invalid referrer address");

        // Calculate the equivalent wei amount based on the USDT rate
        uint256 weiAmount = _usdtToWei(_usdtAmount);

        // Validate the purchase
        weiAmount = _preValidatePurchase(msg.sender, weiAmount);

        // Calculate the number of tokens to send
        uint256 tokens = _getTokenAmount(weiAmount);

        // Calculate the referral bonus
        uint256 referralBonus = weiAmount.mul(referralBonusRate).div(100);

        // Transfer USDT from the buyer to this contract
        require(usdt.transferFrom(msg.sender, address(this), _usdtAmount), "USDT transfer failed");

        // Process the purchase
        weiRaised = weiRaised.add(weiAmount);
        _processPurchase(msg.sender, _referrer, tokens, referralBonus);
        emit TokenPurchaseWithUSDT(msg.sender, _referrer, _usdtAmount, tokens, referralBonus);
        _updatePurchasingState(msg.sender, weiAmount);

        // Refund any excess USDT
        uint256 usdtBalance = usdt.balanceOf(address(this));
        if (usdtBalance > 0) {
            usdt.transfer(msg.sender, usdtBalance);
        }
    }

    // Internal function to convert USDT amount to wei based on the rate
    function _usdtToWei(uint256 _usdtAmount) internal view returns (uint256) {
        // Calculate the equivalent wei amount based on the USDT rate
        return _usdtAmount.mul(1e18).div(rate);
    }
  
    function _preValidatePurchase(address _beneficiary, uint256 _weiAmount) public view returns (uint256) {
        require(_beneficiary != address(0));
        require(_weiAmount != 0);
        uint256 tokenAmount = _getTokenAmount(_weiAmount);
        uint256 curBalance = token.balanceOf(address(this));
        if (tokenAmount > curBalance) {
            return curBalance.mul(1e18).div(rate);
        }
        return _weiAmount;
    }
  
    function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
        token.transfer(_beneficiary, _tokenAmount);
    }
  
    function _processPurchase(address _buyer, address _referrer, uint256 _tokenAmount, uint256 _referralBonus) internal {
        _deliverTokens(_buyer, _tokenAmount);
        if (_referralBonus > 0) {
            address payable referrer = payable(_referrer);
            referrer.transfer(_referralBonus);
        }
    }
  
    function _updatePurchasingState(address _beneficiary, uint256 _weiAmount) internal {
        purchasedBnb[_beneficiary] = _weiAmount.add(purchasedBnb[_beneficiary]);
    }
  
    function _getTokenAmount(uint256 _weiAmount) public view returns (uint256) {
        return _weiAmount.mul(rate).div(1e18);
    }
  
    function setPresaleRate(uint256 _rate) external {
        require(admin == msg.sender, "caller is not the owner");
        rate = _rate;
    }
  
    function maxBnb(address _beneficiary) public view returns (uint256) {
        return weiMaxPurchaseBnb.sub(purchasedBnb[_beneficiary]);
    }
  
    function withdrawCoins() external {
        require(admin == msg.sender, "caller is not the owner");
        admin.transfer(address(this).balance);
    }
  
    function withdrawTokens(address tokenAddress, uint256 tokens) external {
        require(admin == msg.sender, "caller is not the owner");
        IST20(tokenAddress).transfer(admin, tokens);
    }
}