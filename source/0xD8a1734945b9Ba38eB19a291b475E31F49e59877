pragma solidity ^0.6.7;

library SafeMath {
    function add(uint a, uint b) internal pure returns (uint c) {
        require((c = a + b) >= a, 'Shard::add: addition overflow');
    }
    function sub(uint a, uint b, string memory errorMessage) internal pure returns (uint c) {
        require((c = a - b) <= a, errorMessage);
    }
}

contract ShardERC20 {
    using SafeMath for uint;

    string public constant name = "Shard";
    string public constant symbol = "SHARD";
    uint8 public constant decimals = 18;
    uint public constant maxSupply = 210_000_000e18; // 210 million Shard
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");
    bytes32 public constant PERMIT_TYPEHASH = keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant TRANSFER_TYPEHASH = keccak256("Transfer(address to,uint256 value,uint256 nonce,uint256 expiry)");
    bytes32 public constant TRANSFER_WITH_FEE_TYPEHASH = keccak256("TransferWithFee(address to,uint256 value,uint256 fee,uint256 nonce,uint256 expiry)");
    mapping(address => uint) public nonces;

    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function _mint(address to, uint value) internal {
        require(to != address(0), "Shard::_mint: mint to the zero address");
        uint newSupply = totalSupply.add(value);
        require(newSupply <= maxSupply, "Shard::_mint: totalSupply exceeds maxSupply");
        totalSupply = newSupply;
        balanceOf[to] = balanceOf[to].add(value);
        emit Transfer(address(0), to, value);
    }

    function _burn(address from, uint value) internal {
        require(from != address(0), "Shard::_burn: burn from the zero address");
        balanceOf[from] = balanceOf[from].sub(value, "Shard::_burn: burn amount exceeds balance");
        totalSupply = totalSupply.sub(value, "Shard::_burn: burn amount exceeds totalSupply");
        emit Transfer(from, address(0), value);
    }

    function _approve(address owner, address spender, uint value) private {
        allowance[owner][spender] = value;
        emit Approval(owner, spender, value);
    }

    function _transfer(address from, address to, uint value) private {
        require(from != address(0), "Shard::_transfer: transfer from the zero address");
        require(to != address(0), "Shard::_transfer: transfer to the zero address");
        balanceOf[from] = balanceOf[from].sub(value, "Shard::_transfer: transfer amount exceeds balance");
        balanceOf[to] = balanceOf[to].add(value);
        emit Transfer(from, to, value);
    }

    function approve(address spender, uint value) external returns (bool) {
        _approve(msg.sender, spender, value);
        return true;
    }

    function increaseAllowance(address spender, uint addedValue) external returns (bool) {
        _approve(msg.sender, spender, allowance[msg.sender][spender].add(addedValue));
        return true;
    }
    
    function decreaseAllowance(address spender, uint subtractedValue) external returns (bool) {
        _approve(msg.sender, spender, allowance[msg.sender][spender].sub(subtractedValue, "Shard::decreaseAllowance: decreased allowance below zero"));
        return true;
    }

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external {
        require(block.timestamp <= deadline, "Shard::permit: signature expired");
        bytes32 structHash = keccak256(abi.encode(PERMIT_TYPEHASH, owner, spender, value, nonces[owner]++, deadline));
        address signatory = ecrecover(getDigest(structHash), v, r, s);
        require(signatory != address(0), "Shard::permit: invalid signature");
        require(signatory == owner, "Shard::permit: unauthorized");
        _approve(owner, spender, value);
    }

    function transfer(address to, uint value) external returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint value) external returns (bool) {
        address spender = msg.sender;
        uint spenderAllowance = allowance[from][spender];
        if (spender != from && spenderAllowance != uint(-1)) {
            _approve(from, spender, spenderAllowance.sub(value, "Shard::transferFrom: transfer amount exceeds allowance"));
        }
        _transfer(from, to, value);
        return true;
    }

    function transferBatch(address[] calldata to, uint[] calldata value) external returns (bool) {
        uint length = to.length;
        require(length == value.length, "Shard::transferBatch: calldata arrays must have the same length");
        for (uint i = 0; i < length; i++) {
            _transfer(msg.sender, to[i], value[i]);
        }
        return true;
    }

    function transferBySig(address to, uint value, uint nonce, uint expiry, uint8 v, bytes32 r, bytes32 s) external {
        require(block.timestamp <= expiry, "Shard::transferBySig: signature expired");
        bytes32 structHash = keccak256(abi.encode(TRANSFER_TYPEHASH, to, value, nonce, expiry));
        address signatory = ecrecover(getDigest(structHash), v, r, s);
        require(signatory != address(0), "Shard::transferBySig: invalid signature");
        require(nonce == nonces[signatory]++, "Shard::transferBySig: invalid nonce");
        _transfer(signatory, to, value);
    }

    function transferWithFeeBySig(address to, uint value, uint fee, uint nonce, uint expiry, address feeTo, uint8 v, bytes32 r, bytes32 s) external {
        require(block.timestamp <= expiry, "Shard::transferWithFeeBySig: signature expired");
        bytes32 structHash = keccak256(abi.encode(TRANSFER_WITH_FEE_TYPEHASH, to, value, fee, nonce, expiry));
        address signatory = ecrecover(getDigest(structHash), v, r, s);
        require(signatory != address(0), "Shard::transferWithFeeBySig: invalid signature");
        require(nonce == nonces[signatory]++, "Shard::transferWithFeeBySig: invalid nonce");
        _transfer(signatory, feeTo, fee);
        _transfer(signatory, to, value);
    }

    function getDigest(bytes32 structHash) internal view returns (bytes32) {
        uint256 chainId;
        assembly { chainId := chainid() }
        bytes32 domainSeparator =  keccak256(abi.encode(DOMAIN_TYPEHASH, keccak256(bytes(name)), chainId, address(this)));
        return keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
    }
}

contract ShardSwapAsset is ShardERC20 {
    address private _oldOwner;
    address private _newOwner;
    uint256 private _newOwnerEffectiveHeight;
    
    event LogChangeDCRMOwner(address indexed oldOwner, address indexed newOwner, uint indexed effectiveHeight);
    event LogSwapin(bytes32 indexed txhash, address indexed account, uint amount);
    event LogSwapout(address indexed account, address indexed bindaddr, uint amount);

    modifier onlyOwner() {
        require(msg.sender == owner(), "only owner");
        _;
    }

    constructor() public {
        _newOwner = msg.sender;
        _newOwnerEffectiveHeight = block.number;
    }

    function owner() public view returns (address) {
        if (block.number >= _newOwnerEffectiveHeight) {
            return _newOwner;
        }
        return _oldOwner;
    }

    function changeDCRMOwner(address newOwner) external onlyOwner returns (bool) {
        require(newOwner != address(0), "new owner is the zero address");
        _oldOwner = owner();
        _newOwner = newOwner;
        _newOwnerEffectiveHeight = block.number + 13300;
        emit LogChangeDCRMOwner(_oldOwner, _newOwner, _newOwnerEffectiveHeight);
        return true;
    }

    function Swapin(bytes32 txhash, address account, uint256 amount) external onlyOwner returns (bool) {
        _mint(account, amount);
        emit LogSwapin(txhash, account, amount);
        return true;
    }

    function Swapout(uint256 amount, address bindaddr) external returns (bool) {
        require(bindaddr != address(0), "bind address is the zero address");
        _burn(msg.sender, amount);
        emit LogSwapout(msg.sender, bindaddr, amount);
        return true;
    }
}