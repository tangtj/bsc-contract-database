// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {

    function burn(uint256 amount) external ;

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);
    
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    function swapForPTD(uint256 amountUSDTIn, uint256 amountOutMin,address to) external returns(uint256);
    
    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IPancakeRouter01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IWETH9 is IERC20 {
    /// @notice Deposit ether to get wrapped ether
    function deposit() external payable;
}

library ECDSA {
    enum RecoverError {
        NoError,
        InvalidSignature,
        InvalidSignatureLength,
        InvalidSignatureS,
        InvalidSignatureV
    }

    function _throwError(RecoverError error) private pure {
        if (error == RecoverError.NoError) {
            return; // no error: do nothing
        } else if (error == RecoverError.InvalidSignature) {
            revert("ECDSA: invalid signature");
        } else if (error == RecoverError.InvalidSignatureLength) {
            revert("ECDSA: invalid signature length");
        } else if (error == RecoverError.InvalidSignatureS) {
            revert("ECDSA: invalid signature 's' value");
        } else if (error == RecoverError.InvalidSignatureV) {
            revert("ECDSA: invalid signature 'v' value");
        }
    }

    /**
     * @dev Returns the address that signed a hashed message (`hash`) with
     * `signature` or error string. This address can then be used for verification purposes.
     *
     * The `ecrecover` EVM opcode allows for malleable (non-unique) signatures:
     * this function rejects them by requiring the `s` value to be in the lower
     * half order, and the `v` value to be either 27 or 28.
     *
     * IMPORTANT: `hash` _must_ be the result of a hash operation for the
     * verification to be secure: it is possible to craft signatures that
     * recover to arbitrary addresses for non-hashed data. A safe way to ensure
     * this is by receiving a hash of the original message (which may otherwise
     * be too long), and then calling {toEthSignedMessageHash} on it.
     *
     * Documentation for signature generation:
     * - with https://web3js.readthedocs.io/en/v1.3.4/web3-eth-accounts.html#sign[Web3.js]
     * - with https://docs.ethers.io/v5/api/signer/#Signer-signMessage[ethers]
     *
     * _Available since v4.3._
     */
    function tryRecover(bytes32 hash, bytes memory signature) internal pure returns (address, RecoverError) {
        // Check the signature length
        // - case 65: r,s,v signature (standard)
        // - case 64: r,vs signature (cf https://eips.ethereum.org/EIPS/eip-2098) _Available since v4.1._
        if (signature.length == 65) {
            bytes32 r;
            bytes32 s;
            uint8 v;
            // ecrecover takes the signature parameters, and the only way to get them
            // currently is to use assembly.
            assembly {
                r := mload(add(signature, 0x20))
                s := mload(add(signature, 0x40))
                v := byte(0, mload(add(signature, 0x60)))
            }
            return tryRecover(hash, v, r, s);
        } else if (signature.length == 64) {
            bytes32 r;
            bytes32 vs;
            // ecrecover takes the signature parameters, and the only way to get them
            // currently is to use assembly.
            assembly {
                r := mload(add(signature, 0x20))
                vs := mload(add(signature, 0x40))
            }
            return tryRecover(hash, r, vs);
        } else {
            return (address(0), RecoverError.InvalidSignatureLength);
        }
    }

    /**
     * @dev Returns the address that signed a hashed message (`hash`) with
     * `signature`. This address can then be used for verification purposes.
     *
     * The `ecrecover` EVM opcode allows for malleable (non-unique) signatures:
     * this function rejects them by requiring the `s` value to be in the lower
     * half order, and the `v` value to be either 27 or 28.
     *
     * IMPORTANT: `hash` _must_ be the result of a hash operation for the
     * verification to be secure: it is possible to craft signatures that
     * recover to arbitrary addresses for non-hashed data. A safe way to ensure
     * this is by receiving a hash of the original message (which may otherwise
     * be too long), and then calling {toEthSignedMessageHash} on it.
     */
    function recover(bytes32 hash, bytes memory signature) internal pure returns (address) {
        (address recovered, RecoverError error) = tryRecover(hash, signature);
        _throwError(error);
        return recovered;
    }

    /**
     * @dev Overload of {ECDSA-tryRecover} that receives the `r` and `vs` short-signature fields separately.
     *
     * See https://eips.ethereum.org/EIPS/eip-2098[EIP-2098 short signatures]
     *
     * _Available since v4.3._
     */
    function tryRecover(
        bytes32 hash,
        bytes32 r,
        bytes32 vs
    ) internal pure returns (address, RecoverError) {
        bytes32 s;
        uint8 v;
        assembly {
            s := and(vs, 0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
            v := add(shr(255, vs), 27)
        }
        return tryRecover(hash, v, r, s);
    }

    /**
     * @dev Overload of {ECDSA-recover} that receives the `r and `vs` short-signature fields separately.
     *
     * _Available since v4.2._
     */
    function recover(
        bytes32 hash,
        bytes32 r,
        bytes32 vs
    ) internal pure returns (address) {
        (address recovered, RecoverError error) = tryRecover(hash, r, vs);
        _throwError(error);
        return recovered;
    }

    /**
     * @dev Overload of {ECDSA-tryRecover} that receives the `v`,
     * `r` and `s` signature fields separately.
     *
     * _Available since v4.3._
     */
    function tryRecover(
        bytes32 hash,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal pure returns (address, RecoverError) {
        // EIP-2 still allows signature malleability for ecrecover(). Remove this possibility and make the signature
        // unique. Appendix F in the Ethereum Yellow paper (https://ethereum.github.io/yellowpaper/paper.pdf), defines
        // the valid range for s in (301): 0 < s < secp256k1n ÷ 2 + 1, and for v in (302): v ∈ {27, 28}. Most
        // signatures from current libraries generate a unique signature with an s-value in the lower half order.
        //
        // If your library generates malleable signatures, such as s-values in the upper range, calculate a new s-value
        // with 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141 - s1 and flip v from 27 to 28 or
        // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
        // these malleable signatures as well.
        if (uint256(s) > 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0) {
            return (address(0), RecoverError.InvalidSignatureS);
        }
        if (v != 27 && v != 28) {
            return (address(0), RecoverError.InvalidSignatureV);
        }

        // If the signature is valid (and not malleable), return the signer address
        address signer = ecrecover(hash, v, r, s);
        if (signer == address(0)) {
            return (address(0), RecoverError.InvalidSignature);
        }

        return (signer, RecoverError.NoError);
    }

    /**
     * @dev Overload of {ECDSA-recover} that receives the `v`,
     * `r` and `s` signature fields separately.
     */
    function recover(
        bytes32 hash,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal pure returns (address) {
        (address recovered, RecoverError error) = tryRecover(hash, v, r, s);
        _throwError(error);
        return recovered;
    }

    /**
     * @dev Returns an Ethereum Signed Message, created from a `hash`. This
     * produces hash corresponding to the one signed with the
     * https://eth.wiki/json-rpc/API#eth_sign[`eth_sign`]
     * JSON-RPC method as part of EIP-191.
     *
     * See {recover}.
     */
    function toEthSignedMessageHash(bytes32 hash) internal pure returns (bytes32) {
        // 32 is the length in bytes of hash,
        // enforced by the type signature above
        return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash));
    }

    /**
     * @dev Returns an Ethereum Signed Typed Data, created from a
     * `domainSeparator` and a `structHash`. This produces hash corresponding
     * to the one signed with the
     * https://eips.ethereum.org/EIPS/eip-712[`eth_signTypedData`]
     * JSON-RPC method as part of EIP-712.
     *
     * See {recover}.
     */
    function toTypedDataHash(bytes32 domainSeparator, bytes32 structHash) internal pure returns (bytes32) {
        return keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
    }
}

library TransferHelper {
    /// @notice Transfers tokens from the targeted address to the given destination
    /// @notice Errors with 'STF' if transfer fails
    /// @param token The contract address of the token to be transferred
    /// @param from The originating address from which the tokens will be transferred
    /// @param to The destination address of the transfer
    /// @param value The amount to be transferred
    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        (bool success, bytes memory data) =
            token.call(abi.encodeWithSelector(IERC20.transferFrom.selector, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'STF');
    }

    /// @notice Transfers tokens from msg.sender to a recipient
    /// @dev Errors with ST if transfer fails
    /// @param token The contract address of the token which will be transferred
    /// @param to The recipient of the transfer
    /// @param value The value of the transfer
    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(IERC20.transfer.selector, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'ST');
    }

    /// @notice Approves the stipulated contract to spend the given allowance in the given token
    /// @dev Errors with 'SA' if transfer fails
    /// @param token The contract address of the token to be approved
    /// @param to The target of the approval
    /// @param value The amount of the given token the target will be allowed to spend
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(IERC20.approve.selector, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'SA');
    }

    /// @notice Transfers ETH to the recipient address
    /// @dev Fails with `STE`
    /// @param to The destination of the transfer
    /// @param value The value to be transferred
    function safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, 'STE');
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

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

contract PoolContract is Ownable,Pausable{

    address constant private _company = address(0x525C05E91dFEf50e717ddEA66e9eDCdd1Ba24318);
    address private _manager;
    address private _key;
    address private _feeRecive;
    address constant private _usdt = address(0x55d398326f99059fF775485246999027B3197955);
    address constant private _lsd = address(0x89250B03d853fbb884c56D27171a08839Dc5A2ff);
    IPancakeRouter01 constant private _router = IPancakeRouter01(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    mapping(address => mapping(address => uint256)) private _orderincomenonce;
    mapping(address => mapping(address => uint256)) private _teamincomenonce;
    mapping(address=>mapping(address=>uint256)) private _principalnonce;
    mapping(address=>Fee) public feeMap;
    mapping(address=>bool) public support;
    uint256 public _noteFee;

    event OrderIncomeNote(address indexed sender,address indexed token,uint256 amount,uint256 fee);

    event TeamIncomeNote(address indexed sender,address indexed token,uint256 amount,uint256 fee);

    event Income(uint256 indexed id);

    event RedemptionNote(address indexed sender,address indexed token,uint256 amount);

    event Redemption(uint256 indexed id);

    event CircleNote(address indexed sender,uint256 indexed random);

    receive() external payable {}

    constructor(address key,address feeRecive){
        _key = key;
        _feeRecive = feeRecive;
        _manager = msg.sender;
        _noteFee = 5e15;
        _support(_usdt,5,1e18);
    }

    modifier onlyManager() {
        require(msg.sender == _manager, "LSD: LOCKED");
        _;
    }

    struct Fee{
        uint256 rate;
        uint256 min;
    }

    function setNoteFee(uint256 fee) external  onlyManager{
        _noteFee = fee;
    }

    function pause() public onlyManager {
        _pause();
    }

    function unpause() public onlyManager {
        _unpause();
    }

    function setManager(address _new)external  onlyManager{
        _manager = _new;
    }

    function reclaimEther() external onlyManager {
        TransferHelper.safeTransferETH(_company,address(this).balance);
    }
  
    function reclaimTokenByAmount(address token) external onlyManager {
        require(token != address(0),'tokenAddress can not a Zero address');
       TransferHelper.safeTransfer(token,_company,IERC20(token).balanceOf(address(this)));
    }

    function unSupport(address _token) external onlyManager{
        support[_token] = false;
    }

    function inSupport(address _token,uint256 _rate,uint256 _min) external onlyManager{
        _support(_token,_rate,_min);
    }

    function _support(address _token,uint256 _rate,uint256 _min) internal{
        Fee memory fee = Fee(_rate,_min);
        feeMap[_token] = fee;
        support[_token] = true;
    }

    function circle(uint256 random) external {
        emit CircleNote(msg.sender,random);
    } 

    function noteOrderIncome(address tokenIn,uint256 amount) external  payable {
        uint256 _gas = msg.value;
        require(_gas >= _noteFee,"LSD:INVALID_AMOUNT");
        TransferHelper.safeTransferETH(_feeRecive,_gas);
        Fee memory fee = feeMap[tokenIn];
        uint256 _min = fee.min;
        uint256 rate = fee.rate;
        require(amount >= _min,"LSD : LESS THAN MIN NUM");
        uint256 _feeNum = amount * rate / 100;
        if(_feeNum<_min){
            _feeNum = _min;
        }
        emit OrderIncomeNote(msg.sender,tokenIn,amount,_feeNum);
    }

    function noteTeamIncome(address tokenIn,uint256 amount) external  payable {
        uint256 _gas = msg.value;
        require(_gas >= _noteFee,"LSD:INVALID_AMOUNT");
        TransferHelper.safeTransferETH(_feeRecive,_gas);
        Fee memory fee = feeMap[tokenIn];
        uint256 _min = fee.min;
        uint256 rate = fee.rate;
        require(amount >= _min,"LSD : LESS THAN MIN NUM");
        uint256 _feeNum = amount * rate / 100;
        if(_feeNum<_min){
            _feeNum = _min;
        }
        emit TeamIncomeNote(msg.sender,tokenIn,amount,_feeNum);
    }

    function noteRedemption(address token,uint256 amount) payable external {
        uint256 _fee = msg.value;
        require(_fee>=_noteFee,"LSD:INVALID_AMOUNT");
        TransferHelper.safeTransferETH(_feeRecive,_fee);
        emit RedemptionNote(msg.sender,token,amount);
    }

    function redemption(uint256 id,address token,address recive,uint256 amount,bytes memory signature) external {
        uint256 nonce = _principalnonce[recive][token];
        bytes32 message = ECDSA.toEthSignedMessageHash(keccak256(abi.encodePacked(address(this),id,nonce,token,amount,recive)));
        require(ECDSA.recover(message, signature) == _key,"LSD: SIGN ERROR");
        _principalnonce[recive][token] = _principalnonce[recive][token]+1;
        if(token == address(0)){
            TransferHelper.safeTransferETH(recive,amount);
        }else{
            TransferHelper.safeTransfer(token,recive,amount);
        }
        emit Redemption(id);
    }

    function orderIncome(uint256 id,address tokenIn,uint256 amount,uint256 feeNum,address recive,bytes memory signature) external {  
        uint256 nonce = _orderincomenonce[recive][tokenIn];
        bytes32 message = ECDSA.toEthSignedMessageHash(keccak256(abi.encodePacked(address(this),id,nonce,"ORDER_INCOME",tokenIn,amount,feeNum,recive)));
        require(ECDSA.recover(message, signature) == _key,"LSD: SIGN ERROR");
        _orderincomenonce[recive][tokenIn] = _orderincomenonce[recive][tokenIn]+1;
        _income(tokenIn,recive,amount,feeNum);
        emit Income(id);
    }

    function teamIncome(uint256 id,address tokenIn,uint256 amount,uint256 feeNum,address recive,bytes memory signature) external {
        uint256 nonce = _teamincomenonce[recive][tokenIn];
        bytes32 message = ECDSA.toEthSignedMessageHash(keccak256(abi.encodePacked(address(this),id,nonce,"TEAM_INCOME",tokenIn,amount,feeNum,recive)));
        require(ECDSA.recover(message, signature) == _key,"LSD: SIGN ERROR");
        _teamincomenonce[recive][tokenIn] = _teamincomenonce[recive][tokenIn]+1;
        _income(tokenIn,recive,amount,feeNum);
        emit Income(id);
    }

    function _income(address tokenIn,address recive,uint256 amount,uint256 feeNum) internal {
         if(tokenIn == address(0)){
            IWETH9(_router.WETH()).deposit{value:feeNum}();
            TransferHelper.safeTransferETH(recive,amount - feeNum);
        }else{
            TransferHelper.safeTransfer(tokenIn,recive,amount - feeNum);
        }
        address[] memory path;
        if(tokenIn != _usdt){
            path = new address[](3);
            path[0] = tokenIn == address(0)?_router.WETH():tokenIn;
            path[1] = _usdt;
            path[2] = _lsd; 
        }else{
            path = new address[](2);
            path[0] = _usdt;
            path[1] = _lsd; 
        }
        _approveTokenIfNeeded(path[0],feeNum);
        _router.swapExactTokensForTokens(feeNum,0, path, address(this),block.timestamp);
        IERC20(_lsd).burn(IERC20(_lsd).balanceOf(address(this)));
    }

    function _approveTokenIfNeeded(address _token, uint256 _swapAmountIn) private {
        if (IERC20(_token).allowance(address(this), address(_router)) < _swapAmountIn) {
            // Reset to 0
            TransferHelper.safeApprove(_token,address(_router), 0);
            // Re-approve
            TransferHelper.safeApprove(_token,address(_router), type(uint256).max);
        }
    }
}