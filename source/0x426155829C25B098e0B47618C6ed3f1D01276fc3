/**
 *Submitted for verification at testnetstage.bscscan.com on 2023-09-11
*/

// SPDX-License-Identifier: UNLICENSED

pragma solidity >=0.8.2;

library SafeMath {
    function add(uint x, uint y) internal pure returns (uint z) {
        require((z = x + y) >= x, 'ds-math-add-overflow');
    }

    function sub(uint x, uint y) internal pure returns (uint z) {
        require((z = x - y) <= x, 'ds-math-sub-underflow');
    }

    function mul(uint x, uint y) internal pure returns (uint z) {
        require(y == 0 || (z = x * y) / y == x, 'ds-math-mul-overflow');
    }
}

interface CrosschainERC20 {
    function mint(address to, uint256 amount) external returns (bool);
    function burn(address from, uint256 amount) external returns (bool);
    function changeVault(address newVault) external returns (bool);
    function depositVault(uint amount, address to) external returns (uint);
    function withdrawVault(address from, uint amount, address to) external returns (uint);
    function underlying() external view returns (address);
    function deposit(uint amount, address to) external returns (uint);
    function withdraw(uint amount, address to) external returns (uint);
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function permit(address target, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external;
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transferWithPermit(address target, address to, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external returns (bool);

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

contract CrosschainRouter {

    using SafeERC20 for IERC20;
    using SafeMath for uint;
    address private _mpc;
    mapping(bytes32 =>uint256) private transactionHistory;

    modifier onlyMPC() {
        require(msg.sender == mpc(), "XStringSwapRouter: FORBIDDEN");
        _;
    }

    event LogChangeMPC(address indexed oldMPC, address indexed newMPC,uint chainID);
    event LogSwapIn(bytes32 indexed txhash, address indexed token, address indexed to, uint amount, uint fromChainID, uint toChainID,uint fee);
    event LogSwapOut(address indexed token, address indexed from, address indexed to, uint amount, uint fromChainID, uint toChainID);

    function initialize(address __mpc) external{
        require(mpc()==address(0),"already initialized"); 
        _mpc = __mpc;
    }

    function _crosschainSwapOut(address from, address token, address to, uint amount, uint toChainID) internal {
        CrosschainERC20(token).burn(from, amount);
        emit LogSwapOut(token, from, to, amount, cID(), toChainID);
    }

    // Swaps `amount` `token` from this chain to `toChainID` chain with recipient `to`
    function SwapOut(address token, address to, uint amount, uint toChainID) external {
        _crosschainSwapOut(msg.sender, token, to, amount, toChainID);
    }

    // Swaps `amount` `token` from this chain to `toChainID` chain with recipient `to` by minting with `underlying`
    function SwapOutUnderlying(address token, address to, uint amount, uint toChainID) external {
        IERC20(CrosschainERC20(token).underlying()).safeTransferFrom(msg.sender, token, amount);
        CrosschainERC20(token).depositVault(amount, msg.sender);
        _crosschainSwapOut(msg.sender, token, to, amount, toChainID);
    }


    function SwapOutUnderlyingWithPermit(
        address from,
        address token,
        address to,
        uint amount,
        uint deadline,
        uint8 v,
        bytes32 r,
        bytes32 s,
        uint toChainID
    ) external {
        address _underlying = CrosschainERC20(token).underlying();
        IERC20(_underlying).permit(from, address(this), amount, deadline, v, r, s);
        IERC20(_underlying).safeTransferFrom(from, token, amount);
        CrosschainERC20(token).depositVault(amount, from);
        _crosschainSwapOut(from, token, to, amount, toChainID);
    }

    function SwapOutUnderlyingWithTransferPermit(
        address from,
        address token,
        address to,
        uint amount,
        uint deadline,
        uint8 v,
        bytes32 r,
        bytes32 s,
        uint toChainID
    ) external {
        IERC20(CrosschainERC20(token).underlying()).transferWithPermit(from, token, amount, deadline, v, r, s);
        CrosschainERC20(token).depositVault(amount, from);
        _crosschainSwapOut(from, token, to, amount, toChainID);
    }


    function _crosschainSwapIn(bytes32 txs, address token, address to, uint amount, uint fromChainID, uint fee) internal {
        require(!isTransactionExist(txs),"Router:: Transaction already processed!");
        CrosschainERC20(token).mint(to, amount-fee);
        transactionHistory[txs] = fromChainID;
        emit LogSwapIn(txs, token, to, amount, fromChainID, cID(),fee);
    }

    function isTransactionExist(bytes32 txs) public view returns (bool) {
        return transactionHistory[txs]!=0;
    }

    // swaps `amount` `token` in `fromChainID` to `to` on this chainID
    // triggered by `SwapOut`
    function SwapIn(bytes32 txs, address token, address to, uint amount, uint fromChainID,uint fee) external onlyMPC {
        _crosschainSwapIn(txs, token, to, amount, fromChainID, fee);
    }

    // swaps `amount` `token` in `fromChainID` to `to` on this chainID with `to` receiving `underlying`
    function SwapInUnderlying(bytes32 txs, address token, address to, uint amount, uint fromChainID,uint fee) external onlyMPC {
        _crosschainSwapIn(txs, token, to, amount, fromChainID,fee);
        CrosschainERC20(token).withdrawVault(to, amount-fee, to);
        CrosschainERC20(token).withdrawVault(mpc(), fee, mpc());
    }

    // swaps `amount` `token` in `fromChainID` to `to` on this chainID with `to` receiving `underlying` if possible
    function SwapInAuto(bytes32 txs, address token, address to, uint amount, uint fromChainID,uint fee) external onlyMPC {
        _crosschainSwapIn(txs, token, to, amount, fromChainID, fee);
        CrosschainERC20 _crosschainToken = CrosschainERC20(token);
        address _underlying = _crosschainToken.underlying();
        if (_underlying != address(0) && IERC20(_underlying).balanceOf(token) >= amount) {
            _crosschainToken.withdrawVault(to, amount-fee, to);
            _crosschainToken.withdrawVault(mpc(), fee, mpc());
        }
    }

    function changeMPC(address newMPC) public onlyMPC returns (bool) {
        require(newMPC != address(0), "SwapRouter: address(0x0)");
        address _oldMPC = mpc();
        _mpc = newMPC;
       
        emit LogChangeMPC(_oldMPC, _mpc,  cID());
        return true;
    }

    function changeVault(address token, address newVault) public onlyMPC returns (bool) {
        require(newVault != address(0), "SwapRouter: address(0x0)");
        return CrosschainERC20(token).changeVault(newVault);
    }

    function mpc() public view returns (address) {
        return _mpc;
    }

    function cID() public view returns (uint id) {
        assembly {id := chainid()}
    }

}