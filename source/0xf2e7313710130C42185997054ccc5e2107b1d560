/**
 *Submitted for verification at BscScan.com on 2023-05-24
*/

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
contract Ownable
{
    address public owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() public {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0));
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
}
contract CvlVendor is Ownable {

    event Multisended(uint256 total);

    IERC20 immutable private cvlToken;

    constructor(
        address cvlAdress_
        ) {
        cvlToken = IERC20(cvlAdress_);
    }

    function multiTransfer(address[] memory _addresses, uint256[] memory _amounts)
    public onlyOwner
    returns(bool) 
    {
        uint256 total = 0;
        uint8 i = 0;
        for (i; i < _addresses.length; i++) {
            cvlToken.transfer(_addresses[i], _amounts[i]);
            total += _amounts[i];
        }
        emit Multisended(total);
        return true;
    }
}