// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

library SafeMath {
    function tryAdd(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    function trySub(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    function tryMul(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    function tryDiv(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }

    function _msgValue() internal view virtual returns (uint256) {
        return msg.value;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

library Address {
    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(
            address(this).balance >= amount,
            "Address: insufficient balance"
        );
        (bool success, ) = recipient.call{value: amount}("");
        require(
            success,
            "Address: unable to send value, recipient may have reverted"
        );
    }

    function functionCall(
        address target,
        bytes memory data
    ) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        return
            functionCallWithValue(
                target,
                data,
                value,
                "Address: low-level call with value failed"
            );
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(
            address(this).balance >= value,
            "Address: insufficient balance for call"
        );
        require(isContract(target), "Address: call to non-contract");
        (bool success, bytes memory returndata) = target.call{value: value}(
            data
        );
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function functionStaticCall(
        address target,
        bytes memory data
    ) internal view returns (bytes memory) {
        return
            functionStaticCall(
                target,
                data,
                "Address: low-level static call failed"
            );
    }

    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");
        (bool success, bytes memory returndata) = target.staticcall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function functionDelegateCall(
        address target,
        bytes memory data
    ) internal returns (bytes memory) {
        return
            functionDelegateCall(
                target,
                data,
                "Address: low-level delegate call failed"
            );
    }

    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return _verifyCallResult(success, returndata, errorMessage);
    }

    function _verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) private pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            if (returndata.length > 0) {
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

interface CoinToken {
    function createToken(
        string memory _NAME,
        string memory _SYMBOL,
        uint256 _DECIMALS,
        uint256 _supply,
        uint256 _txFee,
        uint256 _lpFee,
        uint256 _DexFee,
        address routerAddress,
        address feeaddress,
        address tokenOwner,
        address service
    ) external payable returns (address);
}

contract TokenFactory is Context, Ownable {
    using Address for address;
    using SafeMath for uint256;

    address public coinTokenContract;
    uint256 public minValueBNB = 50000000000000000; // 0.05 BNB
    uint256 public servicePriceBNB = 500000000000000000; // 0.5 BNB

    struct configTokenStruct {
        string name;
        bool isActive;
        uint256 paymentAmount;
    }

    mapping(address => configTokenStruct) public tokenMatrix;

    event WithdrawalSuccessful(address indexed owner, uint256 amount);
    event WithdrawalFailed(address indexed owner, uint256 amount);
    event WithdrawalTokenSuccessful(address, address, uint256);
    event onTransferFromEvent(address, address, uint256 amount);
    event onTokenCreatedEvent(address, address, string, string, uint256);

    constructor(address _coinTokenContract, address _owner) {
        transferOwnership(_owner);
        coinTokenContract = _coinTokenContract;
    }

    function setMinValueBNB(
        uint256 _minValueBNB
    ) public onlyOwner returns (bool) {
        minValueBNB = _minValueBNB;
        return true;
    }

    function setServicePriceBNB(
        uint256 _servicePriceBNB
    ) public onlyOwner returns (bool) {
        servicePriceBNB = _servicePriceBNB;
        return true;
    }

    function updateTokenConfig(
        address _tokenAddress,
        string memory _name,
        bool _isActive,
        uint256 _paymentAmount
    ) public onlyOwner {
        tokenMatrix[_tokenAddress].name = _name;
        tokenMatrix[_tokenAddress].isActive = _isActive;
        tokenMatrix[_tokenAddress].paymentAmount = _paymentAmount;
    }

    function getTokenConfig(
        address _tokenAddress
    )
        public
        view
        onlyOwner
        returns (string memory name, bool isActive, uint256 paymentAmount)
    {
        configTokenStruct storage config = tokenMatrix[_tokenAddress];
        return (config.name, config.isActive, config.paymentAmount);
    }

    function removeTokenConfig(
        address _tokenAddress
    ) public onlyOwner returns (bool) {
        if (tokenMatrix[_tokenAddress].isActive) {
            delete tokenMatrix[_tokenAddress];
            return true;
        } else {
            return false;
        }
    }

    function requireTokenActive(address _tokenAddress) private view {
        configTokenStruct storage token = tokenMatrix[_tokenAddress];
        require(token.isActive, "El token no est\u00E1 activo");
    }

    function isERC20Token(address tokenAddress) public view returns (bool) {
        IERC20 token = IERC20(tokenAddress);
        try token.totalSupply() returns (uint256) {
            return true;
        } catch {
            return false;
        }
    }

    function requireValidERC20Token(address _tokenAddress) private view {
        require(
            isERC20Token(_tokenAddress),
            "El token no es un token ERC20 v\u00E1lido"
        );
    }

    function getTokenPaymentAmount(
        address _tokenAddress
    ) private view returns (uint256) {
        configTokenStruct storage token = tokenMatrix[_tokenAddress];
        return token.paymentAmount;
    }

    //verificar si tiene los tokens requeridos
    function requireSufficientTokenBalance(
        address _tokenAddress,
        uint256 _requiredAmount,
        string memory errorMessage
    ) private view {
        IERC20 tokenContract = IERC20(_tokenAddress);
        require(
            tokenContract.balanceOf(_msgSender()) >= _requiredAmount,
            errorMessage
        );
    }

    function requireTokenApproval(
        IERC20 _tokenContract,
        uint256 _toPayment
    ) private view {
        require(
            _tokenContract.allowance(_msgSender(), address(this)) >= _toPayment,
            "Error al aprobar los tokens"
        );
    }

    function createNewToken(
        string memory _NAME,
        string memory _SYMBOL,
        uint256 _DECIMALS,
        uint256 _supply,
        uint256 _txFee,
        uint256 _lpFee,
        uint256 _DexFee,
        address routerAddress,
        address feeaddress,
        address tokenOwner
    ) private returns (address) {
        CoinToken _coinTokenContract = CoinToken(coinTokenContract);
        address newTokenAddress = _coinTokenContract.createToken(
            _NAME,
            _SYMBOL,
            _DECIMALS,
            _supply,
            _txFee,
            _lpFee,
            _DexFee,
            routerAddress, // panckeswap
            feeaddress,
            tokenOwner,
            address(this) // service
        );

        emit onTokenCreatedEvent(
            tokenOwner,
            newTokenAddress,
            _NAME,
            _SYMBOL,
            _DECIMALS
        );

        return newTokenAddress;
    }

    function deployPaidByToken(
        string memory _NAME,
        string memory _SYMBOL,
        uint256 _DECIMALS,
        uint256 _supply,
        uint256 _txFee,
        uint256 _lpFee,
        uint256 _DexFee,
        address routerAddress,
        address feeaddress,
        address tokenOwner,
        address _tokenAddress
    ) external returns (address) {
        requireTokenActive(_tokenAddress);
        requireValidERC20Token(_tokenAddress);
        uint256 toPayment = getTokenPaymentAmount(_tokenAddress);
        requireSufficientTokenBalance(
            _tokenAddress,
            toPayment,
            "No tienes suficientes tokens"
        );
        IERC20 tokenContract = IERC20(_tokenAddress);
        requireTokenApproval(tokenContract, toPayment);
        require(
            tokenContract.transferFrom(_msgSender(), address(this), toPayment),
            "Error en transferencia de los tokens"
        );

        emit onTransferFromEvent(_msgSender(), address(this), toPayment);

        address newTokenAddress = createNewToken(
            _NAME,
            _SYMBOL,
            _DECIMALS,
            _supply,
            _txFee,
            _lpFee,
            _DexFee,
            routerAddress,
            feeaddress,
            tokenOwner
        );
        return newTokenAddress;
    }

    function deployPaidBNB(
        string memory _NAME,
        string memory _SYMBOL,
        uint256 _DECIMALS,
        uint256 _supply,
        uint256 _txFee,
        uint256 _lpFee,
        uint256 _DexFee,
        address routerAddress,
        address feeaddress,
        address tokenOwner
    ) external payable returns (address) {
        require(
            _msgValue() == servicePriceBNB,
            "No tienes suficiente saldo para pagar"
        );
        address newTokenAddress = createNewToken(
            _NAME,
            _SYMBOL,
            _DECIMALS,
            _supply,
            _txFee,
            _lpFee,
            _DexFee,
            routerAddress,
            feeaddress,
            tokenOwner
        );
        return newTokenAddress;
    }

    function withdrawToken(
        address _tokenAddress,
        uint256 _amount
    ) public onlyOwner {
        IERC20 tokenContract = IERC20(_tokenAddress);
        requireValidERC20Token(_tokenAddress);
        requireSufficientTokenBalance(
            _tokenAddress,
            _amount,
            "No hay suficientes tokens para retirar"
        );
        require(
            tokenContract.transfer(owner(), _amount),
            "Error al retirar tokens"
        );
        emit WithdrawalTokenSuccessful(owner(), _tokenAddress, _amount);
    }

    function withdrawBNB(uint256 _amount) public onlyOwner returns (bool) {
        require(
            address(this).balance >= _amount,
            "No hay suficiente BNB para retirar"
        );
        (bool success, ) = payable(owner()).call{value: _amount}("");

        if (success) {
            emit WithdrawalSuccessful(owner(), _amount);
            return true;
        } else {
            emit WithdrawalFailed(owner(), _amount);
            return false;
        }
    }

    function getBalanceToken(
        address _tokenAddress
    ) public view onlyOwner returns (uint256) {
        requireValidERC20Token(_tokenAddress);
        IERC20 token = IERC20(_tokenAddress);
        return token.balanceOf(address(this));
    }

    function getBalanceBNB() public view onlyOwner returns (uint256) {
        return address(this).balance;
    }
}