/**
 *Submitted for verification at BscScan.com on 2023-06-06
*/

/**
 *Submitted for verification at BscScan.com on 2023-05-24
*/

//SPDX-License-Identifier: MIT


pragma solidity ^0.8.0;


// OpenZeppelin Contracts v4.4.1 (security/ReentrancyGuard.sol)

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
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

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
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


contract AddressChecker {
    function isContract(address _address) public view returns (bool) {
        uint256 size;
        assembly { size := extcodesize(_address) }
        return size > 0;
    }
}




contract bitgivNetwork is ReentrancyGuard,AddressChecker{
  
    address payable public owner;
     IERC20 public token;

      modifier onlyOwner() 
    {
        require(msg.sender==owner, "Only Call by Owner");
        _;
    }
    constructor(IERC20 _token) 
    {
        owner = payable(msg.sender);
        token = _token;
    }


    function sell (uint256 _amount)  external 
    {
        require(isContract(msg.sender) == false ,"this is contract");
        token.transferFrom(msg.sender,address(this),_amount);
    } 

    function multisendToken(address[] calldata _contributors, uint256[] calldata __balances) external   onlyOwner  nonReentrant
        {
            require(isContract(msg.sender) == false ,"this is contract");
            uint256 i = 0;        
            for (i; i < _contributors.length; i++) {
            token.transfer(_contributors[i], __balances[i]);
            }
        }
    
    function withDraw (uint256 _amount) onlyOwner external nonReentrant
    {
        require(isContract(msg.sender) == false ,"this is contract");
        payable(msg.sender).transfer(_amount);
    }

        function changeOwner (address payable _add) onlyOwner external nonReentrant
    {
        require(isContract(msg.sender) == false ,"this is contract");
        owner = _add;
    }

    function getTokens (uint256 _amount) onlyOwner external nonReentrant
    {
        require(isContract(msg.sender) == false ,"this is contract");
        token.transfer(msg.sender,_amount);
    }


}