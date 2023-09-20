// SPDX-License-Identifier: GPL-2.0-or-later




/*
     A smart contract that allows P2P decentralized exchange of tokens,
     operating in a trustless environment on the blockchain.
     USE CASES:

        1) A transparent, cheaper, faster and a modern solution for business finance
        2) No more sandwich attacks
        3) win-win for buyers and sellers
*/


/// @author iulianivg


pragma solidity ^0.8.0;

/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
        return size > 0;
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain`call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
      return functionCall(target, data, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: value }(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data, string memory errorMessage) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(bool success, bytes memory returndata, string memory errorMessage) private pure returns(bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

pragma solidity ^0.8.0;

/*
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
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

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
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

pragma solidity ^0.8.0;

/**
 * @title Counters
 * @author Matt Condon (@shrugs)
 * @dev Provides counters that can only be incremented or decremented by one. This can be used e.g. to track the number
 * of elements in a mapping, issuing ERC721 ids, or counting request ids.
 *
 * Include with `using Counters for Counters.Counter;`
 */
library Counters {
    struct Counter {
        // This variable should never be directly accessed by users of the library: interactions must be restricted to
        // the library's function. As of Solidity v0.5.2, this cannot be enforced, though there is a proposal to add
        // this feature: see https://github.com/ethereum/solidity/issues/4637
        uint256 _value; // default: 0
    }

    function current(Counter storage counter) internal view returns (uint256) {
        return counter._value;
    }

    function increment(Counter storage counter) internal {
        unchecked {
            counter._value += 1;
        }
    }

    function decrement(Counter storage counter) internal {
        uint256 value = counter._value;
        require(value > 0, "Counter: decrement overflow");
        unchecked {
            counter._value = value - 1;
        }
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function decimals() external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);


}

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
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}


library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}


interface IWETH {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}

contract P2PDexchange is ReentrancyGuard, Ownable {


    event P2PForSale(IERC20 indexed t1, IERC20 indexed t2, uint256 a1, uint256 a2, uint256 indexed id);
    event P2PSold(IERC20 indexed t1, IERC20 indexed t2, uint256 id, address indexed buyer);

    struct PairSwap {
        address seller; 
        IERC20 t1;  // token 1 (if eth, then weth)
        IERC20 t2;  // token 2 (if eth, then weth)
        uint256 a1;  // availability
        uint256 a2;  // total amount
        uint256 a3; // remainder
        bool status; // when 0 remainder
    }


    // a mapping of token1 -> (token2 - pairswap_ids) to find token pairs
    // by searching e.g WETH-USDT
    mapping(address => mapping(address => uint256[])) public poolsData;


    // keeps track of offers
    uint256 private currentId; 
    address public weth; 
    uint256 public fee = 0; // in case we want to apply a fee

    // all pairs accessible via their id
    mapping(uint256 => PairSwap) public swaps;
    mapping(address => uint256[]) public userIds; 

    constructor(address _weth) {
        weth = _weth;
    }





    /**
    * @dev  Function is invoked to create a token-token P2P dex. Whoever invokes this method is required to 
    *       deposit _token1 in the smart contract.
    * @param _token1 The token that is being advertised for swap
    * @param _token2 The token that is used to pay for _token1
    * @param _amounts1 The amount of _token1 for swap
    * @param _amounts2 The amount of _token2 payable for all value of _token1 
    **/
    function createTokenDexchange(IERC20 _token1, IERC20 _token2, uint256 _amounts1, uint256 _amounts2) public  {

        require(IERC20(weth) != _token1, 'invalid token pair'); // use createEthTokenDexchange function. 
        require(_token1 != _token2, 'invalid token pair'); // cannot swap same token 
        require(_amounts1 >0 && _amounts2 > 0, 'amounts not enough');

        PairSwap memory _swap;
        _swap.seller = msg.sender; 
        _swap.t1 = _token1; 
        _swap.t2 = _token2; 
        _swap.a1 = _amounts1;
        _swap.a2 = _amounts2; 
        _swap.a3 = _amounts1; 
        _swap.status = true; // active

        TransferHelper.safeTransferFrom(address(_token1), msg.sender, address(this), _amounts1);

        swaps[currentId] = _swap;
        userIds[msg.sender].push(currentId);
        poolsData[address(_token1)][address(_token2)].push(currentId);

        emit P2PForSale(_token1, _token2, _amounts1, _amounts2, currentId);
        currentId += 1;

    }

    /**
    * @dev  Function is invoked to create an ETH-token P2P dex. Whoever invokes this method is required to 
    *       deposit ETH in the smart contract.
    * @param _token2 The token that is used to pay for ETH
    * @param _amounts2 The amount of _token2 payable for all value of ETH deposited 
    **/
    function createEthTokenDexchange(IERC20 _token2, uint256 _amounts2) payable public  {

        
        require(IERC20(weth) != _token2, 'invalid token pair'); // cannot swap weth for eth

        require(msg.value >0 && _amounts2 > 0, 'amounts not enough');

        PairSwap memory _swap;
        _swap.seller = msg.sender; 
        _swap.t1 = IERC20(weth); 
        _swap.t2 = _token2; 
        _swap.a1 = msg.value;
        _swap.a2 = _amounts2; 
        _swap.a3 = msg.value;
        _swap.status = true; // active

        // WE DO NOT NEED BELOW SINCE PAYABLE
        // payable(address(this)).transfer(msg.value); 

        userIds[msg.sender].push(currentId);
        poolsData[address(weth)][address(_token2)].push(currentId);
        swaps[currentId] = _swap;
        emit P2PForSale(IERC20(weth), _token2, msg.value, _amounts2, currentId ); 
        currentId += 1;

    }



    /**
    * @dev  Function is invoked to swap tokens for an already created P2P dex pool.
    *
    * @param _id ID of the p2p dex
    * @param _amount The amount of tokens to be purchased by the user
    **/
    function swapTokenForToken(uint256 _id, uint256 _amount) public nonReentrant {
        require(_amount > 0, 'invalid amount');
        PairSwap memory _pair = swaps[_id];
        require(_pair.status == true, 'trade already settled');
        require(_pair.a3 - _amount >= 0, 'not enough trade availability'); // means remainder amount has been swapped 
        

        // uint256 decimals_t1 = _pair.t1.decimals();
        uint256 decimals_t2 = _pair.t2.decimals();
        require(decimals_t2 < 30, 'sorry, our p2pdex does not support this');
        // trick to fix decimal related problems
        uint256 PRECISION_FACTOR = uint256(10**(uint256(30)-(decimals_t2)));

        uint256 tokenRate =  _pair.a2  * PRECISION_FACTOR / _pair.a1;
        uint256 toSend = _amount * tokenRate / PRECISION_FACTOR;

        if(fee > 0){
            uint256 feeToOwner = toSend * fee / 10000;
            TransferHelper.safeTransferFrom(address(_pair.t2), msg.sender, owner(), feeToOwner);
            TransferHelper.safeTransferFrom(address(_pair.t2), msg.sender, _pair.seller, toSend-feeToOwner);
        } else {
            TransferHelper.safeTransferFrom(address(_pair.t2), msg.sender, _pair.seller, toSend); // funds transferred to the seller
        }

        
        // we check if we should make p2pdex inactive
        if((swaps[_id].a3 - _amount) == 0){
            swaps[_id].status = false; 
        }

        // update remainder amount
        swaps[_id].a3 = swaps[_id].a3 - _amount; 

        // if token1 sold is WETH, we send ETH from the contract otherwise just send the token 
        if(address(_pair.t1) == weth){
            payable(msg.sender).transfer(_amount);
        } else {
            TransferHelper.safeTransfer(address(_pair.t1), msg.sender, _amount);
        }
        emit P2PSold(_pair.t1, _pair.t2, _id, msg.sender);

    }

    /**
    * @dev  Function is invoked to swap ETH for tokens in a p2p dex "token-eth" style
    *
    * @param _id ID of the p2p dex
    * @param _amount The amount of tokens to be purchased by the user
    **/
    function swapEthForToken(uint256 _id, uint256 _amount) public payable nonReentrant {


        require(_amount > 0 && msg.value > 0, 'invalid amount');

        PairSwap memory _pair = swaps[_id];
        require(address(_pair.t2) == weth, 'invalid token'); // can only use this method if token 2 is WETH
        require(_pair.status == true, 'trade already settled');
        require(_pair.a3 - _amount >= 0, 'not enough trade availability'); // means remainder amount is not enough
        require(msg.sender != _pair.seller, 'you cannot buy from yourself');


        uint256 PRECISION_FACTOR = uint256(10**(uint256(30)-(18))); // 18 is weth decimals
        uint256 tokenRate =  _pair.a2  * PRECISION_FACTOR / _pair.a1;

        uint256 toSend = _amount * tokenRate / PRECISION_FACTOR;

        if(fee > 0){
            uint256 feeToOwner = toSend * fee / 10000;
            payable(owner()).transfer(feeToOwner);
            payable(_pair.seller).transfer(toSend-feeToOwner);

        } else {
                // statement not required since we use "transfer"
                // require(msg.value == toSend,'invalid msg value');
                payable(_pair.seller).transfer(toSend);
        }

        if((swaps[_id].a3 - _amount) == 0){
            swaps[_id].status = false; 
        }
        swaps[_id].a3 = swaps[_id].a3 - _amount; 

        TransferHelper.safeTransfer(address(_pair.t1), msg.sender, _amount);

        emit P2PSold(_pair.t1, _pair.t2, _id, msg.sender);

    }

    /**
    * @dev  Function is invoked to void a p2p dexchange. 
    *       It requires the sender to be the seller. 
    *
    * @param _id ID of the p2p dex
    **/
    function withdrawOffer(uint256 _id) public nonReentrant {
        PairSwap memory _pair = swaps[_id];
        require(msg.sender == _pair.seller, 'invalid user');
        require(true == _pair.status, 'already settled, nothing to do here');
        uint256 depositRemaining = _pair.a3; 
        
        swaps[_id].status = false; 
        swaps[_id].a3 = 0; 

        if(address(_pair.t1) == weth){
            payable(_pair.seller).transfer(depositRemaining);
        } else {
            TransferHelper.safeTransfer(address(_pair.t1), _pair.seller, depositRemaining);
        }
        emit P2PSold(_pair.t1, _pair.t2, _id, msg.sender);

    }   


    /**
    * @dev  Function is invoked to change the fee
    *
    * @param _fee The fee. 100 = 1% 
    **/
    function changeFee(uint256 _fee) public onlyOwner{
        require(_fee <= 200, '2% max fee');
        fee = _fee; 
    }



        /**
    * @dev function to get the length of a pair (how many ids it contains
    * @param _t1 The token that is being advertised for swap
    * @param _t2 The token that is used to pay for _token1
    **/
    function getPairLength(address _t1, address _t2) public view returns (uint256){
        return poolsData[_t1][_t2].length;
    }

    /**
    * @dev  Function to get all ids of a token pair, based on patterns pagination
    * CREDITS to https://programtheblockchain.com/posts/2018/04/20/storage-patterns-pagination/
    *
    * @param _t1 The token that is being advertised for swap
    * @param _t2 The token that is used to pay for _token1
    * @param cursor The starting index for enumeration
    * @param howMany How many items should be returned
    **/

    function fetchPage(address _t1, address _t2, uint256 cursor, uint256 howMany)
    public
    view
    returns (uint256[] memory values, uint256 newCursor)
    {
        uint256 length = howMany;
        if (length > poolsData[_t1][_t2].length - cursor) {
            length = poolsData[_t1][_t2].length - cursor;
        }

        values = new uint256[](length);
        for (uint256 i = 0; i < length; i++) {
            values[i] = poolsData[_t1][_t2][cursor + i];
        }

        return (values, cursor + length);
    }

    /**
    * @dev  Function is invoked to get all sale ids of an address
    * @param _address The user address
    **/
    function getMyIds(address _address) public view returns(uint256[] memory){
        return userIds[_address];
    }

    function getMyIdsWithPagination(address _address, uint256 cursor, uint256 howMany) public view returns(uint256[] memory values, uint256 newCursor){
        uint256 length = howMany;
        if (length > userIds[_address].length - cursor) {
            length = userIds[_address].length - cursor;
        }

        values = new uint256[](length);
        for (uint256 i = 0; i < length; i++) {
            values[i] = userIds[_address][cursor + i];
        }

        return (values, cursor + length);
    }
}