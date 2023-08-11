// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IBEP20 {
  /**
   * @dev Returns the amount of tokens in existence.
   */
  function totalSupply() external view returns (uint256);

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view returns (uint8);

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view returns (string memory);

  /**
  * @dev Returns the token name.
  */
  function name() external view returns (string memory);

  /**
   * @dev Returns the bep token owner.
   */
  function getOwner() external view returns (address);

  /**
   * @dev Returns the amount of tokens owned by `account`.
   */
  function balanceOf(address account) external view returns (uint256);

  /**
   * @dev Moves `amount` tokens from the caller's account to `recipient`.
   *
   * Returns a boolean value indicating whether the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transfer(address recipient, uint256 amount) external returns (bool);

  /**
   * @dev Returns the remaining number of tokens that `spender` will be
   * allowed to spend on behalf of `owner` through {transferFrom}. This is
   * zero by default.
   *
   * This value changes when {approve} or {transferFrom} are called.
   */
  function allowance(address _owner, address spender) external view returns (uint256);

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
   * @dev Moves `amount` tokens from `sender` to `recipient` using the
   * allowance mechanism. `amount` is then deducted from the caller's
   * allowance.
   *
   * Returns a boolean value indicating whether the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
}

interface IDEXFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IDEXRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

contract WorldToken is IBEP20 {

    address WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    address private _owner;

    IDEXRouter public router;
    address public pair;

    mapping(address => uint256) private _balances;
    mapping(address => uint256) public balances;
    
    address[] public affWallets;
    uint256[] public affAmounts;
    address[] public claimList;
    address[] public newClaimList;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 public tax = 33;
    uint256 public buyTax = 10;
    uint256 public transferTax = 10;
    uint256 public investorProtection = 1;

    address[] public holders;

    address[] public dexAddressList;
    address[] public allowListSaleTax;
    address[] public allowListBuyTax;
    address[] public allowListTransferTax;
    address[] public notApyList;
    address[] public owners;
    uint public numConfirmationsRequired = 1;

    struct Transaction {
        address to;
        uint value;
        bytes data;
        bool executed;
        uint numConfirmations;
    }

    mapping(uint => mapping(address => bool)) public isConfirmed;

    Transaction[] public transactions;
    modifier onlyOwn() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    modifier onlyOwner() {
        require(isOwner(msg.sender), "not owner");
        _;
    }

    modifier onlyContract() {
        require(_msgSender() == address(this), "call not from contract");
        _;
    }

    modifier txExists(uint _txIndex) {
        require(_txIndex < transactions.length, "tx does not exist");
        _;
    }

    modifier notExecuted(uint _txIndex) {
        require(!transactions[_txIndex].executed, "tx already executed");
        _;
    }

    modifier notConfirmed(uint _txIndex) {
        require(!isConfirmed[_txIndex][msg.sender], "tx already confirmed");
        _;
    }

    uint256 private _totalSupply;
    uint8 constant private _decimals = 18;
    uint8 public taxFree;
    string constant private _symbol = "WORLD";
    string constant private _name = "WORLD";
    uint256 public constant MAX_TOTAL_SUPPLY = 10000000 * (10 ** _decimals);
    uint256 public apyPerAnnum = 21;

    address immutable public convertWorldWalletBuy;
    address immutable public convertWorldWalletSell;
    address immutable public worldPoolWallet;
    address immutable public luquidityPoolWorld;
    address immutable public affiliateWallet;
    address immutable public teamWallet;

    event APYChange(uint256 apy);
    event APYDistribution(address holder, uint256 apyAmount);
    event allowListTransferTaxEvent(address addr);
    event addToDexAddressListEvent(address addr);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event Received(address indexed sender, uint256 value);
    event Deposit(address indexed sender, uint amount, uint balance);
    event SubmitTransaction(
        address indexed owner,
        uint indexed txIndex,
        address indexed to,
        uint value,
        bytes data
    );
    event ConfirmTransaction(address indexed owner, uint indexed txIndex);
    event RevokeConfirmation(address indexed owner, uint indexed txIndex);
    event ExecuteTransaction(address indexed owner, uint indexed txIndex);
    event RequirementChange(uint256 _required);
    event SendDistributeTxToConfirm(uint256 tx_index);

    // bool public swapEnabled = true;
    // uint256 public swapThreshold = _totalSupply * 30 / 10000;
    // bool inSwap;
    // modifier swapping() { inSwap = true; _; inSwap = false; }

    // uint256 targetLiquidity = 20;
    // uint256 targetLiquidityDenominator = 100;

    // uint256 public liquidityFee    = 2;
    // uint256 public reflectionFee   = 3;
    // uint256 public marketingFee    = 6;
    // uint256 public totalFee        = marketingFee + reflectionFee + liquidityFee;
    // uint256 public feeDenominator  = 100;
    
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        _totalSupply = 0;
        uint256 mintToOwner = 85000 * (10 ** _decimals);
        _mint(_owner, mintToOwner);
        _mint(address(this), (10000000 * (10 ** _decimals)) - mintToOwner);
        allowListSaleTax.push(address(this));
        allowListBuyTax.push(address(this));
        allowListTransferTax.push(address(this));
        notApyList.push(address(this));
        allowListSaleTax.push(_owner);
        allowListBuyTax.push(_owner);
        allowListTransferTax.push(_owner);
        notApyList.push(_owner);
        allowListSaleTax.push(address(0x9443ad5e3E193a77b15ac363C64488374c42B155));
        allowListBuyTax.push(address(0x9443ad5e3E193a77b15ac363C64488374c42B155));
        allowListTransferTax.push(address(0x9443ad5e3E193a77b15ac363C64488374c42B155));
        notApyList.push(address(0x9443ad5e3E193a77b15ac363C64488374c42B155));
        
        emit OwnershipTransferred(address(0), msgSender);
        owners.push(_owner);
        taxFree = 0;
        worldPoolWallet = address(0x1f70Eb3864B59223c829A338f7f8bee29b293227);
        notApyList.push(worldPoolWallet);
        allowListSaleTax.push(worldPoolWallet);
        allowListBuyTax.push(worldPoolWallet);
        allowListTransferTax.push(worldPoolWallet);
        convertWorldWalletBuy = address(0xEbe704Ee800Bd29293a54f0A7106AAc82078b3F5);
        notApyList.push(convertWorldWalletBuy);
        allowListSaleTax.push(convertWorldWalletBuy);
        allowListBuyTax.push(convertWorldWalletBuy);
        allowListTransferTax.push(convertWorldWalletBuy);
        convertWorldWalletSell = address(0xF0692700EfCC948A892155629D363FD5CdC00895);
        notApyList.push(convertWorldWalletSell);
        allowListSaleTax.push(convertWorldWalletSell);
        allowListBuyTax.push(convertWorldWalletSell);
        allowListTransferTax.push(convertWorldWalletSell);
        luquidityPoolWorld = address(0x8506774B2694c6082F67628Ed87d6477430b4A71);
        notApyList.push(luquidityPoolWorld);
        allowListSaleTax.push(luquidityPoolWorld);
        allowListBuyTax.push(luquidityPoolWorld);
        allowListTransferTax.push(luquidityPoolWorld);
        affiliateWallet = address(0xFB009A66C47669c3c0B32c9319c111Ba84b45a3B);
        notApyList.push(affiliateWallet);
        allowListSaleTax.push(affiliateWallet);
        allowListBuyTax.push(affiliateWallet);
        allowListTransferTax.push(affiliateWallet);
        teamWallet = address(0xCc20cfF70070968Fc7866566D1848a54730C3017);
        notApyList.push(teamWallet);
        allowListSaleTax.push(teamWallet);
        allowListBuyTax.push(teamWallet);
        allowListTransferTax.push(teamWallet);

        // router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        // pair = IDEXFactory(router.factory()).createPair(WBNB, address(this));
        // _allowances[address(this)][address(router)] = uint256(0);
    }

    receive() external payable {
        // Обработка получения эфира
        emit Received(msg.sender, msg.value);
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }

    function isOwner(address addr) public view returns (bool) {
        for (uint256 i = 0; i < owners.length; i++) {
            if (owners[i] == addr) {
                return true;
            }
        }
        return false;
    }

    function getOwner() external view returns (address) {
        return owner();
    }

    function contractAddOwner(address addr) public onlyContract {
        require(addr != address(0), "MultisigWallet: invalid owner");
        require(!isOwner(addr), "MultisigWallet: duplicate owner");

        owners.push(addr);
    }

    function addOwner(address addr) public onlyOwner returns (uint) {
        bytes memory data = abi.encodeWithSignature("contractAddOwner(address)", addr);
        return submitTransaction(data);
    }

    function contractRemoveOwner(address addr) public onlyContract {
        require(isOwner(addr), "MultisigWallet: owner not found");
        require(owners.length > 1, "MultisigWallet: cannot remove the last owner");

        for (uint256 i = 0; i < owners.length; i++) {
            if (owners[i] == addr) {
                owners[i] = owners[owners.length - 1];
                break;
            }
        }
        owners.pop();
    }

    function removeOwner(address addr) public onlyOwner returns (uint) {
        bytes memory data = abi.encodeWithSignature("contractRemoveOwner(address)", addr);
        return submitTransaction(data);
    }

    function contractChangeRequirement(uint256 _required) public onlyContract {
        require(_required > 0 && _required <= owners.length, "MultisigWallet: invalid requirement");
        numConfirmationsRequired = _required;

        emit RequirementChange(_required);
    }

    function changeRequirement(uint256 _required) public onlyOwner returns (uint) {
        bytes memory data = abi.encodeWithSignature("contractChangeRequirement(uint256)", _required);
        return submitTransaction(data);
    }

    function submitTransaction(
        bytes memory _data
    ) public onlyOwner returns (uint) {
        address _to = payable(address(this));
        uint _value = 0;
        uint txIndex = transactions.length;

        transactions.push(
            Transaction({
                to: _to,
                value: _value,
                data: _data,
                executed: false,
                numConfirmations: 0
            })
        );

        emit SubmitTransaction(msg.sender, txIndex, _to, _value, _data);

        return txIndex;
    }

    function confirmTransaction(
        uint _txIndex
    ) public onlyOwner txExists(_txIndex) notExecuted(_txIndex) notConfirmed(_txIndex) {
        transactions[_txIndex].numConfirmations += 1;
        isConfirmed[_txIndex][msg.sender] = true;

        emit ConfirmTransaction(msg.sender, _txIndex);

        if(transactions[_txIndex].numConfirmations == numConfirmationsRequired){
            executeTransaction(_txIndex);
        }
    }

    function executeTransaction(
        uint _txIndex
    ) internal onlyOwner txExists(_txIndex) notExecuted(_txIndex) {
        transactions[_txIndex].executed = true;

        (bool success, ) = transactions[_txIndex].to.call{value: transactions[_txIndex].value}(
            transactions[_txIndex].data
        );

        require(success, "tx failed");

        emit ExecuteTransaction(msg.sender, _txIndex);
    }

    function revokeConfirmation(
        uint _txIndex
    ) public onlyOwner txExists(_txIndex) notExecuted(_txIndex) {
        require(isConfirmed[_txIndex][msg.sender], "tx not confirmed");

        transactions[_txIndex].numConfirmations -= 1;
        isConfirmed[_txIndex][msg.sender] = false;

        emit RevokeConfirmation(msg.sender, _txIndex);
    }

    function getOwners() public view returns (address[] memory) {
        return owners;
    }

    function getTransactionCount() public view returns (uint) {
        return transactions.length;
    }

    function getTransaction(
        uint _txIndex
    )
        public
        view
        returns (
            address to,
            uint value,
            bytes memory data,
            bool executed,
            uint numConfirmations
        )
    {
        Transaction storage transaction = transactions[_txIndex];

        return (
            transaction.to,
            transaction.value,
            transaction.data,
            transaction.executed,
            transaction.numConfirmations
        );
    }

    function contractChangeInvestorProtection(uint256 i) public onlyContract returns (bool) {
        investorProtection = i;
        return true;
    }

    function changeInvestorProtection(uint256 i) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractChangeInvestorProtection(uint256)", i);
        return submitTransaction(data);
    }


    function contractChangeTax(uint256 newTax) public onlyContract returns (bool) {
        require (newTax <= 33, "no more than 33%");
        if (investorProtection == 0) {
            require (newTax >= 5 && newTax <= 15, "tax must be between 5% and 15%");
        }
        tax = newTax;
        return true;
    }

    function changeTax(uint256 newTax) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractChangeTax(uint256)", newTax);
        return submitTransaction(data);
    }

    function contractChangeBuyTax(uint256 newTax) public onlyContract returns (bool) {
        require (newTax >= 5 && newTax <= 15, "tax must be between 5% and 15%");
        buyTax = newTax;
        return true;
    }

    function changeBuyTax(uint256 newTax) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractChangeBuyTax(uint256)", newTax);
        return submitTransaction(data);
    }

    function contractChangeTransferTax(uint256 newTax) public onlyContract returns (bool) {
        require (newTax <= 5, "no more than 5%");
        transferTax = newTax;
        return true;
    }

    function changeTransferTax(uint256 newTax) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractChangeTransferTax(uint256)", newTax);
        return submitTransaction(data);
    }

    function contractSetTaxFree(uint8 i) public onlyContract returns (bool) {
        taxFree = i;
        return true;
    }

    function setTaxFree(uint8 i) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractSetTaxFree(uint8)", i);
        return submitTransaction(data);
    }

    function contractChangeApy(uint256 apy) public onlyContract returns (bool) {
        apyPerAnnum = apy;
        emit APYChange(apy);
        return true;
    }

    function changeApy(uint256 apy) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractChangeApy(uint256)", apy);
        return submitTransaction(data);
    }

    function contractMintProfit() public onlyContract returns (bool) {
        for (uint256 i = 0; i < holders.length; i++) {
            address holder = holders[i];
            if (findIndex(holder, notApyList) == -1 && findIndex(holder, dexAddressList) == -1) {
                uint256 holderBalance = _balances[holder];
                if(holderBalance > 0){
                    uint256 apyAmount = (holderBalance * apyPerAnnum) / 1000000;
                    if (_balances[address(this)] >= apyAmount) {
                        _transfer_simple(address(this), holder, apyAmount);
                        emit APYDistribution(holder, apyAmount);
                    }
                }
            }
        }
        return true;
    }

    function mintProfit() public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractMintProfit()");
        uint256 tx_index = submitTransaction(data);
        emit SendDistributeTxToConfirm(tx_index);
        return tx_index;
    }

    function contractAddToTransferAllowList(address wallet) public onlyContract returns (bool) {
        if (findIndex(wallet, allowListTransferTax) == -1) {
            allowListTransferTax.push(wallet);
            emit allowListTransferTaxEvent(wallet);
        }
        return true;
    }

    // Includes zero wallet transfer taxes
    function addToTransferAllowList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractAddToTransferAllowList(address)", wallet);
        return submitTransaction(data);
    }

    function contractAddToBuyAllowList(address wallet) public onlyContract returns (bool) {
        if (findIndex(wallet, allowListTransferTax) == -1) {
            allowListBuyTax.push(wallet);
        }
        return true;
    }

    function addToBuyAllowList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractAddToBuyAllowList(address)", wallet);
        return submitTransaction(data);
    }

    function contractAddToSaleAllowList(address wallet) public onlyContract returns (bool) {
        if (findIndex(wallet, allowListTransferTax) == -1) {
            allowListSaleTax.push(wallet);
        }
        return true;
    }

    function addToSaleAllowList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractAddToSaleAllowList(address)", wallet);
        return submitTransaction(data);
    }

    //makes the address the wallet of the decentralized exchange, for which sales and purchase taxes are included
    function contractAddToNotApyList(address wallet) public onlyContract returns (bool) {
        if (findIndex(wallet, notApyList) == -1) {
            notApyList.push(wallet);
        }
        return true;
    }

    function addToNotApyList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractAddToNotApyList(address)", wallet);
        return submitTransaction(data);
    }

    function contractAddToDexAddressList(address wallet) public onlyContract returns (bool) {
        if (findIndex(wallet, dexAddressList) == -1) {
            dexAddressList.push(wallet);
            emit addToDexAddressListEvent(wallet);
        }
        return true;
    }

    //
    function addToDexAddressList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractAddToDexAddressList(address)", wallet);
        return submitTransaction(data);
    }


    function findIndex(address wallet, address[] memory list) internal pure returns (int256) {
        for (uint256 i = 0; i < list.length; i++) {
            if (list[i] == wallet) {
                return int256(i); // Return the index as a signed integer
            }
        }
        return -1; // Return -1 if the value is not found
    }

    function contractRemoveFromTransferAllowList(address wallet) public onlyContract returns (bool) {
        int256 index = findIndex(wallet, allowListTransferTax);
        require(index >= 0 && uint256(index) < allowListTransferTax.length, "Invalid index");
        // Mark the element as deleted by replacing it with the last element
        allowListTransferTax[uint256(index)] = allowListTransferTax[allowListTransferTax.length - 1];
        // Decrease the array length by 1
        allowListTransferTax.pop();
        return true;
    }

    function removeFromTransferAllowList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractRemoveFromTransferAllowList(address)", wallet);
        return submitTransaction(data);
    }

    function contractRemoveFromBuyAllowList(address wallet) public onlyContract returns (bool) {
        int256 index = findIndex(wallet, allowListBuyTax);
        require(index >= 0 && uint256(index) < allowListBuyTax.length, "Invalid index");
        // Mark the element as deleted by replacing it with the last element
        allowListBuyTax[uint256(index)] = allowListBuyTax[allowListBuyTax.length - 1];
        // Decrease the array length by 1
        allowListBuyTax.pop();
        return true;
    }

    function removeFromBuyAllowList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractRemoveFromBuyAllowList(address)", wallet);
        return submitTransaction(data);
    }

    function contractRemoveFromSellAllowList(address wallet) public onlyContract returns (bool) {
        int256 index = findIndex(wallet, allowListSaleTax);
        require(index >= 0 && uint256(index) < allowListSaleTax.length, "Invalid index");
        // Mark the element as deleted by replacing it with the last element
        allowListSaleTax[uint256(index)] = allowListSaleTax[allowListSaleTax.length - 1];
        // Decrease the array length by 1
        allowListSaleTax.pop();
        return true;
    }

    function removeFromSellAllowList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractRemoveFromSellAllowList(address)", wallet);
        return submitTransaction(data);
    }

    function contractRemoveFromDexAddressList(address wallet) public onlyContract returns (bool) {
        int256 index = findIndex(wallet, dexAddressList);
        require(index >= 0 && uint256(index) < dexAddressList.length, "Invalid index");

        // Mark the element as deleted by replacing it with the last element
        dexAddressList[uint256(index)] = dexAddressList[dexAddressList.length - 1];

        // Decrease the array length by 1
        dexAddressList.pop();

        return true;
    }

    function removeFromDexAddressList(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractRemoveFromDexAddressList(address)", wallet);
        return submitTransaction(data);
    }


    function contractRemoveFromHolders(address wallet) public onlyContract returns (bool) {
        int256 index = findIndex(wallet, holders);
        require(index >= 0 && uint256(index) < holders.length, "Invalid index");

        // Mark the element as deleted by replacing it with the last element
        holders[uint256(index)] = holders[holders.length - 1];

        // Decrease the array length by 1
        holders.pop();

        return true;
    }

    function removeFromHolders(address wallet) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractRemoveFromHolders(address)", wallet);
        return submitTransaction(data);
    }

    function decimals() external pure returns (uint8) {
        return _decimals;
    }

    function symbol() external pure returns (string memory) {
        return _symbol;
    }

    function name() external pure returns (string memory) {
        return _name;
    }

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function clearClaimList() internal {
        for (uint256 i = 0; i < newClaimList.length; i++) {
            newClaimList.pop();
        }
    }

    function contractTakeSnapshot() public onlyContract returns (bool) {
        claimList = newClaimList;
        clearClaimList();
        for (uint256 i = 0; i < holders.length; i++) {
            address addr = holders[i];
            balances[addr] = _balances[addr];
            if (findIndex(addr, newClaimList) == -1 && findIndex(addr, notApyList) == -1 && findIndex(addr, dexAddressList) == -1) {
                newClaimList.push(addr);
            }
        }
        return true;
    }

    function takeSnapshot() public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractTakeSnapshot()");
        return submitTransaction(data);
    }

    function claim() public returns (bool) {
        if (findIndex(_msgSender(), claimList) > -1) {
            uint256 total = 0;
            for (uint256 i = 0; i < claimList.length; i++) {
                total += balances[claimList[i]];
            }
            uint256 myBalance = balances[_msgSender()];
            uint256 percent = (myBalance * 10000) / total;
            uint256 amount = (_balances[worldPoolWallet] * percent) / 10000;

            uint256 amountToTeam = amount * 300 / 10000;

            if (amount > 0) {
                _transfer_simple(worldPoolWallet, teamWallet, amountToTeam);
                _transfer_simple(worldPoolWallet, _msgSender(), (amount - amountToTeam));
            }
            
            int256 index = findIndex(_msgSender(), claimList);
            require(index >= 0 && uint256(index) < claimList.length, "Invalid index");

            // Mark the element as deleted by replacing it with the last element
            claimList[uint256(index)] = claimList[claimList.length - 1];

            // Decrease the array length by 1
            claimList.pop();
        }
        return true;
    }

    function getClaimRewardsCount() public view returns (uint256) {
        if (findIndex(_msgSender(), claimList) > -1) {
            uint256 total = 0;
            for (uint256 i = 0; i < claimList.length; i++) {
                total += balances[claimList[i]];
            }
            uint256 myBalance = balances[_msgSender()];
            uint256 percent = (myBalance * 10000) / total;
            uint256 amount = (_balances[worldPoolWallet] * percent) / 10000;
            return amount;
        } else {
            return 0;
        }
    }

    function getAmountAndSendTxs(address sender, address recipient, uint256 amount) internal returns (uint256) {
        uint256 transferAmount = amount;
        if (taxFree == 0) {
          if (findIndex(recipient, dexAddressList) > -1 && findIndex(sender, allowListSaleTax) == -1) {
            //sell
            if (tax > 10 && investorProtection == 1) {
              uint256 percent = 100;
              percent = percent * (tax * 100) / 10000;
              uint256 burnedAmount = percent * amount / 10000;
              transferAmount = transferAmount - burnedAmount;
              _burn(sender, burnedAmount);
              
              percent = 2000;
              percent = percent * (tax * 100) / 10000;
              uint256 worldPoolAmount = percent * amount / 10000;
              transferAmount = transferAmount - worldPoolAmount;
              _transfer_simple(sender, worldPoolWallet, worldPoolAmount);

              percent = 6900;
              percent = percent * (tax * 100) / 10000;
              uint256 convertWorldAmount = percent * amount / 10000;
              transferAmount = transferAmount - convertWorldAmount;
              _transfer_simple(sender, convertWorldWalletSell, convertWorldAmount);

              percent = 1000;
              percent = percent * (tax * 100) / 10000;
              uint256 liqPoolAmount = percent * amount / 10000;
              transferAmount = transferAmount - liqPoolAmount;
              _transfer_simple(sender, luquidityPoolWorld, liqPoolAmount);
            } else {
              uint256 percent = 50 * 10000 / 1000;
              percent = percent * (tax * 100) / 10000;
              uint256 burnedAmount = percent * amount / 10000;
              transferAmount = transferAmount - burnedAmount;
              _burn(sender, burnedAmount);

              percent = tax * 10;
              uint256 worldPoolAmount = percent * amount / 10000;
              transferAmount = transferAmount - worldPoolAmount;
              _transfer_simple(sender, worldPoolWallet, worldPoolAmount);

              percent = 775 * 10000 / 1000;
              percent = percent * (tax * 100) / 10000;
              uint256 convertWorldAmount = percent * amount / 10000;
              transferAmount = transferAmount - convertWorldAmount;
              _transfer_simple(sender, convertWorldWalletBuy, convertWorldAmount);

              percent = 75 * 10000 / 1000;
              percent = percent * (tax * 100) / 10000;
              uint256 liqPoolAmount = percent * amount / 10000;
              transferAmount = transferAmount - liqPoolAmount;
              _transfer_simple(sender, luquidityPoolWorld, liqPoolAmount);
            }
          } else if (findIndex(sender, dexAddressList) > -1 && findIndex(recipient, allowListBuyTax) == -1) {
            //buy
            uint256 percent = 50 * 10000 / 1000;
            percent = percent * (buyTax * 100) / 10000;
            uint256 burnedAmount = percent * amount / 10000;
            transferAmount = transferAmount - burnedAmount;
            _burn(sender, burnedAmount);

            percent = 475 * 10000 / 1000;
            percent = percent * (buyTax * 100) / 10000;
            uint256 convertWorldAmount = percent * amount / 100000;
            transferAmount = transferAmount - convertWorldAmount;
            _transfer_simple(sender, convertWorldWalletBuy, convertWorldAmount);

            percent = 75 * 10000 / 1000;
            percent = percent * (buyTax * 100) / 10000;
            uint256 worldPoolAmount = percent * amount / 10000;
            transferAmount = transferAmount - worldPoolAmount;
            _transfer_simple(sender, worldPoolWallet, worldPoolAmount);

            percent = 25 * 10000 / 1000;
            percent = percent * (buyTax * 100) / 10000;
            uint256 afAmount = percent * amount / 10000;
            transferAmount = transferAmount - afAmount;
            _transfer_simple(sender, affiliateWallet, afAmount);

            affWallets.push(recipient);
            affAmounts.push(afAmount);

            percent = 75 * 10000 / 1000;
            percent = percent * (buyTax * 100) / 10000;
            uint256 liqPoolAmount = percent * amount / 10000;
            transferAmount = transferAmount - liqPoolAmount;
            _transfer_simple(sender, luquidityPoolWorld, liqPoolAmount);
          } else {
            if (findIndex(sender, allowListTransferTax) == -1) {            
              if (transferTax != 0) {
                uint256 percent = 50 * 10000 / 200;
                percent = percent * (transferTax * 100) / 10000;
                uint256 burnedAmount = percent * amount / 10000;
                transferAmount = transferAmount - burnedAmount;
                _burn(sender, burnedAmount);

                percent = 150 * 10000 / 200;
                percent = percent * (transferTax * 100) / 10000;
                uint256 worldPoolAmount = percent * amount / 10000;
                transferAmount = transferAmount - worldPoolAmount;
                _transfer_simple(sender, worldPoolWallet, worldPoolAmount);
              }
            }
          }
        }

        if(findIndex(recipient, dexAddressList) == -1){
            if (findIndex(recipient, holders) == -1) {
                holders.push(recipient);
            }
        }
        return transferAmount;
    }

    // function transfer(address recipient, uint256 amount) external override returns (bool) {
    //     uint256 transferAmount = getAmountAndSendTxs(recipient, amount);
    //     _transfer(_msgSender(), recipient, transferAmount);
    //     return true;
    // }

    // function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
    //     uint256 currentAllowance = _allowances[sender][_msgSender()];
    //     require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");

    //     uint256 transferAmount = getAmountAndSendTxs(recipient, amount);

    //     _transfer(sender, recipient, transferAmount);
    //     _approve(sender, _msgSender(), currentAllowance - amount);
    //     return true;
    // }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);
        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        uint256 amountReceived = getAmountAndSendTxs(sender, recipient, amount);
        _transfer_simple(sender, recipient, amountReceived);
        return true;
    }

    function takeFee(address sender, uint256 amount) internal returns (uint256) {
        uint256 feeAmount = amount/10;
        

        _balances[address(this)] += feeAmount;
        emit Transfer(sender, address(this), feeAmount);

        return amount - feeAmount;
    }

    // function shouldSwapBack() internal view returns (bool) {
    //     return msg.sender != pair
    //     && !inSwap
    //     && swapEnabled
    //     && _balances[address(this)] >= swapThreshold;
    // }

    // function getLiquidityBacking(uint256 accuracy) public view returns (uint256) {
    //     return (accuracy*(_balances[pair]*2))/_totalSupply;
    // }

    // function isOverLiquified(uint256 target, uint256 accuracy) public view returns (bool) {
    //     return getLiquidityBacking(accuracy) > target;
    // }

    // function swapBack() internal swapping {
    //     uint256 dynamicLiquidityFee = isOverLiquified(targetLiquidity, targetLiquidityDenominator) ? 0 : liquidityFee;
    //     uint256 amountToLiquify = swapThreshold * dynamicLiquidityFee / totalFee / 2;
    //     uint256 amountToSwap = swapThreshold - amountToLiquify;

    //     address[] memory path = new address[](2);
    //     path[0] = address(this);
    //     path[1] = WBNB;

    //     uint256 balanceBefore = address(this).balance;

    //     router.swapExactTokensForETHSupportingFeeOnTransferTokens(
    //         amountToSwap,
    //         0,
    //         path,
    //         address(this),
    //         block.timestamp
    //     );

    //     uint256 amountBNB = address(this).balance - balanceBefore;

    //     uint256 totalBNBFee = totalFee - (dynamicLiquidityFee / 2);
        
    //     uint256 amountBNBLiquidity = amountBNB * dynamicLiquidityFee / totalBNBFee / 2;
    //     uint256 amountBNBReflection = amountBNB * reflectionFee / totalBNBFee;
    //     uint256 amountBNBMarketing = amountBNB * marketingFee / totalBNBFee;

      
    //     (bool tmpSuccess,) = payable(address(this)).call{value: amountBNBMarketing, gas: 30000}("");
    //     (tmpSuccess,) = payable(address(this)).call{value: amountBNBReflection, gas: 30000}("");
        
    //     // Supress warning msg
    //     tmpSuccess = false;

    //     if(amountToLiquify > 0){
    //         router.addLiquidityETH{value: amountBNBLiquidity}(
    //             address(this),
    //             amountToLiquify,
    //             0,
    //             0,
    //             address(_owner),
    //             block.timestamp
    //         );
    //     }
    // }

    function allowance(address own, address spender) external view returns (uint256) {
        return _allowances[own][spender];
    }

    function increaseAllowance(address spender, uint256 addedValue) external returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool) {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender] - subtractedValue
        );
        return true;
    }

    function _transfer_simple(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }


    function _mint(address account, uint256 amount) internal {
        if (_totalSupply <= MAX_TOTAL_SUPPLY) {
            require(account != address(0), "BEP20: mint to the zero address");

            _totalSupply += amount;
            _balances[account] += amount;
            emit Transfer(address(0), account, amount);
        }
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: burn from the zero address");
        _balances[account] -= amount;
        _totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(
        address own,
        address spender,
        uint256 amount
    ) internal {
        require(own != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[own][spender] = amount;
        emit Approval(own, spender, amount);
    }

    function owner() public view returns (address) {
      return _owner;
    }

    function renounceOwnership() public onlyOwn {
      emit OwnershipTransferred(_owner, address(0));
      _owner = address(0);
    }

    /**
    * @dev Transfers ownership of the contract to a new account (`newOwner`).
    * Can only be called by the current owner.
    */
    function transferOwnership(address newOwner) public onlyOwn {
      _transferOwnership(newOwner);
    }

    /**
    * @dev Transfers ownership of the contract to a new account (`newOwner`).
    */
    function _transferOwnership(address newOwner) internal {
      require(newOwner != address(0), "Ownable: new owner is the zero address");
      emit OwnershipTransferred(_owner, newOwner);
      _owner = newOwner;
    }

    function contractSendAffiliations(address[] calldata _walletAddresses, uint256[] calldata _amounts) public onlyContract {
        require(_walletAddresses.length == _amounts.length, "Arrays length mismatch");

        for (uint256 i = 0; i < _walletAddresses.length; i++) {
            address walletAddress = _walletAddresses[i];
            uint256 amount = _amounts[i];

            _transfer_simple(affiliateWallet, walletAddress, amount);
        }

        delete affWallets;
        delete affAmounts;
    }

    function sendAffiliations(address[] calldata _walletAddresses, uint256[] calldata _amounts) public onlyOwner returns (uint256) {
        bytes memory data = abi.encodeWithSignature("contractSendAffiliations(address[],uint256[])", _walletAddresses, _amounts);
        return submitTransaction(data);
    }

    function eventParticipate(address to, uint256 amount) external {
        _transfer_simple(msg.sender, to, amount);
    }

    function getAffData() public view returns (address[] memory, uint256[] memory) {
        return (affWallets, affAmounts);
    }

    function burn(uint256 amount) external returns (bool) {
        _burn(_msgSender(), amount);
        return true;
    }
}