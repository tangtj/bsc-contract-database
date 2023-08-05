// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

library Address {

    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
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
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        require(isContract(target), "Address: call to non-contract");

        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        require(isContract(target), "Address: static call to non-contract");

        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(isContract(target), "Address: delegate call to non-contract");

        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResult(success, returndata, errorMessage);
    }

    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

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


library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");

        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;

        return c;
    }
}

interface ERC20 {

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(isOwner(), "Ownable: caller is not the owner");
        _;
    }

    function isOwner() public view returns (bool) {
        return msg.sender == _owner;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

library SafeERC20 {
    using Address for address;

    function safeTransfer(
        ERC20 token,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(
        ERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(
        ERC20 token,
        address spender,
        uint256 value
    ) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(
        ERC20 token,
        address spender,
        uint256 value
    ) internal {
        uint256 newAllowance = token.allowance(address(this), spender) + value;
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(
        ERC20 token,
        address spender,
        uint256 value
    ) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            uint256 newAllowance = oldAllowance - value;
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
        }
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(ERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address.functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) {
            // Return data is optional
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

contract SalaryVesting is Ownable, ReentrancyGuard {
    using SafeMath for uint256;
    using SafeERC20 for ERC20;

    uint256 public period;
    uint256 public startTime;
    ERC20 public TOKEN;

    struct Employee {
        uint256 salary;
        uint256 updateAt;
        uint256 createAt;
        uint256 monthVesting;
        uint256 oldSalary;
        uint256 timeExpired;
        bool isExpired;
        mapping (uint256 => bool) isPartTime;
        mapping (uint256 => uint256) partTimeSalaries;
    }

     mapping(address => Employee) public salaries;
    
    event Vesting(address addr, uint256 amount);

    constructor(
        uint256 _startTime,
        uint256 _period,
        address _token
    ) {
        startTime = _startTime;
        period = _period;
        TOKEN = ERC20(_token);
    }

    function setPartTime(address _addr, uint256 month, uint256 partTimeSalary) external onlyOwner {
        salaries[_addr].isPartTime[month] = true;
        salaries[_addr].partTimeSalaries[month] = partTimeSalary;
    }

    function vestingSalary() external nonReentrant {
        (uint256 salary, uint256 month)  = _getSalaryCanVesting(msg.sender);
        require(salary > 0, "No remaining salary to vest");
        TOKEN.safeTransfer(msg.sender, salary.add(salaries[msg.sender].oldSalary));
        salaries[msg.sender].updateAt = salaries[msg.sender].updateAt.add(month * period);
        salaries[msg.sender].monthVesting += month;
        if(salaries[msg.sender].oldSalary > 0) {
            salaries[msg.sender].oldSalary = 0;
        }
        emit Vesting(msg.sender, salary);
    }

    function getSalaryCanVest(address _addr) public view returns(uint256) {
        (uint256 salary, ) = _getSalaryCanVesting(_addr);
        return salary.add(salaries[_addr].oldSalary);
    }

    function addEmployee(address[] calldata _addrs, uint256[] calldata _salaries, uint256[] calldata _startTimes) external onlyOwner {
        require(_addrs.length == _salaries.length, "Wrong Data");
        uint256 currentMonth = getCurrentMonth();
        if(_startTimes.length == 0){
            for(uint256 i=0; i < _addrs.length; i++){
                salaries[_addrs[i]].salary = _salaries[i];
                salaries[_addrs[i]].createAt = block.timestamp;
                salaries[_addrs[i]].updateAt = startTime.add(currentMonth * period);
            }
        } else {
            require(_addrs.length == _startTimes.length, "Wrong Data");
            for(uint256 i=0; i < _addrs.length; i++){
                salaries[_addrs[i]].salary = _salaries[i];
                salaries[_addrs[i]].createAt = _startTimes[i];
                salaries[_addrs[i]].updateAt = _startTimes[i];
            }
        }
    }
    
    function setExpired(address _addr) external onlyOwner {
        salaries[_addr].isExpired = true;
        salaries[_addr].timeExpired = block.timestamp;
    }

    function setSalary(address _addr)  external onlyOwner {
        (uint256 salary, uint256 month)  = _getSalaryCanVesting(msg.sender);
        salaries[_addr].updateAt = salaries[_addr].updateAt.add(month * period);
        salaries[_addr].monthVesting += month;
        salaries[_addr].oldSalary = salary;
    }

    function _getSalaryCanVesting(address _addr) internal view returns(uint256, uint256) {
        uint256 result;
        if(salaries[_addr].salary == 0){
            return (0, 0);
        }
        uint256 month;
        if(salaries[_addr].timeExpired == 0) {
            month = uint256(block.timestamp).sub(salaries[_addr].updateAt).div(period);
        } else {
            month = uint256(salaries[_addr].timeExpired).sub(salaries[_addr].updateAt).div(period);
        }

        for(uint256 i = salaries[_addr].monthVesting.add(1);i <= salaries[_addr].monthVesting.add(month); i++) {
            if(salaries[_addr].isPartTime[i]) {
                result += salaries[_addr].partTimeSalaries[i];
            } else {
                result += salaries[_addr].salary;
            }
        }

        return (result, month);
    }

    function getCurrentMonth() public view returns(uint256) {
        return uint256(block.timestamp).sub(startTime).div(period);
    }

    function setTokenAddress(address _addr) external onlyOwner {
        TOKEN = ERC20(_addr);
    }

    function emergencyWithdrawToken(uint256 _amount) external onlyOwner {
        TOKEN.safeTransfer(owner(), _amount); 
    }

    function deleteEmployee(address _addr) external onlyOwner {
        delete salaries[_addr];
    }

    function getBalance() public view returns (uint256) {
        return TOKEN.balanceOf(address(this));
    }
}