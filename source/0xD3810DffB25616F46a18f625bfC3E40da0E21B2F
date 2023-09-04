// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.0;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) return 0;
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }
}

contract OrderNFT {
    using SafeMath for uint256;

    address private _owner;

    mapping(address => mapping(uint256 => bool)) private _userNftOrders;

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    constructor() {
        _owner = msg.sender;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);

    function hasOrderedNFT(address userAddress, uint256 nftId) public view returns (bool) {
        return _userNftOrders[userAddress][nftId];
    }

    function getUserOrderedNFTs(address userAddress) public view returns (uint256[8] memory) {
        uint256[8] memory orderedNFTs;
        for (uint256 i = 1; i <= 8; i++) {
            orderedNFTs[i - 1] = hasOrderedNFT(userAddress, i) ? i : 0;
        }
        return orderedNFTs;
    }

function BuyNFT(uint256 ID_NFT) external {
    address contractAddress = 0xdDD42201E485ABa87099089b00978B87E7FBE796;
    IERC20 xmmToken = IERC20(contractAddress);

    uint256 Amount = 123;
    address buyer = msg.sender;

    uint256 nftCount = 0;
    for (uint256 i = 1; i <= 8; i++) {
        if (hasOrderedNFT(buyer, i)) {
            nftCount++;
        }
    }
    require(nftCount < 8, "You have already ordered 8 NFTs");

    require(!_userNftOrders[buyer][ID_NFT], "NFT has already been ordered by you");

    require(ID_NFT >= 1 && ID_NFT <= 8, "Invalid NFT ID");

    require(xmmToken.transferFrom(buyer, address(this), Amount), "Token transfer failed");
    uint256 ownerFee = Amount.mul(20).div(100);
    require(xmmToken.transfer(0xA99fa46814Fa4D10EA501120C10AA8D6E8234272, ownerFee), "Token transfer to owner failed");

    _userNftOrders[buyer][ID_NFT] = true;
}

}