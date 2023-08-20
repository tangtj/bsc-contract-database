// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

// import "./TUIN.sol";
// import { IERC20 } from "./interface/IERC20.sol";

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function decimals() external view returns (uint8);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

/**
 * @title ERC20 Token
 * @dev   Implementation Tuin v1.Tuin v1 is an ERC20 Token. All controls and changes have been reserved for Tuin wallet.
 *        Holders token balances are increased and decreased as guided by this contract.
 */

contract TUIN is IERC20 {
    string  public constant  name        = "Tuin v1";
    string  public constant  symbol      = "TT";
    uint8   public constant  decimals    = 18;
    uint256 public totalSupply;
    address public owner;
    address public pool;

    /// @dev the specified token amount by surry for eth chain is 5 billion
    /// initial because tuin wallet is reserved the right to increase or decrease this supply
    uint256  public constant  init_maxsupply_on_eth = 5000000000 * 1e18;

    /// @dev the specified token amount by surry for bsc chain is 2 billion
    /// initial because tuin wallet is reserved the right to increase or decrease this supply
    uint256  public constant  init_maxsupply_on_bsc = 2000000000 * 1e18;

    uint256  public           maxsupply_on_eth      = init_maxsupply_on_eth;

    uint256  public           maxsupply_on_bsc      = init_maxsupply_on_bsc;

    uint256  public immutable deploymentChainId;

    /// @dev Records amount of Tuin token owned by an account.
    mapping ( address => uint256) private _balances;

    /// @dev Records number of TUIN tokens that account (spender) will be allowed to spend on behalf of another account (owner).
    mapping ( address => mapping (address => uint256)) private _allowed;

    /// @dev functions marked with onlyOwner can be called by only owner 
    modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }

    /// @dev functions marked with onlyPool can be called by only owner 
    modifier onlyPool {
        require(msg.sender == pool);
        _;
    }

    event OwnerSet(bool indexed success, address indexed newOwner);

    event PoolSet(bool indexed success, address indexed newPool);
    /**
     * @dev Constructor that gives poolContract initial max tokens. poolContract lists
     *      tokens on exchanges. 
     * @param _poolContract Address of poolcontract.
     */
    constructor(address _poolContract) {
        uint256 chainId;
        assembly {chainId := chainid()}
        deploymentChainId = chainId;
        owner = address(msg.sender);
        pool = _poolContract;


        if (chainId == 1) {
            totalSupply = init_maxsupply_on_eth;
            _balances[_poolContract] = init_maxsupply_on_eth;
        } else {
            totalSupply = init_maxsupply_on_bsc;
            _balances[_poolContract] = init_maxsupply_on_bsc;
        }
    }       

    /**
     * @dev   function that sets the owner of the TUIN contract to _surryWallet
     *        initially the contract owner is set to the contracts deployer address  
     * @param _newOwner Address of _surryWallet.
     */
    function setOwner(address _newOwner) onlyOwner() external returns (bool) {
        require(address(this) != _newOwner, "can't own self");

        owner = _newOwner;

        emit OwnerSet(true, _newOwner);

        return true;
    }

    function setPool(address _newPool) onlyOwner() external returns (bool) {
        pool = _newPool; 

        emit PoolSet(true, _newPool);

        return true;
    }
   
    /**
     * @dev Creates new tokens and assigns them to an address in this case should be the pool contract.
     *      check on chain doesn't use chain id, this is so contract can be adaptable to more chains mintable 
     *      by admin
     * @param account The address to which the tokens will be minted.
     * @param amount The amount of tokens to be minted.
     * @return A boolean value indicating whether the operation succeeded.
     */
    function mint(address account, uint256 amount) onlyOwner() external returns(bool)  {
        require(account != address(0), 'TUIN ERC20: mint to zero address');

        bool isEth = deploymentChainId == 1;

        uint256 newSupply = totalSupply + amount;
         
        // mint on ethereum chain
        if (isEth && newSupply <= maxsupply_on_eth) {
            _mint(account, amount);
        // else if mints on bnb
        } else if (!isEth && newSupply <= maxsupply_on_bsc) {
            _mint(account, amount);
        // specify the error
        } else {
            revert('ERROR: not right chain or attempt more than chain max supply');
        }
        
        emit Transfer(address(0), account, amount);

        return true;
    }


    function _mint(address account, uint256 amount) private {
        totalSupply += amount;
        _balances[account] += amount; 
    }


    /**
     * @dev    Burn tokens from the sender's account.
     *         check on chain doesn't use chain id, this is so contract can be adaptable to more chain burnable by pool redemption contract
     * @param  amount The amount of tokens to burn.
     * @return A boolean indicating whether the operation succeeded.
     */
    function burn(address account, uint256 amount) onlyPool() external returns (bool)  {
        require(account != address(0), 'TUIN ERC20: mint to zero address');

        require(amount <= _balances[account], 'burn amount exceeds balance');

        bool isEth = deploymentChainId == 1;

        // burn on ethereum chain
        if (isEth && totalSupply <= maxsupply_on_eth) {
            _burn(account, amount);
        // else if burn on bsc
        } else if (!isEth && totalSupply <= maxsupply_on_bsc) {
            _burn(account, amount);
        // specify the error
        } else {
            revert('ERROR: not right chain or exceeds chain max supply');
        }
        

        emit Transfer(msg.sender, address(0), amount);

        return true;
    }


    function _burn(address account, uint256 amount) private {
        _balances[account] -= amount; 
        totalSupply -= amount;
    }


    /**
     * @dev Burn tokens from a specified account, subject to allowance.
     * @param _from The address whose tokens will be burned.
     * @param _amount The amount of tokens to burn.
     * @return A boolean indicating whether the operation succeeded.
     */
    function burnFrom(
        address _from,
        uint256 _amount
    ) external returns (bool) {
        require(_from != address(0), 'ERC20: from address is not valid');

        require(_balances[_from] >= _amount, 'ERC20: insufficient balance');

        require(_amount <= _allowed[_from][msg.sender], 'ERC20: burn from value not allowed');

        bool isEth = deploymentChainId == 1;
        // burn on ethereum chain
        if (isEth && totalSupply <= maxsupply_on_eth) {
             _burnFrom(_from, _amount);
        // else if burn on bsc
        } else if (!isEth && totalSupply <= maxsupply_on_bsc) {
            _burnFrom(_from, _amount);
        // specify the error
        } else {
            revert('ERROR: not right chain or exceeds chain max supply');
        }

        emit Transfer(_from, address(0), _amount);

        return true;
    }


    function _burnFrom(address account, uint256 amount) private {
          _allowed[account][msg.sender] -= amount;
          _balances[account] -= amount;
          totalSupply -= amount;
    }


    /// @dev maxsupply_on_eth + maxsupplyon_bsc. this is the total amount of tokens that would ever be issued
    function maxSupply() external view returns (uint256) {
        return maxsupply_on_eth + maxsupply_on_bsc;
    }
    

    /// @dev Tuins supply on eth should only be changeable by the Tuins wallet.
    ///      restrict access using a modifier
    function changeSupplyOnEth(uint256 _new_maxsupply_on_eth) onlyOwner() external returns (bool) {
        require(totalSupply < _new_maxsupply_on_eth, "ERROR: value lesser than or equal to total supply");

        maxsupply_on_eth = _new_maxsupply_on_eth;

        return true;
    }


    /// @dev Tuins supply on bsc should only be changeable by the Tuins wallet.
    ///      restrict access using a modifier
    function changeSupplyOnBsc(uint256 _new_maxsupply_on_bsc) onlyOwner() external returns (bool) {
        require(totalSupply < _new_maxsupply_on_bsc, "ERROR: value lesser than or equal to total supply");

        maxsupply_on_bsc = _new_maxsupply_on_bsc;

        return true;
    }


    /**
    * @dev    Gets the balance of the specified address.
    * @param  _owner The address to query the balance of.
    * @return balance An uint256 representing the balance
    */
    function balanceOf(
       address _owner
     ) external view returns (uint256 balance) {
        return _balances[_owner];
    }


    /**
     * @dev     After a user purchases TUIN, a user must be able to transfer tuin token for a specified address.
     *          Tuin are redeemable 1:1 for profit
     * @param   _to The address to transfer to.
     * @param   _value The amount to be transferred.
     * @return  A boolean that indicates if the operation was successful.
     */
    function transfer(
        address _to, 
        uint256 _value
    ) external returns (bool) {
        require(_to != address(0), 'ERC20: to address is not valid');

        require(_value <= _balances[msg.sender], 'ERC20: insufficient balance');

        _balances[msg.sender] -= _value;

        _balances[_to] += _value;
        
        emit Transfer(msg.sender, _to, _value);
        
        return true;
    }


     /**
     * @dev    Approve the passed address to spend the specified amount of tokens on behalf of msg.sender.
     * @param  _spender The address which will spend the funds.
     * @param  _value The amount of tokens to be spent.
     * @return A boolean that indicates if the operation was successful.
     */
    function approve(
       address _spender, 
       uint256 _value
    ) external returns (bool) {
        _allowed[msg.sender][_spender] = _value;
        
        emit Approval(msg.sender, _spender, _value);
        
        return true;
   }


    /**
    * @dev    Transfer tokens from one address to another.
    * @param  _from The address which you want to send tokens from.
    * @param  _to The address which you want to transfer to.
    * @param  _value The amount of tokens to be transferred.
    * @return A boolean that indicates if the operation was successful
    */
   function transferFrom(
        address _from, 
        address _to, 
        uint256 _value
    ) external returns (bool) {
        require(_from != address(0), 'ERC20: from address is not valid');

        require(_to != address(0), 'ERC20: to address is not valid');

        require(_value <= _balances[_from], 'ERC20: insufficient balance');

        require(_value <= _allowed[_from][msg.sender], 'ERC20: transfer from value not allowed');

        _allowed[_from][msg.sender] -= _value;
        _balances[_from] -= _value;
        _balances[_to] += _value;
        
        emit Transfer(_from, _to, _value);
        
        return true;
   }


    /**
     * @dev Returns the amount of tokens approved by the owner that can be transferred to the spender's account.
     * @param _owner The address of the owner of the tokens.
     * @param _spender The address of the spender.
     * @return The number of tokens approved.
     */
    function allowance(
        address _owner, 
        address _spender
    ) external override view returns (uint256) {
        return _allowed[_owner][_spender];
    }


    /**
     * @dev    Increases the amount of tokens that an owner has allowed to a spender.
     * @param  _spender The address of the spender.
     * @param  _addedValue The amount of tokens to increase the allowance by.
     * @return A boolean value indicating whether the operation succeeded.
     */
    function increaseApproval(
        address _spender, 
        uint256 _addedValue
    ) external returns (bool) {
        _allowed[msg.sender][_spender] += _addedValue;

        emit Approval(msg.sender, _spender, _allowed[msg.sender][_spender]);
        
        return true;
    }


    /**
     * @dev Decreases the amount of tokens that an owner has allowed to a spender.
     * @param _spender The address of the spender.
     * @param _subtractedValue The amount of tokens to decrease the allowance by.
     * @return A boolean value indicating whether the operation succeeded.
     */
    function decreaseApproval(
        address _spender, 
        uint256 _subtractedValue
    ) external returns (bool) {
        uint256 oldValue = _allowed[msg.sender][_spender];
        
        if (_subtractedValue > oldValue) {
            _allowed[msg.sender][_spender] = 0;
        } else {
            _allowed[msg.sender][_spender] = oldValue - _subtractedValue;
        }
        
        emit Approval(msg.sender, _spender, _allowed[msg.sender][_spender]);
        
        return true;
    }


    function getChainMaxSupply() external view returns(uint256) {
       return (deploymentChainId == 1) ? maxsupply_on_eth : maxsupply_on_bsc;
    }
}



       
/**
 * @title TUINPool is an Interest-bearing ERC20-like pool for TUIN Payments.
 * @dev   Implementation Tuin Pool v1. Tuin pool backs Tuin Token and sets collateralization ratio.
 *        Initially the flow of transactions starts at TUIN Token contract which mints TUIN Token to 
 *        TUINPool.
 *  
 *
 *        TUIN Balances represents a holder's share in the total amount of yield derived by TUIN Payments.
 *        Tuin balances are transferable and hence redeemable by the current holder. Each Holders yield is 
 *        calculated with the eq:
 *
 *        Profit in this case is determined per the initial amount of TUIN Purchased
 */

 contract TUINPool  {
    address public owner;
  
    address public acceptedToken1;
    address public acceptedToken2;
    address public acceptedToken3; 
    address public yieldToken;
    
    // for book-keeping purposes
    uint256 public amountDepositedacceptedToken1;
    uint256 public amountDepositedacceptedToken2;
    uint256 public amountDepositedacceptedToken3;

    uint256 public amountWithdrawnacceptedToken1;
    uint256 public amountWithdrawnacceptedToken2;
    uint256 public amountWithdrawnacceptedToken3;

    uint256 public amountYieldDepositedacceptedToken;

    string  public redeemableDate;

    bool    public isApproved;

    bool    public isPaused;

    uint256 public exchangeRateTuin;

    uint256 public exchangeRateUsd; 

    uint256 public immutable deploymentChainId; 
    
// 
    /// @dev functions marked with onlyOwner can be called by only owner 
    modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }

    modifier Paused    {
        require(isPaused == false, "Err: contract is paused");
        _;
    }

    /**
     * @dev Constructor gives the contracts deployer initial ownership after which it can be set to 
     *      _surryWallet
     */
    constructor() {
        uint256 chainId;
        assembly {chainId := chainid()}
        deploymentChainId = chainId;
        owner = address(msg.sender);
    }

    /// @dev Set contract ownership, the initial ownership is set to the contract owner, and passed to _surrywallet
    function setOwner(address _surryWallet) onlyOwner() external returns (bool) {
        require(address(this) != _surryWallet, "cant own self");

        owner = _surryWallet;

        return true;
    }


    function setPaused(bool _isPaused) onlyOwner() external returns (bool) {
        isPaused = _isPaused;

        return true;
    }

    /// @dev Set accpeted token 1 that can be used to purchase TUIN tokens from pool. 
    /// @dev The contract is a representation of single sided liquidity pool.
    function setAcceptedToken1(address _token1) onlyOwner() external returns (bool) {
        acceptedToken1 = _token1;

        return true;
    }
 
    /// @dev Set accpeted token 2 that can be used to purchase TUIN tokens from pool. 
    /// @dev The contract is a representation of single sided liquidity pool.
    function setAcceptedToken2(address _token2) onlyOwner() external returns (bool) {
        acceptedToken2 = _token2;

        return true;
    }


    /// @dev Set accpeted token 3 that can be used to purchase TUIN tokens from pool. 
    /// @dev Initially accepted token 3 is set to a zero address in the constructor.
    /// @dev The contract is a representation of single sided liquidity pool.
    function setAcceptedToken3(address _token3) onlyOwner() external returns (bool) {
        acceptedToken3 = _token3;

        return true;
    }

    function setYieldToken(address _yieldToken) onlyOwner() external returns (bool) {
        require( _yieldToken  ==  acceptedToken1  ||  _yieldToken  ==  acceptedToken2  || _yieldToken  ==  acceptedToken3, "Err: Not accepted token" );
        
        yieldToken = _yieldToken;

        return true;
    }


    /// @dev Approve yield redemption for TUIN Token Holders.
    function approveYield(bool _isApproved) onlyOwner() external returns (bool) {
        isApproved = _isApproved;

        return true;
    }


    /// @dev Announce the set date for yield redemption.
    function setRedeemableDate(string memory _date) onlyOwner() external returns (bool) {
        redeemableDate = _date;

        return true;
    }

    /// @dev the exchange rate is as follows 
    ///      specify the amount of tuin equivalent to 1 usd 
    function setExchangeRateTuin(uint256 _rate) onlyOwner() external returns (bool) {
        if (_rate == 0) revert("rate should be greater than 0");
       
        exchangeRateTuin = _rate;

        return true;
    }

    /// @dev the exchange rate is as follows 
    ///      specify the amount of tuin equivalent to 1 usd 
    function setExchangeRateUsd(uint256 _rate) onlyOwner() external returns (bool) {
        if (_rate == 0) revert("rate should be greater than 0");
       
        exchangeRateUsd = _rate;

        return true;
    }

    /// @dev Get the pool's balance of tokenIn
    /// @dev This function is gas optimized to avoid a redundant extcodesize check in addition to the returndatasize
    /// check
    function balanceTknIn(address _tokenIn) public view returns (uint256) {
        (bool success, bytes memory data) =
            _tokenIn.staticcall(abi.encodeWithSelector(IERC20.balanceOf.selector, address(this)));

        require(success && data.length >= 32);

        return abi.decode(data, (uint256));
    }     

    function amountTuinOut(address _tokenIn, address _tokenOut, uint256 _amountIn) public view returns (uint256 _amountTuinOut) {
        (bool success0, bytes memory data0) =
            _tokenIn.staticcall(abi.encodeWithSelector(IERC20.decimals.selector));

        require(success0 && data0.length >= 32);

        uint256 decimalTokenIn = abi.decode(data0, (uint256));

        (bool success1, bytes memory data1) =
            _tokenOut.staticcall(abi.encodeWithSelector(IERC20.decimals.selector));

        require(success1 && data1.length >= 32);

        uint256 decimalTokenOut = abi.decode(data1, (uint256));

        uint256 scale;

        // scale down
        if (decimalTokenIn > decimalTokenOut) {
            scale = decimalTokenIn - decimalTokenOut;
            _amountTuinOut = _amountIn * exchangeRateTuin / 10 ** scale;
        }

        // scale up
        scale = decimalTokenOut - decimalTokenIn;

        _amountTuinOut = scale == 0 ? _amountIn * exchangeRateTuin : _amountIn * exchangeRateTuin * 10 ** scale;
    }

    function scaleYieldTo18(address _tokenIn, uint256 _amountIn) public view returns (uint256) {
        (bool success0, bytes memory data0) =
            _tokenIn.staticcall(abi.encodeWithSelector(IERC20.decimals.selector));

        require(success0 && data0.length >= 32);

        uint256 decimalTokenIn = abi.decode(data0, (uint256));

        uint256 scale;

        // we want the token to  be scaled to 18.

        // scale down
        if (decimalTokenIn > 18) {
            scale = decimalTokenIn - uint256(18);

            return _amountIn / 10 ** scale;
        }

        // scale up
        scale = uint256(18) - decimalTokenIn;

        

        return ( scale == 0 ) ? _amountIn : _amountIn * 10 ** scale;
    }

    // It takes in a token address and an 18 decimal amount, and scales the 18 decimal amount to the 
    // token addresses decimal
    function scaleYieldToDecimal(address _tokenIn, uint256 _amountIn) public view returns (uint256) {
        (bool success0, bytes memory data0) =
            _tokenIn.staticcall(abi.encodeWithSelector(IERC20.decimals.selector));

        require(success0 && data0.length >= 32);

        uint256 decimalTokenIn = abi.decode(data0, (uint256));

        uint256 scale;

        // scale up
        if (decimalTokenIn > 18) {
            scale = decimalTokenIn - uint256(18);

            return _amountIn * exchangeRateUsd * 10 ** scale;
        }

        // scale down
        scale = uint256(18) - decimalTokenIn;

        return ( scale == 0 ) ? _amountIn * exchangeRateUsd : (_amountIn * exchangeRateUsd) / (10 ** scale);
    }

    function recordAmountDepositedacceptedToken(address _tokenIn, uint256 _amountIn) private {
        if ( _tokenIn  ==  acceptedToken1 )  amountDepositedacceptedToken1  += _amountIn;

        if ( _tokenIn  ==  acceptedToken2 )  amountDepositedacceptedToken2  += _amountIn;

        if ( _tokenIn  ==  acceptedToken3 )  amountDepositedacceptedToken3  += _amountIn;
    }

    function recordamountWithdrawnacceptedToken(address _tokenIn, uint256 _amountIn) private {
        if ( _tokenIn  ==  acceptedToken1 )  amountWithdrawnacceptedToken1 += _amountIn;

        if ( _tokenIn  ==  acceptedToken2 )  amountWithdrawnacceptedToken2  += _amountIn;

        if ( _tokenIn  ==  acceptedToken3 )  amountWithdrawnacceptedToken3  += _amountIn;
    }

    // yield token has to be one of the deposited yield tokens
    function recordamountYieldDepositedacceptedToken(address _tokenIn, uint256 _amountIn) private {
        _amountIn = scaleYieldTo18(_tokenIn, _amountIn);
        
        // tokens are added with 18 decimals
        amountYieldDepositedacceptedToken  += _amountIn;
    }

    // tests swapIn line by line
    // checkSwapInTokenEqAcceptedTkn
    function swapIn(uint256 _amountIn, address _tokenIn, address _tokenOut) Paused() external returns (bool)  {
        require( _amountIn  >  0, "ERR: _amountIn is 0" );

        require( _tokenIn  ==  acceptedToken1  ||  _tokenIn  ==  acceptedToken2  ||  _tokenIn  ==  acceptedToken3, "ERR: Not accepted token" );

        require( acceptedToken1  !=  address(0)  ||  acceptedToken2  !=  address(0)  ||  acceptedToken3  !=  address(0),  "ERR: Accepted tokens not set" );

        require( _tokenIn  !=  address(0) ,  "ERR: TokenIn address(0)" );

        // balance of tokenIn held by this contract
        uint256 balanceBefore = balanceTknIn(_tokenIn);

        // user transfer to this contract
        (bool success1, bytes memory data1) = _tokenIn.call(abi.encodeWithSelector(IERC20.transferFrom.selector, msg.sender, address(this), _amountIn));

        require( success1  &&  data1.length  >=  32, "Err: Couldn't TransferFrom" );

        // balance of tokenIn held by this contract
        uint256 balanceAfter = balanceTknIn(_tokenIn);

        // balance of tokenIn held by this contract after
        require( balanceBefore  +  _amountIn  >=  balanceAfter, "Err: Contracts balance not Increased" );
 
        if ( exchangeRateTuin == 0 ) revert("Err: exchange rate not set");

        uint256 _amountOut = amountTuinOut(_tokenIn, _tokenOut,_amountIn);

        require( tuinHeld(_tokenOut) >= _amountOut, "Err: Tuin Left is Less" );

        // transfer to user
        (bool success2, bytes memory data2) = address(_tokenOut).call(abi.encodeWithSelector(IERC20.transfer.selector, address(msg.sender), _amountOut));

        require( success2  &&  data2.length  >=  32, "Err: Couldn't Transfer Out" );

        recordAmountDepositedacceptedToken(_tokenIn, _amountIn);

        return true;
    }


    function tuinHeld(address tuinToken) public view returns (uint256 amountHeld) {
        (bool success, bytes memory data) = tuinToken.staticcall(
            abi.encodeWithSelector(IERC20.balanceOf.selector, address(this))
        );
        
        require( success  &&  data.length  >=  32, "Err: Couldn't get Tuin held" );

        amountHeld = abi.decode(data, (uint256));
    }

    // withdraws specified amount of Deposited accepted token
    function withdrawAcceptedToken(address _tokenIn, address _to, uint256 _amountOut) onlyOwner() public returns (bool) {
        (bool success, bytes memory data) = address(_tokenIn).call(abi.encodeWithSelector(IERC20.transfer.selector, _to, _amountOut));

        require( success  &&  data.length  >=  32, "Err: Couldn't withdraw accepted token held" );

        recordamountWithdrawnacceptedToken(_tokenIn, _amountOut);

        return true;
    }

    function depositYieldToken(address _tokenIn, uint256 _amountIn) onlyOwner() external returns (bool) {
        require( _tokenIn  ==  acceptedToken1  ||  _tokenIn  ==  acceptedToken2  ||  _tokenIn  ==  acceptedToken3, "Err: Not accepted token" );

        require( _tokenIn  !=  address(0) ,  "Err: TokenIn address(0)" );

        (bool success, bytes memory data) = address(_tokenIn).call(abi.encodeWithSelector(IERC20.transferFrom.selector, msg.sender, address(this), _amountIn));

        require( success  &&  data.length  >=  32, "Err: Couldn't deposit accepted token" );

        recordamountYieldDepositedacceptedToken(_tokenIn, _amountIn);

        return true;
    }

    // Redeem or swap out TUIN to get any of the deposited tokens
    // A different contract can handle redemption, at the point of redemption
    function redeem(uint256 _amountIn, address _tuinToken, address _yieldAddress) Paused() external {
        require( _amountIn != 0, "Err: cannot redeem zero amount");

        require( _yieldAddress  !=  address(0),  "Err: yield not set" );

        require( _yieldAddress  ==  acceptedToken1  ||  _yieldAddress  ==  acceptedToken2  ||  _yieldAddress  ==  acceptedToken3, "Err: Not accepted token" );

        require( isApproved     !=  false, "Err: still generating yield, check back" );
    
        // balance of tokenIn held by this contract
        uint256 balanceBefore = balanceTknIn(_tuinToken);

        // user transfer to this contract
        (bool success1, bytes memory data1) = _tuinToken.call(abi.encodeWithSelector(IERC20.transferFrom.selector, msg.sender, address(this), _amountIn));

        require( success1  &&  data1.length  >=  32, "Err: Couldn't TransferFrom" );

        // balance of tokenIn held by this contract
        uint256 balanceAfter = balanceTknIn(_tuinToken);

        // balance of tuin token held by this contract after increases
        require( balanceBefore  +  _amountIn  >=  balanceAfter, "Err: Contracts balance not Increased" );

        // burns tuin tokens
       (bool success2, bytes memory data2) = _tuinToken.call(abi.encodeWithSignature("burn(address,uint256)", address(this), _amountIn));

        require( success2  &&  data2.length  >=  32, "Err: Couldn't Burn" );

        if ( exchangeRateUsd == 0 ) revert("Err: exchange rate not set");

        uint256 _amountOut = scaleYieldToDecimal(_yieldAddress, _amountIn);

        require(yieldToken == _yieldAddress, "Err: not yield address");

        (bool success4, bytes memory data4) = _yieldAddress.call(abi.encodeWithSelector(IERC20.transfer.selector, address(msg.sender), _amountOut));

        require( success4  &&  data4.length  >=  32, "Err: TRANSFER_OUT_FAILED" );
    }
}