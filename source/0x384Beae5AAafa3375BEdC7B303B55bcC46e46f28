// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

library SafeMath {
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

contract MintReqWojakV1 is Ownable {
    IERC20 public BUSDToken;
    IERC20 public WUSDToken;

    using SafeMath for uint;

    uint public priceBUSD = 650000000000000000000;
    uint public priceWUSD = 650000000000;
    uint public priceEveryNFT = 3;

    uint public totalMintRequest = 0;
     uint public totalMint = 0;
    uint public totalBusdPay = 0;
    uint public totalWusdPay = 0;
    bool public mintOptionalEnabled = true;
    bool public mintEnabled = false;


    address adminAddress;
    event NewMintRequest(
        address user,
        uint256 amountBusd,
        uint256 amountWusd,
        uint256 nftCount
       
    );

    constructor(address _BUSDToken, address _WUSDToken,address _adminAddress) {
        BUSDToken = IERC20(_BUSDToken);
        WUSDToken = IERC20(_WUSDToken);
        adminAddress = _adminAddress;
    }

    function mintReqOptional(uint _nftCount,uint _busd , uint _wusd) public {
        require(mintOptionalEnabled, "Mint Disabled");
        require((_busd + _wusd) >= (priceEveryNFT * (_nftCount)), "amount wrong");


      

        if (_wusd > 0) {
           
            require(WUSDToken.balanceOf(msg.sender) >= _wusd*10**9, "Insufficient WUSD balance");
            require(WUSDToken.allowance(msg.sender, address(this)) >= _wusd*10**9, "Error Allowance");
            WUSDToken.transferFrom(msg.sender, adminAddress, _wusd*10**9);
            totalWusdPay = totalWusdPay.add(_wusd);
        }

        if (_busd > 0) {
           
            require(BUSDToken.balanceOf(msg.sender) >= _busd*10**18, "Insufficient BUSD balance");
            require(BUSDToken.allowance(msg.sender, address(this)) >= _busd*10**18, "Error Allowance");
            BUSDToken.transferFrom(msg.sender, adminAddress, _busd*10**18);
            totalBusdPay = totalBusdPay.add(_busd);
        }

        totalMintRequest = totalMintRequest.add(1);
         totalMint = totalMint.add(_nftCount);

        emit NewMintRequest(msg.sender, _busd, _wusd, _nftCount);
    }

   

    function updatePrice(uint256 _newPriceWUSD, uint256 _newPriceBUSD,uint256 _newPriceEveryNFT) public onlyOwner {
        priceWUSD = _newPriceWUSD;
        priceBUSD = _newPriceBUSD;
        priceEveryNFT = _newPriceEveryNFT;

    }

      function setAdminAddress(address _newAdminAddress) public onlyOwner {
        adminAddress = _newAdminAddress;
      

    }

    function withdraw() public onlyOwner {
        (bool os, ) = payable(owner()).call{value: address(this).balance}("");
        require(os);
    }

    function withdrawTokens(IERC20 token) public onlyOwner {
        uint256 balance = token.balanceOf(address(this));
        token.transfer(msg.sender, balance);
    }

    function enableMint() public onlyOwner {
        mintEnabled = true;
    }

    function disableMint() public onlyOwner {
        mintEnabled = false;
    }
}