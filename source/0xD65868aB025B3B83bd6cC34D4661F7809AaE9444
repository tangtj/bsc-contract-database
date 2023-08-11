// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

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
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

// File: contracts/PzpLock.sol


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

library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != 0x0 && codehash != accountHash);
    }
}

library SafeERC20 {
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint value) internal {
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        require(address(token).isContract(), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}


contract PzpCrossChainV2 is IERC20, ReentrancyGuard {
    using SafeERC20 for IERC20;
    mapping(address => bool) public hasRole;
    mapping (address => mapping (address => uint256)) public override allowance;

    address[] public addressesWithRole;
    address public admin;
    IERC20 token;
    
    event LogChangeAdmin(address indexed oldAdmin, address indexed newAdmin, uint indexed effectiveTime);
    event LogSwapIn(IERC20 _token,address from, address to, uint256 amount, uint date);
    event LogSwapOut(IERC20 _token, address from, address to, uint256 amount, uint date);
    event Deposit(IERC20 _token,address from, address to, uint256 amount, uint date);
    event Received(address, uint256);

    constructor() {
        admin = msg.sender;
        token = IERC20(0x6ad9E9c098a45B2B41b519119C31c3DcB02ACcB2);
        grantRole(admin);
        addressesWithRole.push(admin);
    }

    modifier OnlyAdmin() {
        require(admin == msg.sender, "Only Admin can access");
        _;
    }

    modifier HasRole(){
        require(hasRole[msg.sender], "Address does not have the role");
         _;
    }
    
    function SwapIn(uint256 amount)nonReentrant external HasRole returns (bool) {
        require(token.allowance(msg.sender, address(this)) >= amount, "PZPV1: Insufficient allowance");
        require(token.balanceOf(msg.sender) > 0, "PZPV1: Not enough pzp");

        // Transfer tokens from the 'caller' address to the 'to' address
        token.safeTransferFrom(msg.sender, address(this), amount);
        emit LogSwapIn(token,msg.sender, address(this), amount, block.timestamp);
        return true;
    }
    function SwapIn(IERC20 _token,uint256 amount)nonReentrant external  returns (bool) {
        require(_token.allowance(msg.sender, address(this)) >= amount, "PZPV1: Insufficient allowance");
        require(_token.balanceOf(msg.sender) > 0, "PZPV1: Not enough tokens");

        // Transfer tokens from the 'caller' address to the 'to' address
        _token.safeTransferFrom(msg.sender, address(this), amount);
        emit LogSwapIn(_token,msg.sender, address(this), amount, block.timestamp);
        return true;
    }

    function SwapOut(address from,address to,uint256 amount, address commissionAddress, uint256 fee)nonReentrant external HasRole{
        token.safeTransfer(to, amount);
        token.safeTransfer(commissionAddress, fee);
        emit LogSwapOut(token,from, to, amount, block.timestamp);
    }
    function SwapOut(IERC20 _token, address from,address to,uint256 amount, address commissionAddress, uint256 fee)nonReentrant external HasRole{
        _token.safeTransfer(to, amount);
        _token.safeTransfer(commissionAddress, fee);
        emit LogSwapOut(_token,from, to, amount, block.timestamp);
    }

    function approve(address spender, uint256 value) external override returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);

        return true;
    }

    function changeAdmin(address newAdmin) external OnlyAdmin {
        require(newAdmin != address(0), "Invalid new admin address");
        emit LogChangeAdmin(admin, newAdmin, block.timestamp);
        admin = newAdmin;
    }

    function grantRole(address _address) public OnlyAdmin {
        hasRole[_address] = true;
        addressesWithRole.push(_address);
    }

    function revokeRole(address _address) external OnlyAdmin {
        hasRole[_address] = false;
        removeAddressFromArray(_address);
    }

    function revokeRoleFromAll() external OnlyAdmin {
        for (uint256 i = 0; i < addressesWithRole.length; i++) {
            address _address = addressesWithRole[i];
            hasRole[_address] = false;
        }
        delete addressesWithRole;
    }

    function removeAddressFromArray(address _address) internal OnlyAdmin{
        for (uint256 i = 0; i < addressesWithRole.length; i++) {
            if (addressesWithRole[i] == _address) {
                addressesWithRole[i] = addressesWithRole[addressesWithRole.length - 1];
                addressesWithRole.pop();
                break;
            }
        }
    }

    receive() external payable {
        emit Received(msg.sender, msg.value);
    }
    function withdrawNative(address payable recipient, uint256 amount) nonReentrant external HasRole{
        require(address(this).balance >= amount, "Insufficient balance");
        recipient.transfer(amount);
    }
    function totalSupply() external view override returns (uint256) {}

    function decimals() external view override returns (uint8) {}

    function balanceOf(
        address account
    ) external view override returns (uint256) {}

    function transfer(
        address recipient,
        uint256 amount
    ) external override returns (bool) {}

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {}
}