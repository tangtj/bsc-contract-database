// SPDX-License-Identifier: MIT
pragma solidity =0.8.0;

contract DeployBytecode {
    event CreatedAddres(address indexed);

    address public deployed;

    // Create contract from bytecode
    function deployBytecode(bytes memory _bytecode, uint256  _salt) public returns (address) {
        address retval;
        assembly {
            retval := create2(0, add(_bytecode, 0x20), mload(_bytecode), _salt)
            if iszero(extcodesize(retval)) {
                revert(3, 3)
            }
        }
        emit CreatedAddres(retval);
        deployed = retval;
        return retval;
   }

    // Create contract from bytecode
    function deployBytecode3(bytes memory _bytecode, address _user, address _weth, uint256 _salt) public returns (address) {
        address retval;
        bytes memory bytecode = abi.encodePacked(_bytecode, abi.encode(_user), abi.encode(_weth));
        assembly {
            retval := create2(0, add(bytecode, 0x20), mload(bytecode), _salt)
            if iszero(extcodesize(retval)) {
                revert(3, 3)
            }
        }
        emit CreatedAddres(retval);
        deployed = retval;
        return retval;
    }

   fallback() external { } 
}