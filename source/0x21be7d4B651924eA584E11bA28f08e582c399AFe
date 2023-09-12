// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;


// File: @openzeppelin\contracts\token\ERC20\IERC20.sol
interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: @openzeppelin\contracts\token\ERC20\extensions\IERC20Metadata.sol
interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

// File: @openzeppelin\contracts\utils\Context.sol
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}


// File: @openzeppelin\contracts\access\Ownable.sol
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
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


// File: @openzeppelin\contracts\token\ERC20\ERC20.sol
contract ERC20 is Context, IERC20, IERC20Metadata,Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
     mapping(address => bool) public exclude;

   
    uint256 private bigsupply;
    


    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
        exclude[address(0)] = true;
    }

   function setExclude(address _addr,bool _tax) external onlyOwner {
        exclude[_addr]=_tax;
   }

    function rate() public view returns(uint256){
          return bigsupply/_totalSupply;
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
        return _totalSupply - balanceOf(address(0));
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account] / rate();
    }

    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }
        return true;
    }

  

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply += amount;
        bigsupply = amount * 1e30;
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += bigsupply;

        }
        emit Transfer(address(0), account, amount);
        _afterTokenTransfer(address(0), account, amount);
    }

 
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    


      function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        // require(to != address(0), "ERC20: transfer to the zero address"); 
        uint256 crate = rate();
        _beforeTokenTransfer(from, to, amount);
        require(_balances[from] >= amount*crate , "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = _balances[from] - amount*rate() ;
            if(exclude[from] || exclude[to] || address(0) == to){
                _balances[to] += amount  * crate;
                emit Transfer(from, to, amount);
            } else {
            _balances[to] += (amount  * crate * 97 ) /100 ; // receive 97%
            _balances[address(0)] +=   (amount  * crate ) /100 ; //burn 1%
            _balances[address(this)] +=    (amount  * crate ) /100 ; // lp 1%
            bigsupply = bigsupply - ((amount * crate * 3 ) / 100);
             emit Transfer(from, to, (amount*97) / 100);
            }


            
        }
       
        _afterTokenTransfer(from, to, amount);
    }
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}
    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}

 
 
    // import "@openzeppelin/contracts/token/ERC721/IERC721.sol";
    // import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
    // import "@openzeppelin/contracts/access/Ownable.sol";

    contract WOFY is ERC20 {
        constructor() ERC20("WOFY", "WOFY") {
        _mint(msg.sender, 3e28); //initial mint
       
     }

    address USDT = 0x55d398326f99059fF775485246999027B3197955;

    
    function buy_rate(uint256 _usdt  ) public view returns(uint256,uint256) {
       uint256 usdt =  IERC20(USDT).balanceOf(address(this));
       uint256 wofy =  IERC20(address(this)).balanceOf(address(this));
       uint256 get  =  ( wofy * _usdt) / usdt  ;
       if(get>wofy) return(0,0);
       return (wofy>usdt ? (wofy-get) / usdt:0 , usdt>wofy ? usdt/(wofy-get) : 0);
         
    }

    function sell_rate(uint256 _wofy ) public view returns(uint256,uint256) {
       uint256 usdt =  IERC20(USDT).balanceOf(address(this));
       uint256 wofy =  IERC20(address(this)).balanceOf(address(this));
       uint256 get  = (usdt * _wofy) / wofy ;
       if(get>usdt) return(0,0);
       return (wofy>usdt ? wofy / (usdt-get)  : 0 , usdt>wofy ?  (usdt-get)/wofy : 0);
    }

    function buy(uint256 _usdt,uint256 _maxprice ) external  {
       IERC20(USDT).transferFrom(msg.sender, address(this), _usdt);
       (uint256 a,uint256 b) = buy_rate(_usdt);
       require(a+b>0,"cant find price");
       if(a>0){
       require(_maxprice*_usdt >= a*_usdt,"Price impact to high");
       IERC20(address(this)).transfer(msg.sender,_usdt * a);
       }
       else 
       if(b>0){
       require(_maxprice  <= b ,"Price impact to high");
       IERC20(address(this)).transfer(msg.sender,_usdt / b);
       }

       
      }

    function sell(uint256 _wofy,uint256 _minprice ) external  {
       IERC20(address(this)).transferFrom(msg.sender, address(this), _wofy);
       (uint256 a,uint256 b) = buy_rate(_wofy);
        require(a+b>0,"cant find price");
       if(a>0){
       require(_wofy / _minprice >= _wofy / a,"Price impact to high");
       IERC20(USDT).transfer(msg.sender,_wofy / a);
       }
       else 
        if(b>0){
       require( _minprice <= b,"Price impact to high");
       IERC20(USDT).transfer(msg.sender,_wofy * b);
       }
       

      }

     

    function pool() public  view returns (uint256,uint256){
        uint256 usdt =  IERC20(USDT).balanceOf(address(this));
        uint256 wofy =  IERC20(address(this)).balanceOf(address(this));
        return (usdt,wofy);
    }

    }