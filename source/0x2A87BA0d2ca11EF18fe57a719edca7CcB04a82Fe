// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/*
                @dev Md. Sayem Abedin

*/

interface AggregatorV3Interface {
  function decimals() external view returns (uint8);

  function description() external view returns (string memory);

  function version() external view returns (uint256);

  function getRoundData(
    uint80 _roundId
  ) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);

  function latestRoundData()
    external
    view
    returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
}

// File: contracts/Presale.sol


pragma solidity ^0.8.0;

/*
                @dev Md. Sayem Abedin

*/


// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

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
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (access/Ownable.sol)

pragma solidity ^0.8.0;


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
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

 

pragma solidity ^0.8.0;

interface IERC20 {
    
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}



contract Presale is Ownable{

    IERC20 public token;
    // Stablecoins
    IERC20 public usdt;
    IERC20 public usdc;

    uint public rate ; // 10 token per dollar.

    uint public total_sold  = 0; // to keep tract of total sold token.


    uint public presale_time = block.timestamp + 30 days;

    AggregatorV3Interface private priceFeed;


    constructor(address _token, address _usdt, address _usdc, uint _rate, address _priceFeedAddress){

        token = IERC20(_token);
        usdc = IERC20(_usdc);
        usdt = IERC20(_usdt);

        rate = _rate;


        priceFeed = AggregatorV3Interface(_priceFeedAddress);

    }

    function buy_with_native_token() public payable { 

        require(presale_time>=block.timestamp, "Presale ended");

        uint _token_amount = (rate * msg.value * getETHPrice())/ (10**18 * 10**priceFeed.decimals());

        total_sold += _token_amount;
        token.transfer(msg.sender, _token_amount);


    }

    function buy_with_stablecoins(uint _tid, uint _amount) public { // _tid = 1 = usdt, _tid = 2 = usdc

        require(_tid == 1 || _tid ==2, "Invalid token id");
        require(presale_time>=block.timestamp, "Presale ended");


        // Calculate the amount of token to be sold.

        if(_tid ==1 ){

            require(usdt.allowance(msg.sender, address(this))>= _amount, "Does to have enough allownace to buy");
            uint _token_amount = (rate * _amount)/ 10**usdt.decimals();

            total_sold += _token_amount;

            usdt.transferFrom(msg.sender, address(this), _amount);

            token.transfer(msg.sender, _token_amount);

        }else{

            require(usdc.allowance(msg.sender, address(this))>= _amount, "Does to have enough allownace to buy");
            uint _token_amount = (rate * _amount)/ 10**usdc.decimals();

            total_sold += _token_amount;

            usdc.transferFrom(msg.sender, address(this), _amount);

            token.transfer(msg.sender, _token_amount);


        }
    }

    function set_presale(uint _rate,address _token, address _usdt, address _usdc) public onlyOwner{

        rate = _rate;

        token = IERC20(_token);
        usdt = IERC20(_usdt);
        usdc = IERC20(_usdc);

    }

    function total_token_for_sale() public view returns(uint){
        return token.balanceOf(address(this));
    }

    function getETHPrice() public view returns(uint) {
        (, int256 price, , , ) = priceFeed.latestRoundData();
        return uint256(price);
    }

    function withdraw() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }

    function withdrawToken(address _add) external onlyOwner{
        IERC20(_add).transfer(msg.sender, IERC20(_add).balanceOf(address(this)));
    }




}