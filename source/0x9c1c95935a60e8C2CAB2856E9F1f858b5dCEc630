// SPDX-License-Identifier: Unlicensed

pragma solidity 0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract SendBatch is Ownable {
    function withdraw(address payable to) external onlyOwner {
        if ( address(this).balance > 0 ) {
            to.transfer(address(this).balance);
        }
    }

    function withdrawToken(address tokenAddress, address to, uint256 amount) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        token.transfer(to, amount);
    }

    function sendBatch(address payable[] memory addresses, uint256[] memory amounts) public onlyOwner {
        require(addresses.length == amounts.length, "length does not match");
        for ( uint i = 0; i < addresses.length; i++ ) {
            addresses[i].transfer(amounts[i]);
        }
    }

    function sendBatchToken(address tokenAddress, address[] memory addresses, uint256[] memory amounts) public onlyOwner {
        require(addresses.length == amounts.length, "length does not match");
        IERC20 token = IERC20(tokenAddress);
        for ( uint i = 0; i < addresses.length; i++ ) {
            require(token.transferFrom(msg.sender, addresses[i], amounts[i]));
        }
    }

    function sendBatchToken2(address tokenAddress1, address tokenAddress2, address[] memory addresses, uint256[] memory amounts1, uint256[] memory amounts2) external onlyOwner {
        sendBatchToken(tokenAddress1, addresses, amounts1);
        sendBatchToken(tokenAddress2, addresses, amounts2);
    }

    receive() external payable {}
}