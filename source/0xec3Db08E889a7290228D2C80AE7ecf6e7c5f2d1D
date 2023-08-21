// SPDX-License-Identifier: Public domain

pragma solidity ^0.8.4;


interface IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    function nonces(address owner) external view returns (uint256);
    function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external;
    function transferWithPermit(address owner, address to, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IWrappedERC20 is IERC20 {
    function underlying() external view returns (IERC20);

    function depositWithPermit(address owner, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s, address to) external returns (uint);
    function depositWithTransferPermit(address owner, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s, address to) external returns (uint);
    
    function deposit() external returns (uint);
    function deposit(uint amount) external returns (uint);
    function deposit(uint amount, address to) external returns (uint);

    function withdraw() external returns (uint);
    function withdraw(uint amount) external returns (uint);
    function withdraw(uint amount, address to) external returns (uint);
}


contract WrappedERC20 is IWrappedERC20 {

    using SafeERC20 for IERC20;

    IERC20 public immutable underlying;

    bytes32 public constant PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant TRANSFER_TYPEHASH = keccak256("Transfer(address owner,address to,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public immutable DOMAIN_SEPARATOR;

    uint256 public totalSupply;
    mapping (address => uint256) public nonces;
    mapping (address => uint256) public balanceOf;
    mapping (address => mapping (address => uint256)) public allowance;


    constructor(IERC20 _underlying) {
        underlying = _underlying;
        require(_underlying != IERC20(address(0x0)), "Underlying is zero");
        require(0 < address(_underlying).code.length, "Underlying is non-contract");

        // Ensure no throw:
        _underlying.name();
        _underlying.symbol();
        _underlying.decimals();

        uint256 chainId;
        assembly {chainId := chainid()}
        DOMAIN_SEPARATOR = keccak256(
            abi.encode(
                keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
                keccak256(bytes(_underlying.name())),
                keccak256(bytes("53")),
                chainId,
                address(this)));
    }

    function name() external view returns (string memory) {
        return string.concat("Wrapped ", underlying.name());
    }

    function symbol() external view returns (string memory) {
        return underlying.symbol();
    }

    function decimals() external view returns (uint8) {
        return underlying.decimals();
    }


    function depositWithPermit(address owner, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s, address to) external returns (uint) {
        underlying.permit(owner, address(this), value, deadline, v, r, s);
        underlying.safeTransferFrom(owner, address(this), value);
        return _deposit(value, to);
    }

    function depositWithTransferPermit(address owner, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s, address to) external returns (uint) {
        underlying.transferWithPermit(owner, address(this), value, deadline, v, r, s);
        return _deposit(value, to);
    }

    function deposit() external returns (uint) {
        uint amount = underlying.balanceOf(msg.sender);
        underlying.safeTransferFrom(msg.sender, address(this), amount);
        return _deposit(amount, msg.sender);
    }

    function deposit(uint amount) external returns (uint) {
        underlying.safeTransferFrom(msg.sender, address(this), amount);
        return _deposit(amount, msg.sender);
    }

    function deposit(uint amount, address to) external returns (uint) {
        underlying.safeTransferFrom(msg.sender, address(this), amount);
        return _deposit(amount, to);
    }

    function _deposit(uint amount, address to) internal returns (uint) {
        _mint(to, amount);
        return amount;
    }


    function withdraw() external returns (uint) {
        return _withdraw(msg.sender, balanceOf[msg.sender], msg.sender);
    }

    function withdraw(uint amount) external returns (uint) {
        return _withdraw(msg.sender, amount, msg.sender);
    }

    function withdraw(uint amount, address to) external returns (uint) {
        return _withdraw(msg.sender, amount, to);
    }

    function _withdraw(address from, uint amount, address to) internal returns (uint) {
        _burn(from, amount);
        underlying.safeTransfer(to, amount);
        return amount;
    }


    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");

        totalSupply += amount;
        balanceOf[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");

        balanceOf[account] -= amount;
        totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    }


    function approve(address spender, uint256 value) external returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function permit(address owner, address spender, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external {
        require(block.timestamp <= deadline, "Expired permit");

        bytes32 hashStruct = keccak256(
            abi.encode(
                PERMIT_TYPEHASH,
                owner,
                spender,
                value,
                nonces[owner]++,
                deadline));

        require(verifyEIP712(owner, hashStruct, v, r, s));

        allowance[owner][spender] = value;
        emit Approval(owner, spender, value);
    }

    function transferWithPermit(address owner, address to, uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external returns (bool) {
        require(block.timestamp <= deadline, "Expired permit");

        bytes32 hashStruct = keccak256(
            abi.encode(
                TRANSFER_TYPEHASH,
                owner,
                to,
                value,
                nonces[owner]++,
                deadline));

        require(verifyEIP712(owner, hashStruct, v, r, s));

        _transferFrom(owner, to, value);
        return true;
    }

    function verifyEIP712(address owner, bytes32 hashStruct, uint8 v, bytes32 r, bytes32 s) internal view returns (bool) {
        bytes32 hash = keccak256(
            abi.encodePacked(
                "\x19\x01",
                DOMAIN_SEPARATOR,
                hashStruct));
        address signer = ecrecover(hash, v, r, s);
        
        // ecrecover returns the zero address on recover failure, so we need to handle that explicitly.
        return signer != address(0) && signer == owner;
    }

    function transfer(address to, uint256 value) external returns (bool) {
        _transferFrom(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        if (from != msg.sender) {
            uint256 allowed = allowance[from][msg.sender];

            if (allowed != type(uint256).max) {
                require(value <= allowed, "request exceeds allowance");

                uint256 reduced = allowed - value;
                allowance[from][msg.sender] = reduced;
                
                emit Approval(from, msg.sender, reduced);
            }
        }

        _transferFrom(from, to, value);
        return true;
    }

    function _transferFrom(address from, address to, uint256 value) internal {
        require(to != address(0) && to != address(this));

        uint256 balance = balanceOf[from];
        require(value <= balance, "transfer amount exceeds balance");
        
        balanceOf[from] = balance - value;
        balanceOf[to] += value;
        
        emit Transfer(from, to, value);
    }
}


library SafeERC20 {
    error Inner(string comment, bytes innerError);

    function safeTransfer(IERC20 token, address to, uint value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function callOptionalReturn(IERC20 token, bytes memory data) private {
        (bool success, bytes memory returndata) = address(token).call(data);
        if (!success) {
            revert Inner("SafeERC20: low-level call failed", returndata);
        }
        
        if (0 < returndata.length) { // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}