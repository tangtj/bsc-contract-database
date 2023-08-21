// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function token0() external view returns (address);
    function token1() external view returns (address);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface IWBNB {
    function deposit() external payable;
}

interface IPancakeRouter {
    function WETH() external pure returns (address);  // This will return the WBNB address on BSC.
    function factory() external pure returns (address);
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
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
        _setOwner(address(0xdead));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IPancakePair {
    function sync() external;
    function mint(address to) external returns (uint liquidity);
}

interface IPancakeFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

contract fix is Ownable{

    function calculateTokenAmount(
        uint256 tokenAmount, 
        uint256 currencyAmount, 
        address token,
        address currency,
        address pair
    ) public view returns(uint256,uint256)  {
        uint256 tokenBalance;
        uint256 currencyBalance;
        tokenBalance = IERC20(token).balanceOf(pair);
        currencyBalance = IERC20(currency).balanceOf(pair);


        uint256 a = tokenAmount - tokenBalance;
        uint256 b = currencyAmount - currencyBalance;
      
        return (a,b);
    }

    function fixPool(
        uint256 tokenAmount, 
        uint256 currencyAmount, 
        bool isEth,
        address token,
        address currency,
        address pair
    ) payable public {
        IERC20(token).transferFrom(msg.sender,pair,tokenAmount);
        if(isEth){
            require(msg.value>=currencyAmount,"not enough currency balance");
             IWBNB wbnb = IWBNB(currency);
             wbnb.deposit{value: currencyAmount}();
             IERC20(address(wbnb)).transfer(pair,currencyAmount);
        }else{
            IERC20(currency).transferFrom(msg.sender,pair,currencyAmount);
        }
        IPancakePair(pair).mint(msg.sender);
    }


    function claimBNB() public onlyOwner{
        payable(_msgSender()).transfer(address(this).balance);
    }

    function claimToken(
        IERC20 token,
        uint256 amount
    ) public onlyOwner{
        token.transfer(_msgSender(), amount);
    }
}