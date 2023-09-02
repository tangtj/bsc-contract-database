pragma solidity 0.5.17;

interface IUniswapV2Factory {
  event PairCreated(address indexed token0, address indexed token1, address pair, uint);
  function getPair(address tokenA, address tokenB) external view returns (address pair);
  function allPairs(uint) external view returns (address pair);
  function allPairsLength() external view returns (uint);
  function feeTo() external view returns (address);
  function feeToSetter() external view returns (address);
  function createPair(address tokenA, address tokenB) external returns (address pair);
}


interface IUniswapV2Pair {
  event Approval(address indexed owner, address indexed spender, uint value);
  event Transfer(address indexed from, address indexed to, uint value);
  function name() external pure returns (string memory);
  function symbol() external pure returns (string memory);
  function decimals() external pure returns (uint8);
  function totalSupply() external view returns (uint);
  function balanceOf(address owner) external view returns (uint);
  function allowance(address owner, address spender) external view returns (uint);
  function approve(address spender, uint value) external returns (bool);
  function transfer(address to, uint value) external returns (bool);
  function transferFrom(address from, address to, uint value) external returns (bool);
  function DOMAIN_SEPARATOR() external view returns (bytes32);
  function PERMIT_TYPEHASH() external pure returns (bytes32);
  function nonces(address owner) external view returns (uint);
  function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;
  event Mint(address indexed sender, uint amount0, uint amount1);
  event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
  event Swap(
      address indexed sender,
      uint amount0In,
      uint amount1In,
      uint amount0Out,
      uint amount1Out,
      address indexed to
  );
  event Sync(uint112 reserve0, uint112 reserve1);
  function MINIMUM_LIQUIDITY() external pure returns (uint);
  function factory() external view returns (address);
  function token0() external view returns (address);
  function token1() external view returns (address);
  function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
  function price0CumulativeLast() external view returns (uint);
  function price1CumulativeLast() external view returns (uint);
  function kLast() external view returns (uint);
  function mint(address to) external returns (uint liquidity);
  function burn(address to) external returns (uint amount0, uint amount1);
  function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
  function skim(address to) external;
  function sync() external;
}


interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function name() external view returns (string memory);
    function decimals() external view returns (uint8);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IWETH {
    function withdraw(uint) external;
    function deposit() external payable;
}

contract flashSwap {

    enum SwapType {SimpleLoan, SimpleSwap, TriangularSwap}

    // CONSTANTS
    IUniswapV2Factory constant uniswapV2Factory = IUniswapV2Factory(0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73); // same for all networks
    address constant BNB = address(0);

    // ACCESS CONTROL
    // Only the `permissionedPairAddress` may call the `uniswapV2Call` function
    address permissionedPairAddress = address(1);

    // DEFAULT TOKENS
    address WBNB;
    address CAKE;

    constructor() public {
        WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        CAKE = 0x0E09FaBB73Bd3Ade0a17ECC321fD13a19e81cE82;
    }

    // Fallback must be payable
    function() external payable {}

    // @notice Flash-borrows _amount of _tokenBorrow from a Uniswap V2 pair and repays using _tokenPay
    // @param _tokenBorrow The address of the token you want to flash-borrow, use 0x0 for BNB
    // @param _amount The amount of _tokenBorrow you will borrow
    // @param _tokenPay The address of the token you want to use to payback the flash-borrow, use 0x0 for BNB
    // @param _userData Data that will be passed to the `execute` function for the user
    // @dev Depending on your use case, you may want to add access controls to this function
    function startSwap(address _tokenBorrow, uint256 _amount, address _tokenPay, bytes memory _userData) public {
        bool isBorrowingBNB;
        bool isPayingBNB;
        address tokenBorrow = _tokenBorrow;
        address tokenPay = _tokenPay;

        if (tokenBorrow == BNB) {
            isBorrowingBNB = true;
            tokenBorrow = WBNB; // we'll borrow WBNB from UniswapV2 but then unwrap it for the user
        }
        if (tokenPay == BNB) {
            isPayingBNB = true;
            tokenPay = WBNB; // we'll wrap the user's BNB before sending it back to UniswapV2
        }

        // if (tokenBorrow == tokenPay) {
        //     simpleFlashLoan(tokenBorrow, _amount, isBorrowingBNB, isPayingBNB, _userData);
        //     return;
        // } else if (tokenBorrow == WBNB || tokenPay == WBNB) {
        simpleFlashSwap(tokenBorrow, _amount, tokenPay, isBorrowingBNB, isPayingBNB, _userData);
        return;
        // } else {
        //     traingularFlashSwap(tokenBorrow, _amount, tokenPay, _userData);
        //     return;
        // }

    }


    // @notice Function is called by the Uniswap V2 pair's `swap` function
    function pancakeCall(address _sender, uint _amount0, uint _amount1, bytes calldata _data) external {
        // access control
        // require(msg.sender == permissionedPairAddress, "only permissioned UniswapV2 pair can call");
        // require(_sender == address(this), "only this contract may initiate");
        
        // decode data
        // (
        //     SwapType _swapType,
        //     address _tokenBorrow,
        //     uint _amount,
        //     address _tokenPay,
        //     bool _isBorrowingBNB,
        //     bool _isPayingBNB,
        //     bytes memory _triangleData,
        //     bytes memory _userData
        // ) = abi.decode(_data, (SwapType, address, uint, address, bool, bool, bytes, bytes));

        // if (_swapType == SwapType.SimpleLoan) {
        simpleFlashLoanExecute();
        return;
        // } else if (_swapType == SwapType.SimpleSwap) {
        //     simpleFlashSwapExecute(_tokenBorrow, _amount, _tokenPay, msg.sender, _isBorrowingBNB, _isPayingBNB, _userData);
        //     return;
        // }
        // // } else {
        //     traingularFlashSwapExecute(_tokenBorrow, _amount, _tokenPay, _triangleData, _userData);
        // }

        // NOOP to silence compiler "unused parameter" warning
        if (false) {
            _amount0;
            _amount1;
        }
    }

    // @notice This function is used when the user repays with the same token they borrowed
    // @dev This initiates the flash borrow. See `simpleFlashLoanExecute` for the code that executes after the borrow.
    function simpleFlashLoan(address _tokenBorrow, uint256 _amount, bool _isBorrowingBNB, bool _isPayingBNB, bytes memory _userData) private {
        address tokenOther = _tokenBorrow == WBNB ? CAKE : WBNB;
        permissionedPairAddress = uniswapV2Factory.getPair(_tokenBorrow, tokenOther); // is it cheaper to compute this locally?
        address pairAddress = permissionedPairAddress; // gas efficiency
        require(pairAddress != address(0), "Requested _token is not available.");
        address token0 = IUniswapV2Pair(pairAddress).token0();
        address token1 = IUniswapV2Pair(pairAddress).token1();
        uint amount0Out = _tokenBorrow == token0 ? _amount : 0;
        uint amount1Out = _tokenBorrow == token1 ? _amount : 0;
        bytes memory data = abi.encode(
            SwapType.SimpleLoan,
            _tokenBorrow,
            _amount,
            _tokenBorrow,
            _isBorrowingBNB,
            _isPayingBNB,
            bytes(""),
            _userData
        ); // note _tokenBorrow == _tokenPay
        IUniswapV2Pair(pairAddress).swap(amount0Out, amount1Out, address(this), data);
    }

    // @notice This is the code that is executed after `simpleFlashLoan` initiated the flash-borrow
    // @dev When this code executes, this contract will hold the flash-borrowed _amount of _tokenBorrow
    function simpleFlashLoanExecute() private {
        uint256 _amount = 100000000000000000;
        uint fee = ((_amount * 3) / 997) + 1;
        uint amountToRepay = _amount + fee;
        
        IERC20(CAKE).transfer(0x0eD7e52944161450477ee417DE9Cd3a859b14fD0, amountToRepay);

                // unwrap WBNB if necessary
        // if (_isBorrowingBNB) {
        //     IWETH(WBNB).withdraw(_amount);
        // }

        // compute amount of tokens that need to be paid back

        // address tokenBorrowed = _isBorrowingBNB ? BNB : _tokenBorrow;
        //address tokenToRepay = _isPayingBNB ? BNB : _tokenBorrow;
        //address tokenToRepay = CAKE
        // do whatever the user wants
       // execute(tokenBorrowed, _amount, tokenToRepay, amountToRepay, _userData);

        // payback the loan
        // wrap the BNB if necessary
        // if (_isPayingBNB) {
        //     IWETH(WBNB).deposit.value(amountToRepay)();
        // }
        
    }

    // @notice This function is used when either the _tokenBorrow or _tokenPay is WBNB or BNB
    // @dev Since ~all tokens trade against WBNB (if they trade at all), we can use a single UniswapV2 pair to
    //     flash-borrow and repay with the requested tokens.
    // @dev This initiates the flash borrow. See `simpleFlashSwapExecute` for the code that executes after the borrow.
    function simpleFlashSwap(
        address _tokenBorrow,
        uint _amount,
        address _tokenPay,
        bool _isBorrowingBNB,
        bool _isPayingBNB,
        bytes memory _userData
    ) private {
        permissionedPairAddress = uniswapV2Factory.getPair(_tokenBorrow, _tokenPay); // is it cheaper to compute this locally?
        address pairAddress = permissionedPairAddress; // gas efficiency
        require(pairAddress != address(0), "Requested pair is not available.");
        address token0 = IUniswapV2Pair(pairAddress).token0();
        address token1 = IUniswapV2Pair(pairAddress).token1();
        uint amount0Out = _tokenBorrow == token0 ? _amount : 0;
        uint amount1Out = _tokenBorrow == token1 ? _amount : 0;
        bytes memory data = abi.encode(
            SwapType.SimpleSwap,
            _tokenBorrow,
            _amount,
            _tokenPay,
            _isBorrowingBNB,
            _isPayingBNB,
            bytes(""),
            _userData
        );
        IUniswapV2Pair(pairAddress).swap(amount0Out, amount1Out, address(this), data);
    }

    // @notice This is the code that is executed after `simpleFlashSwap` initiated the flash-borrow
    // @dev When this code executes, this contract will hold the flash-borrowed _amount of _tokenBorrow
    // function simpleFlashSwapExecute(
    //     address _tokenBorrow,
    //     uint _amount,
    //     address _tokenPay,
    //     address _pairAddress,
    //     bool _isBorrowingBNB,
    //     bool _isPayingBNB,
    //     bytes memory _userData
    // ) private {
    //     // unwrap WBNB if necessary
    //     if (_isBorrowingBNB) {
    //         IWETH(WBNB).withdraw(_amount);
    //     }

    //     // compute the amount of _tokenPay that needs to be repaid
    //     address pairAddress = permissionedPairAddress; // gas efficiency
    //     uint pairBalanceTokenBorrow = IERC20(_tokenBorrow).balanceOf(pairAddress);
    //     uint pairBalanceTokenPay = IERC20(_tokenPay).balanceOf(pairAddress);
    //     uint amountToRepay = ((1000 * pairBalanceTokenPay * _amount) / (997 * pairBalanceTokenBorrow)) + 1;

    //     // get the orignal tokens the user requested
    //     address tokenBorrowed = _isBorrowingBNB ? BNB : _tokenBorrow;
    //     address tokenToRepay = _isPayingBNB ? BNB : _tokenPay;

    //     // do whatever the user wants
    //     execute(tokenBorrowed, _amount, tokenToRepay, amountToRepay, _userData);

    //     // payback loan
    //     // wrap BNB if necessary
    //     if (_isPayingBNB) {
    //         IWETH(WBNB).deposit.value(amountToRepay)();
    //     }
    //     IERC20(_tokenPay).transfer(_pairAddress, amountToRepay);
    // }

    // @notice This function is used when neither the _tokenBorrow nor the _tokenPay is WBNB
    // @dev Since it is unlikely that the _tokenBorrow/_tokenPay pair has more liquiCAKEty than the _tokenBorrow/WBNB and
    //     _tokenPay/WBNB pairs, we do a triangular swap here. That is, we flash borrow WBNB from the _tokenPay/WBNB pair,
    //     Then we swap that borrowed WBNB for the desired _tokenBorrow via the _tokenBorrow/WBNB pair. And finally,
    //     we pay back the original flash-borrow using _tokenPay.
    // @dev This initiates the flash borrow. See `traingularFlashSwapExecute` for the code that executes after the borrow.
    // function traingularFlashSwap(address _tokenBorrow, uint _amount, address _tokenPay, bytes memory _userData) private {
    //     address borrowPairAddress = uniswapV2Factory.getPair(_tokenBorrow, WBNB); // is it cheaper to compute this locally?
    //     require(borrowPairAddress != address(0), "Requested borrow token is not available.");

    //     permissionedPairAddress = uniswapV2Factory.getPair(_tokenPay, WBNB); // is it cheaper to compute this locally?
    //     address payPairAddress = permissionedPairAddress; // gas efficiency
    //     require(payPairAddress != address(0), "Requested pay token is not available.");

    //     // STEP 1: Compute how much WBNB will be needed to get _amount of _tokenBorrow out of the _tokenBorrow/WBNB pool
    //     uint pairBalanceTokenBorrowBefore = IERC20(_tokenBorrow).balanceOf(borrowPairAddress);
    //     require(pairBalanceTokenBorrowBefore >= _amount, "_amount is too big");
    //     uint pairBalanceTokenBorrowAfter = pairBalanceTokenBorrowBefore - _amount;
    //     uint pairBalanceWBNB = IERC20(WBNB).balanceOf(borrowPairAddress);
    //     uint amountOfWBNB = ((1000 * pairBalanceWBNB * _amount) / (997 * pairBalanceTokenBorrowAfter)) + 1;

    //     // using a helper function here to avoid "stack too deep" :(
    //     traingularFlashSwapHelper(_tokenBorrow, _amount, _tokenPay, borrowPairAddress, payPairAddress, amountOfWBNB, _userData);
    // }

    // @notice Helper function for `traingularFlashSwap` to avoid `stack too deep` errors
    // function traingularFlashSwapHelper(
    //     address _tokenBorrow,
    //     uint _amount,
    //     address _tokenPay,
    //     address _borrowPairAddress,
    //     address _payPairAddress,
    //     uint _amountOfWBNB,
    //     bytes memory _userData
    // ) private returns (uint) {
    //     // Step 2: Flash-borrow _amountOfWBNB WBNB from the _tokenPay/WBNB pool
    //     address token0 = IUniswapV2Pair(_payPairAddress).token0();
    //     address token1 = IUniswapV2Pair(_payPairAddress).token1();
    //     uint amount0Out = WBNB == token0 ? _amountOfWBNB : 0;
    //     uint amount1Out = WBNB == token1 ? _amountOfWBNB : 0;
    //     bytes memory triangleData = abi.encode(_borrowPairAddress, _amountOfWBNB);
    //     bytes memory data = abi.encode(SwapType.TriangularSwap, _tokenBorrow, _amount, _tokenPay, false, false, triangleData, _userData);
    //     // initiate the flash swap from UniswapV2
    //     IUniswapV2Pair(_payPairAddress).swap(amount0Out, amount1Out, address(this), data);
    // }

    // @notice This is the code that is executed after `traingularFlashSwap` initiated the flash-borrow
    // @dev When this code executes, this contract will hold the amount of WBNB we need in order to get _amount
    //     _tokenBorrow from the _tokenBorrow/WBNB pair.
    // function traingularFlashSwapExecute(
    //     address _tokenBorrow,
    //     uint _amount,
    //     address _tokenPay,
    //     bytes memory _triangleData,
    //     bytes memory _userData
    // ) private {
    //     // decode _triangleData
    //     (address _borrowPairAddress, uint _amountOfWBNB) = abi.decode(_triangleData, (address, uint));

    //     // Step 3: Using a normal swap, trade that WBNB for _tokenBorrow
    //     address token0 = IUniswapV2Pair(_borrowPairAddress).token0();
    //     address token1 = IUniswapV2Pair(_borrowPairAddress).token1();
    //     uint amount0Out = _tokenBorrow == token0 ? _amount : 0;
    //     uint amount1Out = _tokenBorrow == token1 ? _amount : 0;
    //     IERC20(WBNB).transfer(_borrowPairAddress, _amountOfWBNB); // send our flash-borrowed WBNB to the pair
    //     IUniswapV2Pair(_borrowPairAddress).swap(amount0Out, amount1Out, address(this), bytes(""));

    //     // compute the amount of _tokenPay that needs to be repaid
    //     address payPairAddress = permissionedPairAddress; // gas efficiency
    //     uint pairBalanceWBNB = IERC20(WBNB).balanceOf(payPairAddress);
    //     uint pairBalanceTokenPay = IERC20(_tokenPay).balanceOf(payPairAddress);
    //     uint amountToRepay = ((1000 * pairBalanceTokenPay * _amountOfWBNB) / (997 * pairBalanceWBNB)) + 1;

    //     // Step 4: Do whatever the user wants (arb, liqudiation, etc)
    //     execute(_tokenBorrow, _amount, _tokenPay, amountToRepay, _userData);

    //     // Step 5: Pay back the flash-borrow to the _tokenPay/WBNB pool
    //     IERC20(_tokenPay).transfer(payPairAddress, amountToRepay);
    // }

    // @notice This is where the user's custom logic goes
    // @dev When this function executes, this contract will hold _amount of _tokenBorrow
    // @dev It is important that, by the end of the execution of this function, this contract holds the necessary
    //     amount of the original _tokenPay needed to pay back the flash-loan.
    // @dev Paying back the flash-loan happens automatically by the calling function -- do not pay back the loan in this function
    // @dev If you entered `0x0` for _tokenPay when you called `flashSwap`, then make sure this contract holds _amount BNB before this
    //     finishes executing
    // @dev User will override this function on the inheriting contract
       function execute(address _tokenBorrow, uint _amount, address _tokenPay, uint _amountToRepay, bytes memory _userData) internal {
        // do whatever you want here
        // we're just going to update some local variables because we're boring
        // but you could do some arbitrage or liquidaztions or CDP collateral swaps, etc

       
    }

}