// File: @openzeppelin/contracts@4.9.3/utils/Context.sol




pragma solidity ^0.8.0;


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// File: @openzeppelin/contracts@4.9.3/access/Ownable.sol



pragma solidity ^0.8.0;



abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        
    }

    function transferOwnership(address newOwner) external  virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

// File: @openzeppelin/contracts@4.9.3/token/ERC20/IERC20.sol



pragma solidity ^0.8.0;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);
    
}

// File: @openzeppelin/contracts@4.9.3/token/ERC20/extensions/IERC20Metadata.sol



pragma solidity ^0.8.0;



interface IERC20Metadata is IERC20 {

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

// File: @openzeppelin/contracts@4.9.3/token/ERC20/ERC20.sol




pragma solidity ^0.8.0;





contract ERC20 is Context, IERC20, IERC20Metadata {

     address private _contractOwner;

     event ContractOwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
        _contractOwner = msg.sender;
        _transferContractOwnership(_msgSender());
    }

    modifier onlyContractOwner() {
        _checkContractOwner();
        _;
    }

    function contractOwner() public view virtual returns (address) {
        return _contractOwner;
    }

    function _checkContractOwner() internal view virtual {
        require(contractOwner() == _msgSender(), "Ownable: caller is not the owner");
        
    }

    function transferContractOwnership(address newContractOwner) external  virtual onlyContractOwner {
        require(newContractOwner != address(0), "Ownable: new owner is the zero address");
        _transferContractOwnership(newContractOwner);
    }

    function _transferContractOwnership(address newContractOwner) internal virtual {
        address oldContractOwner = _contractOwner;
        _contractOwner = newContractOwner;
        emit ContractOwnershipTransferred(oldContractOwner, newContractOwner);
    }
    
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

      uint256 commission;
      uint256 burning;

    modifier commissions(){
         if(_totalSupply >= 10240000000 * 10 ** decimals()){
            commission=500;
        } else if(_totalSupply >= 5120000000 * 10 ** decimals()){
            commission=400;
        } else if(_totalSupply >= 2560000000 * 10 ** decimals()){
            commission=350;
        } else if(_totalSupply >= 1280000000 * 10 ** decimals()){
            commission=300;
        } else if(_totalSupply >= 640000000 * 10 ** decimals()){
            commission=250;
        } else if(_totalSupply >= 320000000 * 10 ** decimals()){
            commission=200;
        } else if(_totalSupply >= 160000000 * 10 ** decimals()){
            commission=150;
        } else if(_totalSupply >= 80000000 * 10 ** decimals()){
            commission=100;
        } else if(_totalSupply >= 40000000 * 10 ** decimals()){
            commission=50;
        } else if(_totalSupply > 20000000 * 10 ** decimals()){     
            commission=25;
        } else if(_totalSupply <= 20000000 * 10 ** decimals()){     
            commission=10;
        }
        _;
    }

    modifier mBurning(){
         if(_totalSupply >= 640000000 * 10 ** decimals()){
            burning=500;
        } else if(_totalSupply >= 320000000 * 10 ** decimals()){
            burning=400;
        } else if(_totalSupply >= 160000000 * 10 ** decimals()){
            burning=300;
        } else if(_totalSupply >= 40000000 * 10 ** decimals()){
            burning=200;
        } else if(_totalSupply > 20000000 * 10 ** decimals()){
            burning=100;
        } 
        _;
    }

    function transfer(address to, uint256 amount) public commissions mBurning virtual override returns (bool) {
        address owner = _msgSender();
        _burn(_msgSender(), amount * burning / 100000);
        _transfer(owner, to, amount-((amount * burning /100000)+(amount * commission /100000)));
        _transfer(owner, _contractOwner, amount * commission / 100000);
        return true;
    }


    function _transfer(address from, address to, uint256 amount ) internal  virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        
        _beforeTokenTransfer(from, to, amount );
        
        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
           
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);
       
        _afterTokenTransfer(from, to, amount );

    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }


    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            _totalSupply -= amount;
        }
        require(_totalSupply >= 20000000 * 10 ** decimals(), "The remainder of coins after burning is less than the minimum acceptable value");

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }
    
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}
    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}
// File: @openzeppelin/contracts@4.9.3/token/ERC20/extensions/ERC20Burnable.sol




pragma solidity ^0.8.0;




abstract contract ERC20Burnable is Context, ERC20 {

    function burn(uint256 amount) public virtual {
        _burn(_msgSender(), amount);
    }

}

// File: TIF.sol


pragma solidity ^0.8.9;




/// @custom:security-contact security@tiftoken.com
contract ThisIsFine is ERC20, ERC20Burnable, Ownable {
    constructor() ERC20("This Is Fine", "TIF") {
        _mint(msg.sender, 20480000000 * 10 ** decimals());
    }
}