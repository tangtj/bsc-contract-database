// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

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
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
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
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
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
        _owner = 0x6ADbB5385698d756E4582f932acFca2AEa2ffaba;
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract Web3GiG_Presale is Ownable {

    uint256[] public priceperusd = [100000000000000000000]; 
    uint256 public constant totalTokenAmount = 190000000000000000000000000;                                      
    uint256 public constant stages = 1; // Number Of Stages
    uint256 public tokenAmountPerStage;
    uint256 public lastStagetime ;
    uint256 public stage = 0;
    uint256 public tokenSold = 0;
    IERC20 public w3g = IERC20(0x70E2EDFc8DD0f3cC5993D91F28c6227F9217269E);
    IERC20 public BSCUSD = IERC20(0x55d398326f99059fF775485246999027B3197955);
    mapping(address => Purchase[]) public purchases;
    mapping(address => uint256) public referralBalances;

    struct Purchase {
        uint256 stage;
        uint256 amount;
        bool claimed;
    }
    bool public isPresaleOpen = true;
    bool public isClaimable = false;


    constructor() {
        tokenAmountPerStage = totalTokenAmount / stages;
        lastStagetime = block.timestamp;

    }

    function PurchaseThroughUSD(uint256 usdtAmount) public {
        require(isPresaleOpen, "Presale has ended");
        uint256 tokensToBuy = usdtAmount * priceperusd[stage];
        uint256 usdtAmountFortransfer = usdtAmount;
        BSCUSD.transferFrom(msg.sender, address(this), usdtAmountFortransfer);
        
        tokenSold += (tokensToBuy / 1000000000000000000);
        purchases[msg.sender].push(Purchase(stage, tokensToBuy / 1000000000000000000, false));
        w3g.transfer(msg.sender, tokensToBuy / 1000000000000000000);

      
    }
    function getPurchaseInfo(address walletAddress)
        public
        view
        returns (Purchase[] memory)
    {
        return purchases[walletAddress];
    }

    function EndPresale(bool status) public onlyOwner {
        isPresaleOpen = status;
    }
    

    function withdrawbscusd() external onlyOwner {
        uint256 balance = BSCUSD.balanceOf(address(this));
        require(balance > 0, "No USDT available");
        BSCUSD.transfer(owner(), balance);
    }

    function withdraww3g() external onlyOwner {
        uint256 balance = w3g.balanceOf(address(this));
        require(balance > 0, "No token available");
        w3g.transfer(owner(), balance);
    }
}